0~# Example Skeleton Main Program
```
using System;

namespace dotnettmp1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello {0}!","World");
        }
    }
}
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
Console.WriteLine("Hello World!");
```

## Input
```
string i = Console.ReadLine();
```

# Namespaces
Create `MyClassA.cs` (doesn't have to be named to match though):
```
namespace myapp.Models
{
    class MyClassA
    {
		//...
    }
}
```
Then you can import it from another class using:
```
using myapp.Models;
MyClassA mca = new MyClassA();
```

# Classes

## Declaration
```
namespace myapp.Models
{
    public class MyClassA
	{
        protected string Bar = "";

		public MyClassA(string initBar = "foo")
		{
			this.Bar = initBar;
		}

		// instead of virtual, can be abstract if implementation in subclasses is required
		// classes containing any abstract methods need to be declared as abstract
		public virtual void DoSomething()
		{
			Console.WriteLine("Hello {0}",this.Bar);
		}

		public override int GetSomething()
		{
			return 123;
		}
	}
}
```

## Inheritance
```
namespace myapp.Models
{
    public class MyClassSubA : MyClassA
	{
		public MyClassSubA() : base("subA")
		{
			//the super constructor is called using base()
		}
		
		public override void DoSomething()
		{
			Console.WriteLine($"Hello {this.Bar} from SubA");
		}
	}
}


```

## Instantiation and Method Invocation
```
MyClassA mca = new MyClassA("1000");
mca.DoSomething();
```

# Functions
A function must be a static function in a class
```
public class FunctionModule
{
	public static string MyStaticFunction(string arg1, string arg2 = "default value")
	{
		return $"MyStaticFunction {arg1} {arg2}";
	}
}

// lambda function
// Func<return_type, arg_type, ...>
// shorthand:
Func<int, int> LOneArgument = x => x * x;
// longhand:
Func<int, int> LOneArgument = (x) => { return x * x ; };
Console.WriteLine(LOneArgument(5)); // prints 25
// no parameters...
Func<int> LNoArguments = () => { return 15 * 5 ; };
Console.WriteLine(LNoArguments()); // prints 75		
```

# Variables

## Declaration/Assignment
```
// var is a generic value whose type will be determined at compile time
var someVariable = 123;
// can explicitly set the type:
int someInt = 123;
```

## Ternary Operator
```
// (evaluate bool expression result) ? <return_if_true> : <else_return_this> ;
int someVariable = (otherVariable > 0) ? 4535 : 0;
```

## Global Variables
A global variable must be a static member of a class
```
public class FunctionModule
{
	public static string SOME_VALUE = "global";
}

// somewhere in the code...
FunctionModule.SOME_VALUE = "changed";
```

## Data Basic Types
```
int my_int = 1;
short s = 123;
long l = 2189738723L;

double d = 3.25D;
float f = 3.234F;

bool my_bool = false || true;

byte b = 0x23;
char c = 'a'; //unicode

var something = null;
```

## Data Basic Type Comparison
```
a == b // compare if they are equal (both must be the same type)
a != b // compare if they are not equal (both must be the same type)
a > b
a >= b
a < b
a <= b
```

## Logical Operators (Boolean Operands)
```
a & b  // AND: evaluates both operands
a && b // AND: evaluates right operand only if necessary
a | b  // OR: evaluates both operands
a || b // OR: evaluates right operand only if necessary
!a
```

# Data Structures

## array
```
int[] ia = {1,2,3};
string[] sa = {"one","two","three","four"};

// reference an element
ia[1] == 2

// sort
Array.Sort(sa); // {"four","one","three","two"}

// iterate
foreach (string s in sa)
  Console.WriteLine(s);
```

## list
```
using System.Collections.Generic;

List<string> my_list = new List<string>();
List<string> my_list = new List<string> {"one","two","three","four"} ;
my_list[2].Equals("three") // true
List<string> list_slice = my_list.GetRange(2, 1); // GetRange(startIndex, numItems)
my_list.Add("five"); // my_list is now {"one","two","three","four","five"}
my_list.Insert(0,"zero"); // Insert(atIndex, valueToInsert)
my_list.RemoveAt(2); // is now {"zero","one","three","four","five"}
my_list.AddRange(new List<string> {"six","seven"}); // append another List<type> to the end

// check length
my_list.Count == 7 // true

// iteration
foreach(var element in my_list)
{
    processValue(element);
}

// check if value is in list (basic data types)
string first_match = my_list.Find(x => x.StartsWith("t"));

// find all matching values in list
List<string> all_matches = my_list.FindAll(x => x.StartsWith("t"));

// casting List<X> to List<Y>
using System.Linq;
List<Y> listOfY = listOfX.Cast<Y>().ToList();

/*
Some things to be aware of:
This casts each item in the list - not the list itself. A new List<Y> will be created by the call to ToList().
This method does not support custom conversion operators. ( see http://stackoverflow.com/questions/14523530/why-does-the-linq-cast-helper-not-work-with-the-implicit-cast-operator )
This method does not work for an object that has a explicit operator method (framework 4.0)
*/
```

## dictionary
```
using System.Collections.Generic;

Dictionary<string, int> my_dict = new Dictionary<string,int>();
Dictionary<string, int> my_dict = new Dictionary<string, int>() { { "a", 1 }, { "b" , 2 } };
my_dict["c"] = 3; // my_dict is now {'a':1, 'b':2, 'c':3}
mydict.Remove("b"); // my_dict is now {'a': 1, 'c': 3}

// iteration of key value pairs enumerator returns KeyValuePair<TKey,TValue>
foreach(var kvpair in my_dict)
    processValue(kvpair.Key,kvpair.Value);

//get the values or the keys to iterate through
//use foreach to iterate through the collections
Dictionary<string, string>.ValueCollection valueColl = my_dict.Values;
Dictionary<string, string>.KeyCollection keyColl = my_dict.Keys;

// check if a key is in dictionary
if (my_dict.ContainsKey("a")) // true

// check if a value is in dictionary
// determines equality using default equality comparer EqualityComparer<T>.Default for the type of values in the dictionary.
// This method performs a linear search; therefore, the average execution time is proportional to Count
if (my_dict.ContainsValue("a")) // true
```

## set
```
HashSet<string> my_set = new HashSet<string>();
HashSet<string> my_set = new HashSet<string>() { "a", "b" };
HashSet<string> set0 = new HashSet<string>() { "b", "c", "d" };

my_set.Add("d"); // my_set is now {"a","b","d"}
my_set.Remove("a"); // my_set is now {"b","d"} 
my_set.UnionWith(new HashSet<string>() { "e", "f" }); // my_set is now {"b","d","e","f"}
my_set.IntersectWith(set0); // my_set is now {"b","d"}
my_set.ExceptWith(new HashSet<string>() { "d", "e", "f" }); // my_set is now {"b"}

// check if value is in set
if(my_set.Contains("b"))

// enumeration/iteration
foreach (var e in my_set)
    processValue(e); 
```

# Control Flow Statements

## if/else
```
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
```
for (int i = 0; i < 101; i++) {
    var x = doSomething(i);
	if (x == 123) {
	    continue;
	}
	else {
	    break;
	}
}

// can iterate where an enumerator is available
foreach (int element in fibNumbers)
{
    count++;
    Console.WriteLine($"Element #{count}: {element}");
}
```

## while
```
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
```
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
```
try {
	//...
}
catch (Exception e) {
	MyLogger.logError(e.ToString());
}
finally {
	//cleanup();
}
```

# Numbers

## Operators
```
int x = 10;
int y = 5;
x + y == 15
x - y == 5
x / y == 2
x * y == 50
Math.Pow(x,y) == 100000.0 //exponent, result is a double
y % x == 5  //modulo
```

## Absolute Value
```
Math.Abs(-x) == 10
```

## Min/Max
```
Math.Min(x,y) == 5
Math.Max(x,y) == 10
```

## Floor/Ceiling
```
double f = 3.634;
Math.Floor(f) == 3
Math.Ceiling(f) == 4
```

## Convert Between Int/Float
```
(int)f == 3
(double)x == 10.0
```

## Parse Number from String
```
int result = Int32.Parse("27");
double dresult = Double.Parse("27.89721");
```
## Random
```
using System.Security.Cryptography;
public static uint RandomUint() {
	using (RNGCryptoServiceProvider crypto = new RNGCryptoServiceProvider())
	{
		byte[] data = new byte[4];
		crypto.GetBytes(data);
		return BitConverter.ToUInt32(data);
	}
}
uint ru = RandomUint();
```

# Strings

## Literals
```
string singleLineStr = "single line"; // can use single or double quotes
string multiLineStr = @"multi line...
...string";
string msg2 = "\u0048 unicode literal";
```

## Concatenation
```
str1 + str2
```

## Formatting
```
string msg0 = $"Something {str1} and string {x.foo()}";
string msg1 = String.Format("An int {0} and string {1}",123,"test"); // msg1.Equals("An int 123 and string test");
string msg2 = String.Format("Formatting examples, an int {0:D}, a float to 3 places {1:F3}, and hex {2:X}", 3, 1045.89735, 0x2F);
// msg2.Equals("Formatting examples, an int 3, a float 1045.897, and hex 2F")
```

## Convert Types to a String Representation
```
string svs = someVariable.ToString();
$"This will obtain a string representation of {someVariable}"
```

## Strip/Trim
```
string xs = ",a,b,c,,";
string xst = xs.Trim(new char[] { ',', ':'});

string x = ",a,b,c,,";
string xt = x.Trim(); // will remove whitespace at both ends of the string
string xt = x.Trim(new char[] { ',', ':'}); // will remove the specified characters from both ends, in this case the output is 'a,b,c'
string xt = x.TrimStart(','); // will remove the specified characters from start, in this case the output is 'a,b,c,,'
string xt = x.TrimEnd(','); // will remove the specified characters from end, in this case the output is ',a,b,c'
```

## Split
```
string x = ",a,b,c,,";
string[] tokenList = x.Split(new string[] {",",":"}); // the separator strings can be regexes 
```

## Replace
```
var tx = x.Replace(",",":"); // tx is ':a:b:c::'
string y = "The Trees";
string ty = y.Replace("Trees","Forest"); // ty is "The Forest"
```

## StartsWith
```
if (y.StartsWith("The")){
```

## EndsWith
```
if (y.EndsWith("Forest")){
```

## Find a Substring
```
y.IndexOf("Trees") == 4 //find first occurrence of substring, returns -1 if not found
```

## Select a Substring
```
y = y.Substring(4); // slice from 4th character to the end of the string, returns: "Trees"
y = y.Substring(4,2); // (startIndex, lengthOfSlice), returns: "Tr"
```

## Regex
```	 
using System.Text.RegularExpressions;

// find simple pattern, get list of all occurrences
string ms = "";
foreach (Match m in Regex.Matches(stringToSearch, "[a-z]+"))
	ms = m.Groups[0].Value;

// match multiple groups
string line = "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)";
foreach (Match m in Regex.Matches(line, "([0-9\\.]+).+GET (.+) HTTP")) {
	string ipaddr = m.Groups[1].Value;
	string url = m.Groups[2].Value;
}
```

# Binary Bytes/Buffers

## Declaration
```
byte key = 0x65; //hex literal
byte[] header = {0xFF,0xC3,0x00,0x00};
char key = (char)0x00BC; //Char type is interpreted as unsigned 16bit value (unicode char)
byte[] data = System.Text.Encoding.UTF8.GetBytes("<h1>Hello World</h1>"); //convert string to UTF8 bytes
byte[] info = new UTF8Encoding(true).GetBytes("test string");
```

## Bytearray/Buffer Operations
```
using System.IO;
BinaryWriter bw = null;
try {
	MemoryStream ms = new MemoryStream();
	bw = new BinaryWriter(ms);
	bw.Write("test");
	bw.Write(0xFF);
	// ...
}
finally {
	bw?.Close();
}

// for reading, see file operations for detail, but the basics are...
br = new BinaryReader(bw.BaseStream);
br.BaseStream.Position = 0;
byte[] b8 = br.ReadBytes(8);

//convert MemoryStream to byte[]
// use standard array methods and operators to manipulate it
byte[] ba = ms.ToArray();
```

## Bitwise Operations 

```
// C# doesn't have binary literals, so have to use hex
byte a = 0xD9; //0b11011001
byte b = 0x9D; //0b10011101

// bitwise operators convert operands to int, so 
// need to cast back to char|byte
```

### And
```
// a & b
$"{Convert.ToString((byte)(a & b), 2).PadLeft(8, '0')}".Equals("10011001");
```

### Or
```
// a | b
$"{Convert.ToString((byte)(a | b), 2).PadLeft(8, '0')}".Equals("11011101");
```

### Xor
```
// a ^ b
$"{Convert.ToString((byte)(a ^ b), 2).PadLeft(8, '0')}".Equals("01000100");
```

### Not
```
// a ^ 0xFF
$"{Convert.ToString((byte)(a ^ 0xFF), 2).PadLeft(8, '0')}".Equals("00100110");
```

### Shift Left/Right
```
// a >> 4, or a << 5
$"{Convert.ToString((byte)(a >> 4), 2).PadLeft(8, '0')}".Equals("00001101");
```

# CLI Argument Parsing
```
//the executable name is NOT in the argument list, so args[0] is the first supplied command line argument after the exe name
static void Main(string[] args) {
    if (args.Length < 1)
      return;
    Console.WriteLine("Args[0]:"+args[0]); 
}
```

# Execute Shell Commands
```
using System.Diagnostics;

Process proc = new Process {
    StartInfo = new ProcessStartInfo {
        FileName = "program.exe",
        Arguments = "command line arguments to your executable",
        UseShellExecute = false,
        RedirectStandardOutput = true,
        CreateNoWindow = true
    }
};
proc.Start();
while (!proc.StandardOutput.EndOfStream) {
    string line = proc.StandardOutput.ReadLine();
    // do something with line
}
```

# File Operations
```
// file modes
```
## Open File and Read
```
// Read the file as one string.
string filelines_as_one_string = System.IO.File.ReadAllText(@"C:\temp\pB.txt");

// Read each line of the file into a string array. Each element of the array is one line of the file.
string[] filelines = System.IO.File.ReadAllLines(@"C:\temp\pB.txt");

// read binary data (also see binary bytearray/buffer operations section above)
using System.IO;
...
BinaryReader br = null;
try {
   br = new BinaryReader(new FileStream(@"C:\temp\pB.txt", FileMode.Open));
   int i = br.ReadInt32();
   double d = br.ReadDouble();
   byte b = br.ReadByte();
   byte[] b8 = br.ReadBytes(8);
} 
catch (IOException e) {
    Console.WriteLine(e.Message + "\n Error reading from file.");
}
finally {
	br?.Close();
}
```

## Open File and Write
```
// WriteAllLines creates a file, writes a collection of strings to the file, and then closes the file.  You do NOT need to call Flush() or Close().
string[] lines = { "First line", "Second line", "Third line" };
System.IO.File.WriteAllLines(@"C:\temp\file.txt", lines);

// WriteAllLines creates a file, writes a collection of strings to the file, and then closes the file.  You do NOT need to call Flush() or Close().
string text = "Single String\nContains entire file contents.";
System.IO.File.WriteAllText(@"C:\temp\file.txt", text);

// Write one line at a time to a file
using (System.IO.StreamWriter file = new System.IO.StreamWriter(@"C:\temp\file.txt"))
{
	string line = "something";
	//whatever logic or processing...
	file.WriteLine(line);
	//...
}

// write binary data
using System.IO;
...
BinaryWriter bw = null;
try {
   bw = new BinaryWriter(new FileStream(@"C:\temp\pB.txt", FileMode.Create));
   //overloaded Write() methods exist with bool,byte,byte[],char,char[],double,int,string arguments
   bw.Write(data);
} 
catch (IOException e) {
    Console.WriteLine(e.Message + "\n Error reading from file.");
}
finally {
	bw?.Close();
}
```

## Skip To File Location
```
// FileStream.Seek(offset_num_bytes, whence)
// 'whence' is optional and specifies positioning
// default is SeekOrigin.Begin == absolute file positioning
// SeekOrigin.Current == seek relative to the current position 
// SeekOrigin.End == seek relative to the file's end.

using System.IO;
...
BinaryReader br = null;
try {
   br = new BinaryReader(new FileStream(@"C:\temp\foo.dat", FileMode.Open));
   int offset_num_bytes = 8192;
   System.IO.SeekOrigin whence = SeekOrigin.Begin;
   br.BaseStream.Seek(offset_num_bytes, whence);
   byte[] b8 = br.ReadBytes(8);
} 
catch (IOException e) {
    Console.WriteLine(e.Message + "\n Error reading from file.");
}
finally {
	br?.Close();
}

```

## Close File
```
fileSteamObject.Close();
```

# Networking

## Sockets
```
// client send/receive

using System;  
using System.Net;  
using System.Net.Sockets;  
using System.Text;  
  
public class SynchronousSocketClient {  
  
    public static int Main(String[] args) {  
        byte[] bytes = new byte[1024];  
  
        try {  
            IPAddress ipAddress = IPAddress.Parse("192.168.51.51");  
            IPEndPoint remoteEP = new IPEndPoint(ipAddress,8088);  
  
            // Create a TCP/IP  socket.  
            Socket sender = new Socket(ipAddress.AddressFamily,   
                SocketType.Stream, ProtocolType.Tcp );  
  
            try {  
                sender.Connect(remoteEP);  
                byte[] msg = Encoding.ASCII.GetBytes("This is a test<EOF>");  
                int bytesSent = sender.Send(msg);  
                int bytesRec = sender.Receive(bytes);  
                Console.WriteLine("Echoed test = {0}",  
                    Encoding.ASCII.GetString(bytes,0,bytesRec));  
                sender.Shutdown(SocketShutdown.Both);  
                sender.Close();  
  
            } catch (Exception ex) {  
                Console.WriteLine("Exception : {0}",ex.ToString());  
            }    
        } catch (Exception e) {  
            Console.WriteLine( e.ToString());  
        }
        return 0;  
    }   
}


// server listen/reply

using System;  
using System.Net;  
using System.Net.Sockets;  
using System.Text;  
  
public class SynchronousSocketServer {  
  
    public static int Main(String[] args) {  
        byte[] bytes = new Byte[1024];  
		string data = null;
		
        IPAddress ipAddress = IPAddress.Parse("0.0.0.0");  
        IPEndPoint localEndPoint = new IPEndPoint(ipAddress,8088);  
  
        Socket listener = new Socket(ipAddress.AddressFamily,  
            SocketType.Stream, ProtocolType.Tcp );  
  
        try {  
            listener.Bind(localEndPoint);  
            listener.Listen(10);  //arg is max num of pending connections
  
            while (true) {  
                Socket handler = listener.Accept();  
                data = null;  
                while (true) {  
                    int bytesRec = handler.Receive(bytes);  
                    data += Encoding.ASCII.GetString(bytes,0,bytesRec);  
                    if (data.IndexOf("<EOF>") > -1) {  
                        break;  
                    }  
                }  
                Console.WriteLine( "Text received : {0}", data);  
                byte[] msg = Encoding.ASCII.GetBytes(data);  
                handler.Send(msg);  
                handler.Shutdown(SocketShutdown.Both);  
                handler.Close();  
            }  
  
        } catch (Exception e) {  
            Console.WriteLine(e.ToString());  
        }    
        return 0;  
    }   
}
```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client
```
//dotnet 4.0
using System;
using System.IO;
using System.Net;
//dotnet 4.5 uses HttpClient in System.Net.Http

class SimpleHttpClient
{
  static void Main(string[] args) {

    string page = "http://en.wikipedia.org/";
	HttpWebRequest http = (HttpWebRequest)HttpWebRequest.Create(page);
	http.Referer = "http://127.0.0.1";
	HttpWebResponse response = (HttpWebResponse )http.GetResponse();
	using (StreamReader sr = new StreamReader(response.GetResponseStream()))
	{
		string result = sr.ReadToEnd();
        if (result != null &&
            result.Length >= 500)
        {
            Console.WriteLine(result.Substring(0, 500) + "...");
        }
	}
  }
}
```

### HTTP server
```
using System.Text;
using System.Net;
...
HttpListener listener = new HttpListener();
listener.Prefixes.Add("http://localhost:8903/");
listener.Start();
while (true)
{
	HttpListenerContext ctx = listener.GetContext();
	HttpListenerRequest req = ctx.Request;
	HttpListenerResponse resp = ctx.Response;
	Console.WriteLine(req.Url.ToString());
	Console.WriteLine(req.HttpMethod);
	Console.WriteLine(req.UserHostName);
	Console.WriteLine(req.UserAgent);
	byte[] data = Encoding.UTF8.GetBytes("<h1>Hello World</h1>");
	resp.ContentType = "text/html";
	resp.ContentEncoding = Encoding.UTF8;
	resp.ContentLength64 = data.LongLength;
	resp.OutputStream.Write(data, 0, data.Length);
	resp.Close();
}
listener.Close();
```

# JSON Serialization

## Serialize
```
using System.Text.Json;
Dictionary<string, object> mydict = new Dictionary<string, object>()
{
	{"a", new int[] { 1,2,3 } },
	{"b", "test" }
};
string js = JsonSerializer.Serialize(mydict); // js is '{"a": [1, 2, 3], "b": "test"}'
// most simple types and datastructs (List,Dictionary) have default serializers
```

## Deserialize
```
using System.Text.Json;
string jsonstr = "{\"a\": [4, 5, 6], \"b\": \"foo\"}";
Dictionary<string, object> mydict = JsonSerializer.Deserialize<Dictionary<string, object>>(jsonstr);
```

# Object Introspection
```
using System.Type;

System.Type type = obj.GetType();

// dynamically load assembly from file Test.dll
Assembly testAssembly = Assembly.LoadFile(@"c:\Test.dll");

// get type of class Calculator from just loaded assembly
Type calcType = testAssembly.GetType("Test.Calculator");

// create instance of class Calculator
object calcInstance = Activator.CreateInstance(calcType);

// get info about property: public double Number
PropertyInfo numberPropertyInfo = calcType.GetProperty("Number");

// get value of property: public double Number
double value = (double)numberPropertyInfo.GetValue(calcInstance, null);

// set value of property: public double Number
numberPropertyInfo.SetValue(calcInstance, 10.0, null);
```

# Date/Time
```
//format will be 7/10/2021 6:24:06 PM
DateTime dateTime = DateTime.Now;
DateTime dateTime = new DateTime(2016, 7, 15, 3, 15, 0);
Console.WriteLine(dateTime);

//epoch time format in milliseconds
DateTimeOffset now = DateTimeOffset.UtcNow;
long unixTimeMilliseconds = now.ToUnixTimeMilliseconds();
Console.WriteLine(unixTimeMilliseconds);
```

# Sleep
```
System.Threading.Thread.Sleep(2500); // milliseconds
```

# Multithreading

## Threads
```
using System.Threading;
using System.Threading.Tasks;

//Example of synchronous method getStuff() turned into async
//The calling thread will yield, same or another thread will continue the execution of the function after await returns: 
//just wrap whatever the method returns in Task<>, or if it's void return Task
//https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming
public async Task<MyThing> getStuffAsync()
{
	//convention is that Async methods are suffixed with "Async"
	//need to prefix with await
	MyThing mt = await ManagerThing.SomethingThatTakesAWhileAsync();
	//the same thread or another thread could continue execution after the method returns
	mt.SomethingElse();
	return mt;
}

//how to call it (the calling code needs to be declared as async too):
MyThing m1 = await getStuffAsync();
```

## Synchronization/Locking Primitives
```
using System.Threading;
using System.Threading.Tasks;

//semaphore
protected SemaphoreSlim sem = new SemaphoreSlim(0);
sem.Wait(); // blocks, synchronous
await sem.WaitAsync(); // async
sem.Release(1);

//mutex
private readonly object _mutex = new object();
lock(_mutex)
{
	//doCriticalCode();
}

// threadsafe data structs:
using System.Collections.Concurrent;
ConcurrentQueue<Message> _messages = new ConcurrentQueue<Message>();

```
