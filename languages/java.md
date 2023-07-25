# Example Skeleton Main Program
```
// HelloWorld.java
public class HelloWorld {
  public static void main(String[] args) {
    String m1 = "m1";
    System.out.println("m1="+m1);
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
System.out.println("Hello World!");
```

## Input
```
import java.util.Scanner;
...
Scanner scan = new Scanner(System.in);
System.out.println("Enter your name:");
String name = scan.next();
System.out.println("Enter your age:");
int age = scan.nextInt();
```

# Namespaces
The package is Java's mechanism for managing namespaces. They allow programmers to create small private areas in which to declare classes. The names of those classes will not collide with identically named classes in different packages.

```
// filename: $PROJECTPATH/src/org/foobar/mypackage/myclass.java
package org.foobar.mypackage;
public class myclass { //...

// to use myclass
import org.foobar.mypackage.*; // OR...
import org.foobar.mypackage.myclass; 
// can also use it by its fully qualified name...
org.foobar.mypackage.myclass m = new org.foobar.mypackage.myclass(); 
```

# Classes

Go does not provide classes but it does provide structs. Methods can be added on structs. This provides the behaviour of bundling the data and methods that operate on the data together akin to a class.

## Declaration
Put the class declaration into a file named `$PROJECTPATH/src/org/foobar/mypackage/MyClassA.java`.
```
package org.foobar.mypackage;
public class MyClassA {  
    protected String Bar;
    protected int Foo = 0;

    public MyClassA(String initBar)
		{
			this.Bar = initBar;
		}

		public void DoSomething()
		{
			System.out.println("Hello " + this.Bar);
		}

		public int GetSomething()
		{
			return Foo;
		}

}

// in the main program....
import org.foobar.mypackage.MyClassA;
public class MainProgram {
  public static void main(String[] args) {  
    MyClassA mca = new MyClassA("Sam");
    mca.DoSomething();
  }
}

//interface
public interface ActionableThing {
	public void doIt();
}

//implement an interface
public class MyActionableThing implements ActionableThing {
	@Override 
	public void doIt() {
		System.out.println("doing it");
	}
}//ActionableThing t = new MyActionableThing();

//abstract class
public abstract class AbstractThing {
	public void doIt() {
		System.out.println("doing it");
	}
	
	public abstract int getId();
}

//implement an abstract class
public class MyThing extends AbstractThing {
	 
	public int getId() {
		return 0;
	}
}//AbstractThing t = new MyThing();
```

## Inheritance
```
public class MyClassSubA extends MyClassA
{
	public MyClassSubA()
	{
		//the super constructor is called using super()
		super("subA");
	}
	
	@Override //best practice because compiler will flag if it cant match to superclass
	public void DoSomething()
	{
		System.out.println("Hello "+this.Bar+" from SubA");
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
	public static String MyStaticFunction(String arg1, String arg2)
	{
		return String.format("MyStaticFunction %s %s", arg1, arg2);
	}
}
// call it like this:
FunctionModule.MyStaticFunction("a","b");

// lambda function
interface NumStringFunction {
  public String GetResult(int n, String s);
}
...
NumStringFunction nsf = (n,s) -> { return String.format("%d %s",n,s); };
System.out.println(nsf.GetResult(500,"five hundred"));

// or can use the Consumer type:
import java.util.function.Consumer;
Consumer<Integer> method = (n) -> { System.out.println(n); };
// numbers is an ArrayList<Integer>
numbers.forEach( method );
```

# Variables

## Declaration/Assignment
```
// must explicitly set the type:
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
	public static String SOME_VALUE = "global";
}

// somewhere in the code...
FunctionModule.SOME_VALUE = "changed";
```

## Data Basic Types
```
int my_int = 1;
short s = 123;
long l = 2189738723;

double d = 3.25;
float f = 3.234f;

boolean my_bool = false || true;

byte b = 0x23;
char c = 'a'; //unicode

Object something = null;
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
a && b // AND: evaluates right operand only if necessary
a || b // OR: evaluates right operand only if necessary
!a    // NOT
```

