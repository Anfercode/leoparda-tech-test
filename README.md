
# Technical test - Leoparda Electric 🏍⚡💨

The solution of the Technical test made with 💚 by [Anfercode
](https://github.com/Anfercode)


## General

### Given a list of Nodes where each Node can have a list of Nodes, how would you visit all the Nodes?

What I would do is a recursive function that visit a list of nodes if a node in the list has a list of nodes, call the function recursive otherwise end the execution.

### Given two nodes in a tree, how would you calculate the distance between them?

I would find the common ancestor of each node then i calculate the distance of each node and the common ancestor node then i will add the 2 distances.

## Javascript

### What, if anything, is wrong with this:
```
const express = require( 'express' )
…
const app = express()
app.post( '/data', function( req, res, next ) => {
console.log( 'body', req.body )
try {
    callAsyncMethod( req.body, ( err, result ) => {
        console.log( 'error', err.status, err.message )
        if ( err ) {
            next( new Error( 'failed' ) )
        }
        res.json( { status : 200 } )
    } )
} catch ( ex ) {
    next( new Error( 'failed' ) )
}
} )
```
### Refactor:

```
const express = require( 'express' )
…
const app = express()
app.post( '/data', function( req, res, next ) => {
console.log('body', req.body)
try {
    callAsyncMethod( req.body, ( err, result ) => {
        if ( err ) {
            console.error( 'error', err.status, err.message )
            throw new Error( 'failed' )
        }
        res.status(200).json({ message : "service execute successfully"})
    } )
} catch ( ex ) {
    next( ex )
}
} )

function errorHandler(err, req, res, next){
    res.status(500).json({ err : err })
}

app.use(errorHandler)
```

### Pretend you had a src variable like the one below. Instead of making changes to the object directly, the code represents changes to variables by creating copies of them with the modification applied (like in flux or redux).

```
const src = {
    id : 12,
    name : 'Mr Wiggles',
    values : [ { x : 1, y : 3 }, { x : 100, y : 2.1 }, … ],
    alternates : { id : 16, alias : 'Wig' },
    history : [ … ]
}
```

#### How would you make a copy of src where the copy's name is 'Mrs Wiggles'?

i use the method `Object.assign({}, src);` to create a copy of the object and i can will change the name property.
```
const src = {
    id : 12,
    name : 'Mr Wiggles',
    values : [ { x : 1, y : 3 }, { x : 100, y : 2.1 }, … ],
    alternates : { id : 16, alias : 'Wig' },
    history : [ … ]
}

const src2 = Object.assign({}, src)
src2.name = 'Mrs Wiggles';

```

#### How would you make a copy of src where the copy's values[1].x is now 200?

I use the this:

const src2 = JSON.parse(JSON.stringify(src))

`src2.values[1].x = 200;`


### How do you delete an entry from an Object, like x from { x : 1, y : 2 }?

i would use the `delete` keyword operator to do this.

```
const values = { x : 1, y : 2 }
delete values.x
```

### How do you delete an item from an Array, like 2 from [1,2,3]?

i would use the `.pop` method if the delete is positional, if the delete is by value, first i use the method  `.indexOf` to know what is the position in the array of the value and use `.splice` to delete the specific value.

### What is the difference between the forEach and for-loop implementation of the following code to get device data given their unique identifier?

```
const axios = require( 'axios')
const devices = ["0x00077C98", "0x00078D38", "0x0007964F"]
const deviceDataArr = []
// For Each
devices.forEach((device, index) => {
    axios.get(`http://a.url.com/device/` + device )
    .then(res => {
        deviceDataArr[index] = res.data;
    })
    .catch(err => console.log(err))
})

// For Loop
for(var index=0; index=devices.length; index++){
    axios.get(`http://a.url.com/device/` + devices[index] )
    .then(res => {
        deviceDataArr[index] = res.data;
    })
    .catch(err => console.log(err))
}
```

The `forEach` will be executed asynchronous, `for-loop` will be executed sequentially, this can be slow the execution of the requests, because this will make the requests execute sequentially.

### How would you modify the above code to ensure that the data comes back in index order?

The `forEach` already return the data in the index order, the for loop is change the `var index=0` to `let index=0` to fix the scope.

### What, if anything, is wrong with this class:

```
class T {
    width() {
        return this.width
    }

    setWidth( w ) {
        this.width = w
    }
}
```

The name of the class is not ok, because isn't meen nothing, and the method `width` shoud be named `getWidth`.

## Database Questions

### What might the structure for a database containing customer, product, and transaction information look like?

[This link](https://drive.google.com/file/d/1fmOqFjZJgI0XlpowuwiICA8NjoLuaVh1/view?usp=sharing) is my proposal of the db design.

### How would you delete all but the 30 most recent rows from a table with the following structure?

```
create table example {
    id varchar(50),
    time timestamp,
    name text
}
```

With this query, can solve the problem: 
```
DELETE
FROM example
WHERE id NOT IN
    (SELECT id
     FROM example
     ORDER BY time DESC LIMIT 30);
```

### What, if anything, is wrong with the following code?

```
const mysql = require('mysql')
function queryDb(tableName, value, cb) {
    const connection = mysql.createPool({...})
    
    const query = 'Select * as everything from ' + tableName + ‘ where time < ’ + value

    let results = mysql.query(query, [tableName, value])
    
    return results
}
```
**Refactor:**

```
const mysql = require('mysql')

function queryDb(tableName, value) {
    const connection = mysql.createPool({...})
    
    const query = 'Select * from ? where time < ?;'
    const results = mysql.query(query, [tableName, value], (error, results, fields) => {
        if (error) throw error;
    });

    return results
}


```

## React/UI Questions

### What is wrong with the following React implementation. Give two solutions and discuss why one might be better:

```
class MyPane extends React.Component {
    constructor( props ) {
        super( props )
    }
    onButtonClick( ) {
        // do stuff
    }
    render( ) {
        return <Button onClick={this.onButtonClick}>;Click Me</Button>;
    }
}
```

the first argument is the bind of the `this` in the class component.

the second argument the component is not updated to the newest version of react.

this is my proposal for the following component.

```

const MyPane = props => {
    const handleClick = () => // do stuff
    return (
        <Button onClick={handleClick}>
             Click Me
        </Button>
    );
}


```