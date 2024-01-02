# Example Skeleton Main Program
```javascript
// hw.js
console.log('hello world!');
// run with: nodejs hw.js
```

# Comments
```javascript
//this is single line comment
/*
this is a 
multiline comment
*/
```

# Console Input/Output

## Output
```javascript
console.log("Hello World!");
```

## Input
```javascript
function hello(name) {
    console.log(`hello ${name}!`);
}

var readline = require('readline');
var rl = readline.createInterface({
  input: process.stdin
});

rl.on('line', function(line){
    hello(line);
})
```

# Namespaces
```javascript
// filename: ./mymodule.js
var mymodule = {
            info: function (info) { 
                console.log('Info: ' + info);
            },
            warning:function (warning) { 
                console.log('Warning: ' + warning);
            },
            error:function (error) { 
                console.log('Error: ' + error);
            }
};
module.exports = mymodule

// to use mymodule from another module
var myModule = require('./mymodule.js');
myModule.info('Node.js started');

// import specifying path where module is in a subfolder
var log = require('./utility/log.js');

```

# Classes

## Declaration
```javascript
class MyClassA {
  constructor(initBar) {
    this.Bar = initBar;
  }

  doSomething() {
    console.log("Hello "+this.Bar);
  }

  getSomething() {
    return 123;
  }
}

// can also be declared in an expression:
var MyClassA = class {
  constructor(initBar) {
  // ...
  }
};
```

## Inheritance
```javascript
class MyClassSubA extends MyClassA
{
        constructor()
        {
                //the super constructor is called using super()
                super("subA");
        }

        doSomething()
        {
                console.log("Hello "+this.Bar+" from SubA");
        }
}
```

## Instantiation and Method Invocation
```javascript
var mca = new MyClassA("1000");
mca.doSomething();
```

# Functions
```javascript
function MyFunction(arg1, arg2 = 'some default value') {
	return "returns: "+arg1+", "+arg2;
}

// async function
async function MyAsyncFunction() {
	var x = await someOtherAsyncFunction();
	return x;
}

// lambda function
var lambdatest = (x) => { x = x*x; return `lambdatest${x}`; }
console.log(lambdatest(3));
```

# Variables

- let -> limited to the scope of a block statement, or expression on which it is used
- var -> declares a variable globally, or locally to an entire function regardless of block scope.  The other difference between var and let is that the latter is initialized to a value only when a parser evaluates it (see below).
- const -> a const variable is still mutable but any object references cannot be changed to another object|null

## Declaration/Assignment
```javascript
// you don't specify the type:
var someInt = 123;
{
	let someOtherInt = 456;
} // someOtherInt cannot be referenced from outside the block

// the type of uninitialised variables (let/var) are 'undefined'
let ul; // typeof(ul) == 'undefined'
var uv; // typeof(uv) == 'undefined'

/*
Spread syntax (...) allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected. 
*/
function sum(x, y, z) {
  return x + y + z;
}
const numbers = [1, 2, 3];
console.log(sum(...numbers)); // output 6

// destructuring assignment
let a,b,rest;
[a, b, ...rest] = [10, 20, 30, 40, 50];
// a == 10
// b == 20
// rest == [30,40,50]

({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
// a == 10
// b == 20
// rest == {c: 30, d: 40}
```

## Ternary Operator
```javascript
// (evaluate bool expression result) ? <return_if_true> : <else_return_this> ;
var someVariable = (otherVariable > 0) ? 4535 : 0;

// can also use:
var someStringVal = otherStringVal || "default value";
```

## Global Variables

A var variable declared outside a function, becomes GLOBAL

```javascript
```

## Data Basic Types
```javascript
var my_int = 1;
typeof(my_int) == "number"  // true

var my_float = 1.0284;
typeof(my_float) == "number" // true

var my_bool = false or true;
typeof(my_bool) == "boolean" // true

var my_string = "test";
typeof(my_string) == "string" // true

var my_object = { name: "john", age: 52 };
typeof(my_object) == "object" // true

var my_function = function () { console.log('my_function'); };
typeof(my_function) == "function" // true

var some_var = null;

let car;    // uninitialised let value is undefined, type is undefined
var someVar = undefined; // uninitialised vars are null (object), however can be explicitly unset

var x = 0xF2; // hex literal
var x = 0b1101; // binary literal
```