# Data Structures

## array
```

int[] ia = {1,2,3};
String[] sa = {"one","two","three","four"};

// reference an element
ia[1] == 2

// array length
sa.length == 4

// iterate
for (String s : sa) {
  System.out.println(s);
}
```

## list
```
import java.util.ArrayList;

ArrayList<String> my_list = new ArrayList<String>();
ArrayList<String> my_list = new ArrayList<String>(Arrays.asList("one","two","three","four")) ; // import java.util.Arrays
my_list.get(2).equals("three") // true
my_list.set(2,"three") // change an element

List<String> list_slice = my_list.subList(2, 4); // subList(startIndexInclusive, untilBeforeIndex) {"three","four"}
my_list.add("five"); // my_list is now {"one","two","three","four","five"}
my_list.add(0,"zero"); // Insert(atIndex, valueToInsert)
my_list.remove(2); // is now {"zero","one","three","four","five"}
my_list.addAll(new ArrayList<String> (Arrays.asList("six","seven"))); // append another ArrayList<type> to the end

// check length
my_list.size() == 7 // true

// iteration
for(String element : my_list)
{
    processValue(element);
}

// check if value is in list, can check objects if they have implemented a custom hashcode or equals operator
if(my_list.contains("six")) { //...
int itemindex = my_list.indexOf("six"); // == 5, or -1 if not in list

// find all matching values in list
import java.util.stream.Collectors;
List<String> all_matches = my_list.stream().filter(el -> el.startsWith("s")).collect(Collectors.toList());
```

## dictionary
```
import java.util.HashMap; // <- not threadsafe
//import java.util.ConcurrentHashMap; <- threadsafe

HashMap<String, Integer> my_dict = new HashMap<String, Integer>();
my_dict.put("a",1); // my_dict is now {'a':1}
my_dict.put("b",2); // my_dict is now {'a':1, 'b':2}
my_dict.put("c",3); // my_dict is now {'a':1, 'b':2, 'c':3}
my_dict.remove("b"); // my_dict is now {'a': 1, 'c': 3}

// iteration of keys
for(String key : my_dict.keySet())
    processValue(key,my_dict.get(key));

// iteration of values
for(Integer val : my_dict.values())
    System.out.println("value : "+val.toString());

// check if a key is in dictionary
if(my_dict.containsKey("c")) { //...

// check if a value is in dictionary
if(my_dict.containsValue(1)) { //...
```

## set
```
import java.util.HashSet;
// a hack for a threadsafe Set is to use ConcurrentHashMap.newKeySet().

HashSet<String> my_set = new HashSet<String>();
HashSet<String> my_set = new HashSet<String>(Arrays.asList("a", "b")); //import java.util.Arrays
HashSet<String> set0 = new HashSet<String>(Arrays.asList("b", "c", "d")); //import java.util.Arrays

my_set.add("d"); // my_set is now {"a","b","d"}
my_set.remove("a"); // my_set is now {"b","d"} 
my_set.addAll(new HashSet<string>(Arrays.asList("e", "f"))); // my_set is now {"b","d","e","f"}
my_set.retainAll(set0); // my_set is now {"b","d"}
my_set.removeAll(new HashSet<string>(Arrays.asList("d", "e", "f"))); // my_set is now {"b"}

// check if value is in set
if(my_set.contains("b"))

// enumeration/iteration
for(String s : my_set)
    processValue(s); 
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
    int x = doSomething(i);
	if (x == 123) {
	    continue;
	}
	else {
	    break;
	}
}

// can iterate where an enumerator is available
int[] fibNumbers = {1,2,3,5,8,13};
for (int element : fibNumbers) {
    System.out.println(String.format("element: %d",element));
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
	MyLogger.logError(e.toString());
	e.printStackTrace(); // print full stack trace to stdout
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
Math.pow(x,y) == 100000.0 //exponent, result is a double
y % x == 5  //modulo
```

## Absolute Value
```
abs(-x) == 10
```

## Min/Max
```
Math.min(x,y) == 5
Math.max(x,y) == 10
```

