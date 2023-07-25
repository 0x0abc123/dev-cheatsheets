# Example Skeleton Main Program
```
<?php
include 'something.php';
echo "hello PHP";
?>
```

# Comments
```
//this is single line comment
/*
this is a 
multiline comment
*/
```

# Console Input/Output

## Output
```
echo "Hello World!";
echo("Hello World!");
print("Hello World!");
```

## Input
```
$input = readline("Enter a line: ");
```

# Namespaces
Create `mynamespace.php` (doesn't have to be named to match though):
```
<?php namespace mynamespace;
  class MyClassA {
    static function says() {echo 'fooooo';}  
  } 
?>
```
Then you can import it from another class using:
```
include 'mynamespace.php';
\mynamespace\MyClassA::says();
```

# Classes

## Declaration
```
<?php namespace myappmodels;
  class MyClassA {
	public $Name;
	protected $Bar;
	
	function __construct($initBar = "defaultValue") {
		$this->Bar = $initBar;
	}
	
    function DoSomething() {
		echo("Hello $this->Bar");
	}
	
	function GetSomething() {
		return $this->Bar;
	}
  } 
?>
```

## Inheritance
```
<?php namespace myappmodels;
include 'myclassA.php';

  class MyClassSubA extends MyClassA {
	protected $SubBar;
	
	function __construct() {
		parent::__construct("subA");
		$this->SubBar = "test";
	}
	
    function DoSomething() {
		echo("Hello $this->Bar from SubA");
	}
  } 
?>
```

## Instantiation and Method Invocation
```
include 'myclassA.php';
$mca = new myappmodels\MyClassA("1000");
$mca->DoSomething();
```

# Functions
A function can be locally scoped or a static function in a class
```
// local scope function
function MyFunction($arg1, $arg2 = "default value")
{
	return "MyFunction $arg1 $arg2";
}
// call like this...
$s = MyFunction("msf")

// static function in a class
class FunctionModule
{
	public static function MyStaticFunction($arg1, $arg2 = "default value")
	{
		return "MyStaticFunction $arg1 $arg2";
	}
}
// call like this...
$s = FunctionModule::MyStaticFunction("msf")

// lambda function
$greet = function($name) {
    printf("Hello %s\r\n", $name);
};
// call like this...
$greet('World');
```

# Variables

## Declaration/Assignment
```
// created once a value is assigned, need to prefix with "$":
$someInt = 123;
```

## Ternary Operator
```
// (evaluate bool expression result) ? <return_if_true> : <else_return_this> ;
$someVariable = ($otherVariable > 0) ? 4535 : 0;
```

## Global Variables
All PHP variables generally only have a single (global) scope. This single scope includes included and required files
An exception is where the variable scope is local eg. in a function or class
```
<?php
$a = "something";
include 'b.inc';
?>

// somewhere in the 'b.inc' code...
$a = "changed";
```

## Data Basic Types
```
$my_int = 1; //between -2,147,483,648 and 2,147,483,647
$my_float = 3.25;
$my_bool = false || true;
$something = null;
```

## Data Basic Type Comparison
```
$a == $b // juggle types and then compare if they are equal (beware of type confusion)
$a === $b // must be identical types and then compare if they are equal
$a != $b // juggle types, compare if they are equal and return if they are not equal (beware of type confusion)
$a <> $b // juggle types, compare if they are equal and return if they are not equal (beware of type confusion)
$a !== $b // must be identical types and then compare if they are not equal
$a > $b
$a >= $b
$a < $b
$a <= $b
$a <=> $b // returns -1 if a < b, 0 if a == b, 1 if a > b8
```

## Logical Operators (Boolean Operands)
```
$a and $b
$a && $b // AND
$a or $b
$a || $b // OR
$a xor $b // true if a or b is true but not both, false if a and b are the same
!a  // NOT
```

# Data Structures

## array
```
$ia = array(1,2,3);
$sa = array("one","two","three","four");

// reference an element
$ia[1] == 2

// slice
$list_slice = array_slice($sa, 2, 1); // (array, startIndex, numItems)

// append
array_push(sa, "five"); // sa is now {"one","two","three","four","five"}
$sa[] = "five"; // is an alternative, more efficient way of doing the above

// insert
array_splice( $sa, 0, 0, array("zero") ); // (array, insertBeforeIndex, 0, valueToInsert)

// extend (use splice, which allows another array to be added)
array_splice( $sa, count($sa), 0, array("six","seven") ); // (array, arrayLength, 0, arrayToAppend)

// check length
count($sa);

// iteration
foreach($sa as $element) {
    processValue($element);
}

// check if value is in list (basic data types)
if(in_array("six", $sa)) {
```

## dictionary
An Associative Array is the PHP equivalent of a dictionary
```
$my_dict = array();
$my_dict = array("a"=>1,"b"=>2);
$my_dict["c"] = 3; // my_dict is now {'a':1, 'b':2, 'c':3}

// remove key/value
unset($my_dict['b']);

// iteration of key value pairs enumerator returns Key Value Pairs
foreach($my_dict as $key => $value) {
   processValue($key,$value);
}

// check if a key is in dictionary
if (array_key_exists("a",$my_dict)) // true
```

## set
PHP doesn't have a builtin Set data structure, you can use the DS extension (https://www.php.net/manual/en/book.ds.php), or emulate one using an Associative Array
```
use Ds\Set;
$numbers = new Set([1, 2, 3]);

// emulate using associative array with placeholder values
$my_set = array();

// add
$my_set["a"] = 1;

// remove
unset($my_set['b']);

// check if member exists
if (array_key_exists("a",$my_set)) {

// iteration of members
foreach($my_set as $key => $value) {
   processValue($key);
}
```

# Control Flow Statements

## if/else
```
if ($someVariable < 10) {
    doThis();
}
elseif ($someVariable < 20) {
    doThat();
}
else {
    handleFail();
}
```

## for
```
for ($i = 0; i < 101; $i++) {
    $x = doSomething($i)
	if ($x == 123) {
	    continue;
	}
	else {
	    break;
	}
}

// can iterate over arrays with foreach, see above
```

## while
```
while ($someBooleanCondition) {
    $x = doSomething();
	if ($x == 123) {
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
```
switch($month) {
	case 1:  // execute if $month == 1
		$monthString = "January";
		break;
	case 2:
		$monthString = "February";
		break;
		…
	default:
		$monthString = "Invalid";
}
```

# Exception Handling
```
try {
	//...
	if($divisor == 0) {
       throw new Exception("Division by zero");
    }
	//...
}
catch (Exception $e) {
	$e_code = $e->getCode();
	$e_msg = $e->getMessage();
	$e_file = $e->getFile();
	$e_line = $e->getLine();
	$MyLogger->logError("$e_code, $e_msg, $e_file, $e_line\n");
}
finally {
	//cleanup();
}
```

# Numbers

## Operators
```
$x = 10;
$y = 5;
$x + $y == 15
$x - $y == 5
$x / $y == 2
$x * $y == 50
$x ** $y == 100000 //exponent
$y % $x == 5  //modulo

// in-place operators can be used, eg.:
$x += 1; // x = x + 1
$x *= 4; // x = x * 4

// increment/decrement can be used (similar to C/C++ with pre and post):
$x++; // return result then increment by 1
$--x; // decrement by 1 then return result

```

## Absolute Value
```
$y = abs($x);
```

## Min/Max
```
min($x,$y) == 5
max($x,$y) == 10

// also support an array (non-associative) argument
$maxval = max($array_of_ints);
```

## Floor/Ceiling
```
$f = 3.634;
floor($f) == 3
ceil($f) == 4
```

## Convert Between Int/Float
```
(int)$f // cast
round($f) == 3  // float to int

(double)$someint // cast
$x = 10; $x == 10.0 // true, PHP will convert int to float as required
```

## Parse Number from String
```
$intresult = (int) "27";
$intresult = intval("27");

$dresult = (double) "27.89721";
$dresult = floatval("27.89721");
```

## Random
```
$r = random_int(1,10); // this is a cryptographically secure function
```

# Strings

## Literals
```
$singleLineStr = "single line"; // can use single or double quotes
$multiLineStr = "multi line...
...string
";
// heredoc form:
$multiLineStr = <<<MLS
multi line...
...string
MLS;
```

## Concatenation
```
$str1.$str2
$str1 .= $str2 ;
```

## Formatting
```
$msg0 = "Something $str1 and string {$x->foo()}"; // use curly braces as delimiter for complex expressions
$msg1 = sprintf("An int %d and string %s",123,"test"); // msg1.Equals("An int 123 and string test");
$msg2 = sprintf("Formatting examples, an int %0d, a float to 3 places %.3f, and hex %2X, binary %08b", 3, 1045.89735, 0x2F, 0x2F);
//$msg2 === "Formatting examples, an int 3, a float to 3 places 1045.897, and hex 2F, binary 00101111"
```

## Convert Types to a String Representation
```
$svs = print_r($someObject,true);
$svs = "This will obtain a string representation of {$someObject}";
```

## Strip/Trim
```
$xs = ",a,b,c,,";
$xst = trim($xs); // will remove whitespace at both ends of the string
$xst = trim($xs, ",:"); // will remove the specified characters from both ends, in this case the output is 'a,b,c'
$xst = ltrim($xs, ","); // will remove the specified characters from start, in this case the output is 'a,b,c,,'
$xst = rtrim($xs, ","); // will remove the specified characters from end, in this case the output is ',a,b,c'
```

## Split
```
$xs = ",a,b,c,,";
$tokenList = explode(",", $xs); // empty strings will be included as elements, in this case result is [,'a','b','c',,]
```

## Replace
```
$tx = str_replace(",",":",$xs); // tx is ':a:b:c::'
$y = "The Trees";
$ty = str_replace("Trees","Forest",$y); // ty is "The Forest"
```

## StartsWith
```
if (strpos($y, "The") === 0) {
```

## EndsWith
```
$check = "Trees";
if (strpos($y, $check) === strlen($y) - strlen($check)) {
```

## Find a Substring
```
strpos($y, "Trees") === 4 //find first occurrence of substring, returns false if not found
```

## Select a Substring
```
$oy = substr($y,4); // slice from 4th character to the end of the string, returns: "Trees"
$oy = substr($y,4,2); // (startIndex, lengthOfSlice), returns: "Tr"
```

## Regex
```	 
// find simple pattern, get list of all occurrences

$matches = array();
if(preg_match_all("/[a-z]+/", "one two three four 5 6 seven", $matches) > 0)
	foreach($matches[0] as $m) { //$matches is an array where array[0] is the array of matches
		print("m: $m\n");
	}

// match multiple groups

$line = "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)";
// use named groups eg. 'ipaddr' and 'url':
preg_match('/^(?<ipaddr>[0-9\\.]+).+GET (?<url>.+) HTTP/', $line, $matches);
print("{$matches['ipaddr']} {$matches['url']}\n");
```

# Binary Bytes/Buffers

## Declaration
```
$key = 0x65; //hex literal
$key = bindec('01100101'); //from binary string
$header = array(0xFF,0xC3,0x00,0x00);
$header = pack("C*", 0xFF,0xC3,0x00,0x00); // creates a binary string from the bytes, see below for the format codes
$char_key = chr($key); // convert 0x65 to 'e'

$byte_array = unpack('C*', '<h1>Hello World</h1>'); //convert string to associative array of int, eg. 1 => 0x3C
```

## Bytearray/Buffer Operations
```
// see https://www.php.net/manual/en/function.pack.php for full reference, but some common pack/unpack codes are:
// C : unsigned char
// S : unsigned short (16bit, machine byte order)
// n : unsigned short (16bit, big endian)
// v : unsigned short (16bit, little endian)
// L : unsigned long (32bit, machine byte order)
// N : unsigned long (32bit, big endian)
// V : unsigned long (32bit, little endian)
// * : repeat the last size until the end of input

$binarydata = pack("nvc*", 0x1234, 0x5678, 65, 66); //The resulting binary string will be the byte sequence 0x12,0x34,0x78,0x56,0x41,0x42
$byte_array = unpack('C*', $binaryinput); //convert binary input data to associative array of int (1-based, not 0-based indexing), eg. 1 => 0x3C
```

## Bitwise Operations 

```
$a = bindec("11011001"); //0xD9
$b = bindec("10011101"); //0x9D
```

### And
```
// a & b
sprintf("%08b\n",($a & $b)) === "10011001"
```

### Or
```
// a | b
sprintf("%08b\n",($a | $b)) === "11011101"
```

### Xor
```
// a ^ b
sprintf("%08b\n",($a ^ $b)) === "01000100"
```

### Not
```
// a ^ 0xFF
sprintf("%08b\n",($a ^ 0xFF)) === "00100110"
```

### Shift Left/Right
```
// a >> 4, or a << 5
sprintf("%08b\n",($a >> 4)) === "00001101"
```

# CLI Argument Parsing
```
// $argv[0] is the script name
// root@localhost:/# php test2.php blah --test=123
print_r($argv);
/*
Array
(
    [0] => test2.php
    [1] => blah
    [2] => --test=123
)
*/
```

# Execute Shell Commands
```
$cmd_output = shell_exec("ls / -ltr"); // returns null if an error occurred
$last_line_of_cmd_output = exec("ls / -ltr", $array_all_output_lines, $exit_code); // also returns null if an error occurred
system("ls / -ltr"); // prints output to stdout
```

# File Operations
```
// file modes
/*
r 	Open a file for read only. File pointer starts at the beginning of the file
w 	Open a file for write only. Erases the contents of the file or creates a new file if it doesn't exist. File pointer starts at the beginning of the file
a 	Open a file for write only. The existing data in file is preserved. File pointer starts at the end of the file. Creates a new file if the file doesn't exist
x 	Creates a new file for write only. Returns FALSE and an error if file already exists
r+ 	Open a file for read/write. File pointer starts at the beginning of the file
w+ 	Open a file for read/write. Erases the contents of the file or creates a new file if it doesn't exist. File pointer starts at the beginning of the file
a+ 	Open a file for read/write. The existing data in file is preserved. File pointer starts at the end of the file. Creates a new file if the file doesn't exist
x+ 	Creates a new file for read/write. Returns FALSE and an error if file already exists
*/
```
## Open File and Read
```
$filename = "test.txt";
$myfile = fopen($filename, "r") or die("Unable to open file!");

// Read the file as one string.

$filelines_as_one_string = fread($myfile, filesize($filename)); // 2nd arg to fread is how many bytes to read
fclose($myfile);

// Read each line of the file into a string array. Each element of the array is one line of the file.

$filelines = array(); // this will contain each line as a separate array element
while(!feof($myfile)) {
 $filelines[] = fgets($myfile); // append each fileline to the array, moves the file pointer to next line
}
fclose($myfile);

// read all lines at once, $file_r is an array of lines, don't have to open or close the file

$file_r = file($filename);


// read binary
$bindata = fread($myfile, 6); // 2nd arg to fread is how many bytes to read
$byte_array = unpack('C*', $bindata); //convert binary input data to associative array of 6 bytes
```

## Open File and Write
```
$filename = "test.txt";
$myfile = fopen($filename, "w") or die("Unable to open file!");

// Write one string at a time to a file (append a newline manually):
// or just pack the entire file contents into one string and write it all at once

fwrite($myfile, "$text\n");
fclose($myfile);

// write binary data

$binarystring = pack("C*", 0x12, 0x34, 0x56, 0x78, 0x65, 0x66);
fwrite($myfile, $binarystring);
fclose($myfile);
```

## Skip To File Location
```
// fseek(resource $stream, int $offset_num_bytes, int $whence = SEEK_SET): int
// 'whence' is optional and specifies positioning
// default is SEEK_SET == absolute file positioning
// SEEK_CUR == seek relative to the current position 
// SEEK_END == seek relative to the file's end.

fseek($fp, 0); // move filestream pointer to beginning of file
fseek($fp, 8, SEEK_CUR); // move filestream pointer 8 bytes from current position

```

## Close File
```
fclose($myfile);
```

# Networking

## Sockets
```
// client send/receive

$host    = "192.168.1.1";
$port    = 8089;
$message = "GET / HTTP/1.1\r\nHost: localhost\r\n\r\n";
$socket = socket_create(AF_INET, SOCK_STREAM, 0) or die("Could not create socket\n");
$result = socket_connect($socket, $host, $port) or die("Could not connect to server\n");
socket_write($socket, $message, strlen($message)) or die("Could not send data to server\n");
$result = socket_read ($socket, 1024) or die("Could not read server response\n");
echo "Reply From Server  :".$result;
socket_close($socket);


// server listen/reply

// set some variables
$host = "0.0.0.0";
$port = 2500;
// don't timeout!
set_time_limit(0);
$socket = socket_create(AF_INET, SOCK_STREAM, 0) or die("Could not create socket\n");
$result = socket_bind($socket, $host, $port) or die("Could not bind to socket\n");
$result = socket_listen($socket, 3) or die("Could not set up socket listener\n");
$spawn = socket_accept($socket) or die("Could not accept incoming connection\n");
$input = socket_read($spawn, 1024) or die("Could not read input\n");
$input = trim($input);
echo "Client Message : ".$input;
$output = strrev($input) . "\n";
socket_write($spawn, $output, strlen ($output)) or die("Could not write output\n");
socket_close($spawn);
socket_close($socket);
```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client
```
//PHP builtin HTTP request wrapper can only make GET HTTP/1.0 requests
$opts = [
    'https' => [
        'max_redirects' => 3,
    ],
];
$queryString = http_build_query([
    'api_key' => $apiKey,
    'safe_search' => 1,
    'text' => 'Kakadu National Park'
]);
$requestUri = sprintf(
    'http://192.168.3.50:8089/rest/?%s',
    $queryString
);
$fp = fopen($requestUri, 'r');
$responseData = stream_get_contents($fp);
print($responseData."\n");

//if cURL extension is enabled (it often is), it is more flexible, as above except instead of fopen use:
$ch = curl_init();
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_URL, $requestUri);
curl_setopt($ch, CURLOPT_SSH_COMPRESSION, true);
curl_setopt_array($ch, [
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_URL => $requestUri
]);
$responseData = curl_exec($ch);
print($responseData."\n");

```

### HTTP server
```
<?php
// router.php
if (preg_match('/\.(?:png|jpg|jpeg|gif)$/', $_SERVER["REQUEST_URI"])) {
    return false;    // serve the requested resource as-is.
} else { 
    echo "<p>Welcome to PHP</p>";
}
?>

root@localhost:~/# php -S localhost:8000 router.php
```

# JSON Serialization

## Serialize
```
$arr = array('a' => array(1,2,3), 'b' => 'test');
print(json_encode($arr));
```

## Deserialize
```
$jsonstr = '{"a": [4, 5, 6], "b": "foo"}';
$jsonobj = json_decode($jsonstr);
```

# Object Introspection
```
$typestring = gettype($var);

if(class_exists("MyClass")) {

$classname = get_class($objInstance);

//get_class_methods() – returns the names of the class methods as an array
$methods_array = get_class_methods($objInstance);

//get_class_vars() – returns the default properties of a class
$vars_array = get_class_vars($objInstance);

//interface_exists() – checks whether the interface is defined
if(interface_exists("MyInterface")) {

//method_exists() – checks whether an object defines a method
if(method_exists($objInstance, "GetSomething")) {
```

# Date/Time
```
//format will be 2021-10-11T01:06:03 +1100
$datestr = date("Y-m-d\Th:i:s O");

//epoch time format in seconds
$unixTimeMilliseconds = date("U");

//generate a date (unix timestamp) from a string
$d=strtotime("2014-04-15 10:30:00");
echo "$d\n"; // 1397521800
echo "Created date is " . date("d/m/Y h:i:sa", $d);
```

# Sleep
```
sleep(5); // seconds (int)
```

# Multithreading

Multi threading is possible using extensions like pthreads, but only in CLI apps, not in a webserver process