## Data Basic Type Comparison
```javascript
$a == $b // juggle types and then compare if they are equal (beware of type confusion)
$a === $b // must be identical types and then compare if they are equal
$a != $b // juggle types, compare if they are equal and return if they are not equal (beware of type confusion)
$a <> $b // juggle types, compare if they are equal and return if they are not equal (beware of type confusion)
$a !== $b // must be identical types and then compare if they are not equal
$a > $b
$a >= $b
$a < $b
$a <= $b
```

## Logical Operators (Boolean Operands)
```javascript
a && b // AND: evaluates right operand only if necessary
a || b // OR: evaluates right operand only if necessary
!a    // NOT
```

# Data Structures

## array/list
```javascript
const my_list = ["one","two","three","four"];
const my_list = new Array("one","two","three","four") ; //alternative declaration
my_list[2] === "three"; // true
my_list[2] = "three"; // change an element

const list_slice = my_list.slice(2, 4); // slice(startIndexInclusive, untilBeforeIndex) ["three","four"]
my_list.push("five"); // my_list is now ["one","two","three","four","five"]
my_list.splice(0,0,"zero"); // insert element: splice(where, howManyToRemoveFirst, elementToAdd1, ....)
my_list.splice(2,1); // remove element 2, array is now ["zero","one","three","four","five"]
var combined_list = my_list.concat(["six","seven"]); // concatenate arrays

// check length
combined_list.length == 7 // true

// iteration
for(let i in combined_list)
	processValue(combined_list[i]);
  //OR...
combined_list.forEach( (element) => {
    processValue(element);
});

// check if value is in list
if(combined_list.includes('seven')) { //...
if(combined_list.find(x => x == 'six')) { //...
var itemindex = combined_list.indexOf("six"); // == 5, or -1 if not in list

// find all matching values in list
var all_matches = combined_list.filter(el => el.startsWith("s"));
```

## dictionary
A dictionary is just a POJO without any methods
```javascript
var my_dict = {};
my_dict["a"] = 1; // my_dict is now {'a':1}
my_dict["b"] = 2; // my_dict is now {'a':1, 'b':2}
my_dict["c"] = 3; // my_dict is now {'a':1, 'b':2, 'c':3}
delete my_dict["b"]; // my_dict is now {'a': 1, 'c': 3}

// iteration of keys
for (const key in my_dict) {
  processValue(key,my_dict[key]);
}

// get values in dictionary (returns array)
Object.values(my_dict);

// check if a key is in dictionary
if(my_dict.hasOwnProperty('c')) { //...

// check if a value is in dictionary
if(Object.values(my_dict).indexOf(1) >= 0) { //...
```

## set
```javascript
var my_set = new Set();
var my_set = new Set(['a','b']);
var set0 = new Set(["b", "c", "d"]);

my_set.add("d"); // my_set is now {"a","b","d"}
my_set.delete("a"); // my_set is now {"b","d"} 
my_set.add(["e", "f"]); // my_set is now {"b","d","e","f"}
// intersect can be simulated via
const intersection = new Set([...my_set].filter(x => set0.has(x))); //intersection is {"b","d"}
// difference can be simulated via
const difference = new Set([...my_set].filter(x => !set0.has(x))); // my_set is now {"c","e","f"}

// check if value is in set
if(my_set.has("b"))

// enumeration/iteration
my_set.forEach(x => processValue(x));
```

# Control Flow Statements

## if/else
```javascript
if (someVariable < 10) {
    doThis();
}
else if (someVariable < 20) {
    doThat();
}
else {
    handleFail();
}
```

## for
```javascript
for (let i = 0; i < 101; i++) {
    var x = doSomething(i);
	if (x == 123) {
	    continue;
	}
	else {
	    break;
	}
}

// can iterate where an enumerator is available
const fibNumbers = [1,2,3,5,8,13];
for (let element in fibNumbers) {
    console.log(`element: ${fibNumbers[element]}`);
}
```

## while
```javascript
while (someBooleanCondition) {
    var x = doSomething();
	if (x == 123) {
	    continue;
	}
	else {
	    break;
	}
}

// or do while...
do {
	do_this();
} while(someBooleanResult());

```

## switch/case
```javascript
switch(month) {
	case 1:
		monthString = "January";
		break;
	case 2:
		monthString = "February";
		break;
		…
	default:
		monthString = "Invalid";
}
```

# Exception Handling
```javascript
try {
	//...
}
catch (e) {
	MyLogger.logError(e.message);
}
finally {
	//cleanup();
}
```

# Numbers