## Floor/Ceiling
```
double f = 3.634;
Math.floor(f) == 3
Math.ceil(f) == 4
```

## Convert Between Int/Float
```
(int)f == 3
(int)Math.round(f) == 4
(double)x == 10.0
```

## Parse Number from String
```
int result = Integer.parseInt("27");
double dresult = Double.parseDouble("27.89721");
```
## Random
```
import java.security.SecureRandom;
int ri = SecureRandom.nextInt(upperBound);
```

# Strings

## Literals
```
String singleLineStr = "single line"; // can only use double quotes
// java doesn't have multiline string delimiters
String multiLineStr = "multi line..." +
"...string";
```

## Concatenation
```
str1 + str2

// more efficient, however use StringBuffer if require threadsafe
StringBuilder sb = new StringBuilder(""); 
sb.append("Part 1 ");
sb.append("Part 2.");
String newstring = sb.toString();
```

## Formatting
```
String msg1 = String.format("An int %d and string %s",123,"test"); // msg1.equals("An int 123 and string test");
String msg2 = String.format("Formatting examples, an int %d, a float to 3 places %.3f, hex %02X", 3, 1045.89735, 0x2F);
// msg2.equals("Formatting examples, an int 3, a float 1045.897, hex 2F")
```

## Convert Types to a String Representation
```
String svs = someVariable.toString();
String.format("This will obtain a string representation of %s",someVariable);
```

## Strip/Trim
```
String xs = ",a,b,c,,";
String xt = xs.trim(); // will remove whitespace at both ends of the string
// java doesn't have a function that trims non-whitespace chars, use xs.replaceAll(",", "")
```

## Split
```
String x = ",a,b,c,,";
String[] tokenList = x.split(",",-1); // generally use -1 as 2nd param, the separator strings can be regexes
// the above returns: ['','a','b','c','',''] 
```

## Replace
```
String tx = x.replaceAll(",",":"); // tx is ':a:b:c::'
String y = "The Trees";
String ty = y.replaceAll("Trees","Forest"); // ty is "The Forest"
```

## StartsWith
```
if (y.startsWith("The")){
```

## EndsWith
```
if (y.endsWith("Forest")){
```

## Find a Substring
```
y.indexOf("Trees") == 4 //find first occurrence of substring, returns -1 if not found
```

## Select a Substring
```
y = y.substring(4); // slice from 4th character to the end of the string, returns: "Trees"
y = y.substring(4,6); // (startIndex, endBeforeIndex), returns: "Tr"
```

## Regex
```
import java.util.regex.*;
String test = "think this is a test string. that.";
Pattern pattern = Pattern.compile("[a-z]+");
Matcher matcher = pattern.matcher(test);
while (matcher.find()) {    
	System.out.println("I found the text "+matcher.group()+" starting at index "+    
	matcher.start()+" and ending at index "+matcher.end());    
}

// match multiple groups
String line = "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)";
Pattern pattern = Pattern.compile("([0-9\\.]+).+GET (.+) HTTP");
Matcher matcher = pattern.matcher(line);
while (matcher.find()) {
        String ipaddr = matcher.group(1);
        String url = matcher.group(2);
        System.out.println(String.format("ip: %s, url: %s",ipaddr,url));
}
```

# Binary Bytes/Buffers

## Declaration
```
byte key = (byte)0x65; //hex literal are ints so must be cast
byte val = (byte)0b01101101;     //binary literal are ints so must be cast
byte[] header = {(byte)0xFF,(byte)0xC3,(byte)0x00,(byte)0x00}; // hex literals are ints so must be cast
char key = 0x00BC; //Char type is interpreted as unsigned 16bit value (unicode char)
byte[] data = "<h1>Hello World</h1>".getBytes(); //convert string to UTF8 bytes
```