## Operators
```javascript
let x = 10;
let y = 5;
x + y == 15
x - y == 5
x / y == 2
x * y == 50
Math.pow(x,y) == 100000 //exponent
y % x == 5  //modulo
```

## Absolute Value
```javascript
Math.abs(-x) == 10
```

## Min/Max
```javascript
Math.min(x,y) == 5
Math.max(x,y) == 10
```

## Floor/Ceiling
```javascript
let f = 3.634;
Math.floor(f) == 3
Math.ceil(f) == 4
```

## Convert Between Int/Float
JavaScript only has the one number type which represents float and int
```javascript
Math.round(3.0001) == 3 //true
Math.round(f) == 4 //true
x === 10.0 //true
```

## Parse Number from String
```javascript
let result = parseInt("27");
let dresult = parseFloat("27.89721");
```

## Random
```javascript
let ri = LOWLIMIT + Math.round(Math.random()*(HIGHLIMIT-LOWLIMIT)); // Math.random returns float 0 -> 1
// secure random only available in recent chrome and firefox
var buf = new Uint8Array(1);
window.crypto.getRandomValues(buf); // generate random byte
buf[0]
```

# Strings

## Literals
```javascript
let singleLineStr = "single line"; // can use single or double quotes
let multiLineStr = `multi line...
...string`;
```

## Concatenation
```javascript
str1 + str2
```

## Formatting
```javascript
let xi = 123;
let xs = "test";
let msg1 = `An int ${xi} and string ${xs}`; // msg1 == "An int 123 and string test";
let xf = 3.523463;
xf.toFixed(2); // "3.52"
xi.toString(16); // "7b"
xi.toString(2); // "1111011"
```

## Convert Types to a String Representation
```javascript
let svs = someVariable.toString();
`This will obtain a string representation of ${someVariable}`;
```

## Strip/Trim
```javascript
let xs = ",a,b,c,,";
let xst = xs.trim(); // will remove whitespace at both ends of the string
// javascript doesn't have a function that trims non-whitespace chars, use:
xs.replace(/^,+|,+$/gm,'');
```

## Split
```javascript
let x = ",a,b,c,,";
const tokenList = x.split(",");
// the above returns: ['','a','b','c','',''] 
```

## Replace
```javascript
let tx = x.replace(/,/g, ":"); // tx is ':a:b:c::'
let y = "The Trees";
let ty = y.replace(/Trees/g,"Forest"); // ty is "The Forest"
```

## StartsWith
```javascript
if (y.startsWith("The")){
```

## EndsWith
```javascript
if (y.endsWith("Forest")){
```

## Find a Substring
```javascript
y.indexOf("Trees") == 4 //find first occurrence of substring, returns -1 if not found
```

## Select a Substring
```javascript
y = y.substring(4); // slice from 4th character to the end of the string, returns: "Trees"
y = y.substring(4,6); // (startIndex, endBeforeIndex), returns: "Tr"
```

## Regex
```javascript
let test = "think this is a test string. that.";
const matches = test.match(/[a-z]+/g); // returns ['think', 'this', 'is', 'a', 'test', 'string', 'that']

// match multiple groups

let line = "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)";
let matchgroups = line.match(/([0-9\\.]+).+GET (.+) HTTP/);
matchgroups[0] // the full match with all groups
matchgroups[1] // "203.4.25.66"
matchgroups[2] // "/index.php"
```

# Binary Bytes/Buffers

## Declaration
```javascript
byte key = (byte)0x65; //hex literal are ints so must be cast
byte val = (byte)0b01101101;     //binary literal are ints so must be cast
byte[] header = {(byte)0xFF,(byte)0xC3,(byte)0x00,(byte)0x00}; // hex literals are ints so must be cast
char key = 0x00BC; //Char type is interpreted as unsigned 16bit value (unicode char)
byte[] data = "<h1>Hello World</h1>".getBytes(); //convert string to UTF8 bytes

let key = 0x65;
let val = 0b01101101;
var buffer = Buffer.alloc(16); // initialized with zeroes x 16
var buffer = Buffer.from([0xFF,0xC3,0x00,0x00]);
var buffer = Buffer.from("<h1>Hello World</h1>", "utf-8");

const fs = require('fs');
fs.writeFileSync('message.dat', buffer);
fs.writeFile('message.dat', buffer, (err) => { if(err) console.log(err); console.log('wrote file'); } );
```

## Bytearray/Buffer Operations
```javascript
var buffer = Buffer.alloc(16);
buffer[3] = 0xFF;
let numbyteswritten = buffer.writeUInt8(0x25); // (valueToWrite[,offset=0])
numbyteswritten = buffer.writeDoubleLE(354.927792821,numbyteswritten);
numbyteswritten = buffer.writeInt32LE(293842,numbyteswritten);
// see Buffer.write*() methods for other types
buffer = Buffer.concat([buffer,Buffer.from([0xD3,0xD4,0xD5,0xD6])]);

let outValue = buffer.readUInt8(0); // read*(offset)
let bufAsString = buffer.toString('utf8');
const bArr = buffer.toJSON().data; // as raw byte value array eg. [92, 115, 196, 74, 33, ...
```

## Bitwise Operations 

```javascript
// bytes are treated as signed int, eg for shift operations
// chars are treated as unsigned
let a = 0b11011001;
let b = 0b10011101;
```

### And
```javascript
// a & b
(a & b) == 0b10011001
```

### Or
```javascript
// a | b
(a | b) == 0b11011101
```

### Xor
```javascript
// a ^ b
(a ^ b) == 0b01000100
```

### Not
```javascript
// a ^ 0xFF
(a ^ 0xFF) == 0b00100110
```

### Shift Left/Right
```javascript
// a >> 4, or a << 5
(a >> 4) == 0b00001101
```

# CLI Argument Parsing
```javascript
//the entire command line is in the argument list, so args[0] is usually node|nodejs, args[1] is the scriptname etc.
// eg. node HelloWorld.js firstarg
// prints: "firstarg"

if (process.argv.length < 3)
  return;
console.log("Args[2]:"+process.argv[2]); 

```

# Execute Shell Commands
```javascript
const { exec } = require("child_process");

exec("ls -la", (error, stdout, stderr) => {
    if (error) {
        console.log(`error: ${error.message}`);
        return;
    }
    if (stderr) {
        console.log(`stderr: ${stderr}`);
        return;
    }
    console.log(`stdout: ${stdout}`);
});
```

# File Operations
```javascript
// file modes
```
## Open File and Read
```javascript
const fs = require('fs');

// reads the entire file as a string into (load entire file into memory - may not be efficient for large files)

try {
  const data = fs.readFileSync('/Users/joe/test.txt', 'utf8')
  console.log(data)
} catch (err) {
  console.error(err)
}

// async file read via stream, process a chunk at a time

let stream = fs.createReadStream(fileName);
    stream.on("error", err => console.log(err));
    stream.on("data", chunk => processFileChunk(chunk));
    stream.on("end", () => processCompleteData());
	
// read binary data (also see binary bytearray/buffer operations section above)

try {
	const fdata = fs.readFileSync('/Users/joe/test.dat')  // the encoding is left as null to read as binary
	let fileContent = fdata.toJSON().data
	for(let bx in fileContent)
		console.log(`${fileContent[bx].toString(16)}`)
} catch (err) {
	console.error(err)
}

```

## Open File and Write
```javascript
const fs = require('fs');

// write entire string to file

fs.writeFileSync('/path/to/message.dat', "A string of characters");

// write lines to file:

var arrayOfLines = ["First line", "Second line", "Third line"];
fs.writeFileSync('/path/to/message.dat', arrayOfLines.join("\n"));
fs.writeFile('/path/to/message.dat', arrayOfLines.join("\n"), (err) => { if(err) console.log(err); console.log('wrote file'); } );

// write binary data

var buffer = Buffer.from([0xFF,0xC3,0x00,0x00]);
fs.writeFileSync('/path/to/message.dat', buffer);
fs.writeFile('/path/to/message.dat', buffer, (err) => { if(err) console.log(err); console.log('wrote file'); } );

// appending to existing file content:

fs.appendFileSync('/path/to/message.dat', "\nA string of characters to append to existing content");
```

## Skip To File Location
The read() function has an offset argument (relative from start of file)
```javascript
let fs = require('fs');
const BYTES_TO_READ = 16;
const OFFSET_IN_FILE = 20;
const BUFFER_OFFSET = 0;

fs.open('/path/to/file', 'r', function(err, fileToRead){
    if (!err){
		const BYTES_TO_READ = 16;
		const OFFSET_IN_FILE = 20;
		const BUFFER_OFFSET = 0;
		let buffer = Buffer.alloc(BYTES_TO_READ);
        fs.read(fileToRead, buffer, BUFFER_OFFSET, BYTES_TO_READ, OFFSET_IN_FILE, function(err,data){
            if (!err){
				console.log('received data: ' + buffer.toString());
            }else{
                console.log(err);
            }
        });
    }else{
        console.log(err);
    }
	fs.close(fileToRead, () => null);
});

```