## Bytearray/Buffer Operations
```
import java.io.*;
ByteArrayOutputStream out = new ByteArrayOutputStream();
DataOutputStream dout = new DataOutputStream(out);
try {
	dout.writeInt(1234);
	dout.writeByte(0xFE);
	dout.writeFloat(1.2f);
	dout.write("<h1>Hello World</h1>".getBytes("UTF-8"));
	byte[] storingData = out.toByteArray();

// reading back from byte[]
	ByteArrayInputStream in = new ByteArrayInputStream(storingData);
	DataInputStream din = new DataInputStream(in);
	int v1 = din.readInt();//1234
	byte v2 = din.readByte();//0xFE
	float v3 = din.readFloat();//1.2f
	byte[] tmpStrBytes = new byte[32];
	din.read(tmpStrBytes, 0, 32); // read(byte_arr, offset, length)
	String msg = new String(tmpStrBytes);
}
catch (Exception e) { 
	//...
}
```

## Bitwise Operations 

```
// bytes are treated as signed int, eg for shift operations
// chars are treated as unsigned
char a = (char)0b11011001;
char b = (char)0b10011101;

// bitwise operators convert operands to int, so 
// need to cast back to char|byte
```

### And
```
// a & b
(char)(a & b) == (char)0b10011001
```

### Or
```
// a | b
(char)(a | b) == (char)0b11011101
```

### Xor
```
// a ^ b
(char)(a ^ b) == (char)0b01000100
```

### Not
```
// a ^ 0xFF
(char)(a ^ 0xFF) == (char)0b00100110
```

### Shift Left/Right
```
// a >> 4, or a << 5
(char)(a >> 4) == (char)0b00001101
```

# CLI Argument Parsing
```
//the class name is NOT in the argument list, so args[0] is the first supplied command line argument after the class name
// eg. java HelloWorld firstarg
// prints: "firstarg"
public static void main(String[] args) {
    if (args.length < 1)
      return;
    System.out.println("Args[0]:"+args[0]); 
}
```

# Execute Shell Commands
```
String cmd = "ls -al";
Runtime run = Runtime.getRuntime();
Process pr = run.exec(cmd);
pr.waitFor();
BufferedReader buf = new BufferedReader(new InputStreamReader(pr.getInputStream()));
String line = "";
while ((line=buf.readLine())!=null) {
    System.out.println(line);
}

ProcessBuilder builder = new ProcessBuilder();
// windows: builder.command("cmd.exe", "/c", "dir");
builder.command("sh", "-c", "ls");
builder.directory(new File(System.getProperty("user.home")));
Process process = builder.start();
BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
while ( (line = reader.readLine()) != null)
    System.out.println(line);
int exitCode = process.waitFor();
```

# File Operations
```
// file modes
```
## Open File and Read
```
import java.io.*;

// Read each line of the file into a string array. Each element of the array is one line of the file.

// import java.util.ArrayList;
ArrayList<String> filelines = new ArrayList<String>();
BufferedReader bfr = new BufferedReader(new FileReader("C:\\temp\\pB.txt"));
String line;
while ((line = bfr.readLine()) != null)
        filelines.add(line);
for(String sl : filelines)
  System.out.println(sl);

// read binary data (also see binary bytearray/buffer operations section above)

import java.nio.file.Files;
byte[] fileContent = Files.readAllBytes(new File("/tmp/blah.dat").toPath());
for(byte bx : fileContent)
  System.out.print(String.format("%02x ",(byte)bx));
```

## Open File and Write
```
import java.io.*;

// write one line at a time to file

String[] lines = { "First line", "Second line", "Third line" };
BufferedWriter bfw = new BufferedWriter(new FileWriter("/tmp/out.txt"));
for(String ol : lines) {
  bfw.write(ol);
  bfw.newLine(); // there is no writeLine()
}
bfw.close();  // flushes and closes the file

// write binary data

import java.nio.file.Files;
byte[] binData = {(byte)0xFF,(byte)0xC3,(byte)0x00,(byte)0x00};
Files.write(new File("/tmp/out.dat").toPath(), binData);
```