## Close File
```javascript
fs.close(fileHandle, () => null); // callback (err)
```

# Networking

## Sockets
```javascript
// client send/receive

var net = require('net');

var client = new net.Socket();
client.connect(1337, '10.3.3.250', function() {
        console.log('Connected');
        client.write('Hello, server! Love, Client.');
});

client.on('data', function(data) {
        console.log('Received: ' + data);
        client.destroy(); // kill client after server's response
});

client.on('close', function() {
        console.log('Connection closed');
});

// server listen/reply

var net = require('net');

var server = net.createServer(function(socket) {
    socket.on('data', function(data) {
        console.log('Received: ' + data.toString());
        socket.write('Echo server\r\n'+data.toString());
    });
});
server.listen(1337, '0.0.0.0');
```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client
```javascript
var http = require('http');

//The url we want is: 'www.random.org/integers/?num=1&min=1&max=10&col=1&base=10&format=plain&rnd=new'
var options = {
  host: '10.3.3.250',
  port: 8098,
  path: '/integers/?num=1&min=1&max=10&col=1&base=10&format=plain&rnd=new',
  method: 'GET',
  headers: {'custom': 'Custom Header Demo works'}
};

callback = function(response) {
  var str = '';

  //another chunk of data has been received, so append it to `str`
  response.on('data', function (chunk) {
    str += chunk;
  });

  //the whole response has been received, so we just print it out here
  response.on('end', function () {
	console.log(response.headers['server']);
    console.log(str);
  });
}

http.request(options, callback).end();
```

### HTTP server
```javascript
const http = require('http');

const requestListener = function (req, res) {
  let h_ua = req.headers['user-agent'];
  let h_p = req.url;
  console.log(`ua: ${h_ua}, url: ${h_p}`);
  res.writeHead(200);
  res.end('Hello, World!');
}

const server = http.createServer(requestListener);
server.listen(8080);
```

# JSON Serialization
 
## Serialize
```javascript
let ob = {"a":123,"b":"test","c":[1,2,3]};
let obstr = JSON.stringify(ob);
```

## Deserialize
```javascript
let jstr = '{"a":123,"b":"test","c":[1,2,3]}';
let ob = JSON.parse(jstr);
console.log(ob['b']);
```

# Object Introspection
```javascript
class MyClassA {
  a = 123;
  constructor(initBar) {
    this.Bar = initBar;
  }

  doSomething() {
    console.log("Hello "+this.Bar);
  }

  getSomething() {
    return 123;
  }
}

var mca = new MyClassA();
Object.getOwnPropertyNames(mca); // ['a','Bar']
Object.getOwnPropertyNames(MyClassA.prototype); // ['constructor', 'doSomething', 'getSomething']
Object.getPrototypeOf(mca); // {constructor: ƒ, doSomething: ƒ, getSomething: ƒ}

typeof MyClassA;             // == "function"
typeof mca;             // == "object"

mca instanceof MyClassA;     // == true
mca.constructor.name;   // == "MyClassA"
MyClassA.name                // == "MyClassA"    
```

# Date/Time
```javascript
var now = new Date().getTime(); // epoch time milliseconds
var isoNow = now.toISOString(); // 2021-10-30T12:13:03.898Z
```

# Sleep
```javascript
// On Node 7.6.0 or higher
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function demo() {
  await sleep(2000);
}
```

# Multithreading

Any node program is always a single threaded program (in user-space). Node 10.x introduced the worker_threads module as an experimental feature. It became stable as of 12.x*x
https://levelup.gitconnected.com/multithreading-with-nodejs-is-reality-10871986b8a9

TODO: need to include Promises, async etc.

## Threads
```javascript
// Async functions (single-threaded)
async function asyncCall() {
  const result = await someOtherAsyncFunction();
}
asyncCall();

// Multithreading using workers

// thread-entry.js:
// console.log("Hello world");

// index.js
const { Worker } = require("worker_threads");(function () {
  new Worker("./src/thread-entry.js")
})()
```

## Synchronization/Locking Primitives
```javascript
// refer to the implementation here https://levelup.gitconnected.com/multithreading-with-nodejs-is-reality-10871986b8a9
```