## Skip To File Location
```
// Only supports absolute file positioning (offset from 0)

import java.io.RandomAccessFile;

RandomAccessFile rafile = new RandomAccessFile(args[0], "rw");
rafile.seek(20); // num of bytes
int aByte = rafile.read(); // increments the file pointer
byte[] dest = new byte[16];
int bytesRead = rafile.read(dest, 0, dest.length); // byte[], offset, num_bytes_to_read
String sval = new String(dest);
long position = file.getFilePointer(); // get current position offset from 0
//file.write(65); // ASCII code for A
//file.close();
```

## Close File
```
fileSteamObject.close();
```

# Networking

## Sockets
```
// client send/receive

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class SocketClient {

    public static void main(String[] args) throws IOException {
        Socket s = new Socket("10.3.3.167", 9090);
        PrintWriter out = new PrintWriter(s.getOutputStream(), true);
        out.print("GET / HTTP/1.1\r\nHost: 10.3.3.167\r\n\r\n");
        out.flush();
        BufferedReader input = new BufferedReader(new InputStreamReader(s.getInputStream()));
        String reply_line;
        while((reply_line = input.readLine()) != null)
                System.out.println(reply_line);
        System.exit(0);
    }
}


// server listen/reply

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketServer {

    public static void main(String[] args) throws IOException {
        ServerSocket listener = new ServerSocket(9090);
		while (true) {
			Socket s = listener.accept();
			BufferedReader input = new BufferedReader(new InputStreamReader(s.getInputStream()));
			String request_line;
			while(!(request_line = input.readLine()).equals("<EOF>"))
				System.out.println(request_line);
			PrintWriter out = new PrintWriter(s.getOutputStream(), true);
			out.print("JavaSocketServer 1.0\r\nHelloWorld!\r\n\r\n");
			out.flush();
			s.close();
		}
    }
}
```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client
```
import java.io.IOException;
import java.net.*;
import java.io.*;

public class HttpClient
{
	public static void main(String[] args) throws IOException {
		URL obj = new URL("http://"+args[0]);
		HttpURLConnection con = (HttpURLConnection) obj.openConnection();
		con.setRequestMethod("GET");
		con.setRequestProperty("User-Agent", "javabrowser23");
		int responseCode = con.getResponseCode();
		String server_hdr = con.getHeaderField("Server");
		System.out.println("GET Response Code :: " + responseCode + ", Server :: " + server_hdr);
		if (responseCode == HttpURLConnection.HTTP_OK) { // success
				BufferedReader in = new BufferedReader(new InputStreamReader(
								con.getInputStream()));
				String inputLine;
				StringBuffer response = new StringBuffer();
				while ((inputLine = in.readLine()) != null) {
						response.append(inputLine);
				}
				in.close();

				// print result
				System.out.println(response.toString());
		} else {
				System.out.println("GET request failed");
		}
	}
}
```

### HTTP server
Oracle Java and OpenJDK since Java 6 have com.sun.net.httpserver
```
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

public class SimpleHttpServer {

    public static void main(String[] args) throws Exception {
        HttpServer server = HttpServer.create(new InetSocketAddress(9090), 0);
        server.createContext("/test", new MyHandler());
        server.setExecutor(null); // creates a default executor
        server.start();
    }

    static class MyHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange t) throws IOException {
            String response = "This is the response";
            t.sendResponseHeaders(200, response.length());
            OutputStream os = t.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }

}
```

# JSON Serialization

Java 8 < 15 have the Nashorn engine, however it was removed in 15
Should use a proper JSON library like Gson, Jackson etc.
 
## Serialize
```
// not possible using Nashorn
```

## Deserialize
```
import java.util.Map;
import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class HelloWorld {
	public static void main(String[] args) {
		try{
			ScriptEngineManager sem = new ScriptEngineManager();
			ScriptEngine engine = sem.getEngineByName("javascript");

			String json = "{\"a\":\"test\",\"b\":\"123\",\"c\":[1,2,3]}";
			String script = "Java.asJSONCompatible(" + json + ")";
			Object result = engine.eval(script);
			Map contents = (Map) result;
			contents.forEach((t, u) -> {
				System.out.println(String.format("%s : %s",t,u));
			});
		} 
		catch (Exception e) {
			System.out.println(e.toString());
		}
	}
}
```

# Object Introspection
```
import java.lang.reflect.*;
import java.util.*;

public class ReflectTest {

        public static void main(String[] args) {

                HashMap<String,String> hm = new HashMap<String,String>();
                Object mo = hm;
                Class<?> oc = mo.getClass();

// get class name for an object:
                System.out.println("classname:"+oc.getSimpleName());

// get superclass for an object:
                System.out.println("superclass:"+oc.getSuperclass());

// get implemented interface for an object:
                Class<?>[] ocifaces = oc.getInterfaces();
                for (Class<?> ifc : ocifaces)
                        System.out.println("interfacename:"+ifc.getSimpleName());

// get fields/properties for an object:
                Field[] fields = hm.getClass().getDeclaredFields();
                for (Field field : fields)
                        System.out.println("fieldname:"+field.getName());

// get methods for an object:
                Method[] methods = oc.getDeclaredMethods();
                for (Method m : methods)
                        System.out.println("methodname:"+m.getName());

// get constructor for an object:
                try {
                        Constructor<?> ocons = oc.getConstructor();

// invoke constructor for an object:
                        HashMap<String,String> newhm = (HashMap<String,String>) ocons.newInstance();

// invoke method for an object, : getDeclaredMethod(methodName, arg1Class, arg2Class ... )
                        Method hmPutMethod = oc.getDeclaredMethod("put",Object.class,Object.class);
                        hmPutMethod.invoke(newhm,"somekey","somevalue");

// dynamically load a class (ReflectTest.class and its dependencies should be in the classpath, eg. in the same folder as the calling class)
                        ClassLoader cloader = ReflectTest.class.getClassLoader();
                        Class dclass = cloader.loadClass("DynClass");
                        Method mSetProp = dclass.getDeclaredMethod("setProperty",String.class);
                        Object dco = dclass.getConstructor().newInstance();
                        mSetProp.invoke(dco,"NewValue");
                        Method mGetProp = dclass.getDeclaredMethod("getProperty");
                        System.out.println( (String) mGetProp.invoke(dco) );
                }
                catch (Exception e) { e.printStackTrace(); }

        }
}

```

# Date/Time
```
import java.util.Date;
import java.text.SimpleDateFormat;

try {
// Current time (local timezone):  HH -> 0-23, hh -> 0-12 (use "a" for AM/PM)
	Date now = new Date();
	System.out.println(new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(now));

// Epoch time (milliseconds):
	System.out.println(now.getTime());

// Parse a string into a Date object:
	Date then = new SimpleDateFormat("yyyy/MM/dd hh:mm:ss").parse("2011/05/31 12:52:32");
	System.out.println(then);	
}
catch (Exception e) { e.printStackTrace(); }
```

# Sleep
```
import java.util.concurrent.TimeUnit;
try {
	TimeUnit.SECONDS.sleep(3);
}
catch (InterruptedException ie) {
	// handle interrupted
}
```

# Multithreading

## Threads
```
// no import statement required
public class MyThread extends Thread
{
  public void run() {
        System.out.println("Thread " + Thread.currentThread().getId() + " is running");
  }

  public static void main(String[] args) {
        MyThread mt = new MyThread();
        mt.start();
		//Other thread methods:
		//void join(long millisecTimeout); //caller blocks until termination or timeout expires
		//void interrupt();
		//boolean isAlive();
  }
}
```

## Synchronization/Locking Primitives
```
import java.util.concurrent.Semaphore;

//semaphore
int MAX_AVAILABLE = 100;
Semaphore available = new Semaphore(MAX_AVAILABLE, true);
available.acquire();
available.release();

// threadsafe data structs: (see java.util.concurrent.*)

import java.util.concurrent.ConcurrentHashMap;
ConcurrentHashMap<String,String> sharedHM = new ConcurrentHashMap<String,String>();

import java.util.concurrent.CopyOnWriteArrayList;
CopyOnWriteArrayList<String> sharedAL = new CopyOnWriteArrayList<String>();
```
