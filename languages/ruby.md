
# Example Skeleton Main Program
```ruby
require 'date'

def greet(name)
  today = Date.today
  puts "Hello, #{name}! Today is #{today}."
end

greet(ARGV[0])
```

# Comments
```ruby
# This is a single-line comment

=begin
This is a multi-line comment.
It can span multiple lines.
=end
```

# Console Input/Output

## Output
```ruby
puts "Message #{variablename} with more literal data."
```

## Input
```ruby
person = gets #read input, simple

print "Enter your name: " # prompt
name = gets.chomp  # remove any trailing newline
puts "Hello, #{name}!"
```

# Namespaces
Ruby doesn't have a direct equivalent to namespaces like in languages like C++ or Java. However, it uses Modules to achieve similar functionality.:
```ruby
=begin
example folder structure:
my_app/
├── lib/
│   ├── greeting.rb
│   └── utils.rb
├── bin/
│   └── main.rb
└── Gemfile
=end

# contents of ./lib/greeting.rb
module Greeting
  def hello(name)
    puts "Hello, #{name}!"
  end
end

# in main.rb
require_relative '../lib/greeting'
...
Greeting.hello("Alice")
```

# Classes

## Declaration
```ruby
class Person
  def initialize(name, age)
    @name = name  #instance variable
    @age = age
    @@count = 0   #class variable, called static member in other languages
  end

  def self.count # self here means this is a class (static) method
    @@count
    #Ruby doesn't require an explicit return statement to specify the value returned from a method.
    #Instead, Ruby implicitly returns the value of the last expression evaluated within the method
    #If you want to exit a method before the end, you can use return
    #If a method has multiple potential return values, using return can help clarify the logic
  end

  def greet
    puts "Hello, my name is #{@name} and I am #{@age} years old."
    @@count += 1  # can reference class variables from instance methods
  end

  #  methods that end with an exclamation point typically indicate that the method modifies the receiver object.
  # This convention is a strong signal to the programmer about the method's behavior.
  # Examples:
  #  upcase!: Modifies the string in-place by converting it to uppercase.
  #  sort!: Modifies the array in-place by sorting its elements.
  def age_up!
    @age += 1
  end

  #methods ending with a question mark typically return a boolean value (true or false).
  #This convention is a strong indicator to the programmer about the method's purpose.
  def name?
    @name != nil
  end

  def full_name(last_name)
    "#{@name} #{last_name}"
  end

  def introduce
    puts "Hello, my name is #{full_name('Smith')}"
  end

end

```

## Inheritance
```ruby
class Animal
  def initialize(name)
    @name = name
  end

  def speak
    puts "Generic animal sound"
  end
end

class Dog < Animal
  def speak
    puts "#{@name} says Woof!"
  end
end

class Cat < Animal
  def speak
    puts "#{@name} says Meow!"
  end
end

# Create objects
dog = Dog.new("Buddy")
cat = Cat.new("Whiskers")

# Call methods
dog.speak
cat.speak
```

Mixing-in (like interfaces in other languages)

```ruby
module Swimmable
  def swim
    "I'm swimming!"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable         # mixing in Swimmable module
end

class Mammal < Animal
end

class Cat < Mammal
end

class Dog < Mammal
  include Swimmable         # mixing in Swimmable module
end

sparky = Dog.new
neemo  = Fish.new
paws   = Cat.new

sparky.swim                 # => I'm swimming!
neemo.swim                  # => I'm swimming!
paws.swim                   # => NoMethodError: undefined method `swim' for #<Cat:0x007fc453152308>
```

## Instantiation and Method Invocation
```ruby
person1 = Person.new("Alice", 30)
person1.greet
person1.age_up!
person1.greet
puts person1.full_name("Foo")
# parenthesis are actually optional in method invocation (but required for declaration)
def foo(arg1, arg2)
   puts arg1+arg2
end
foo 'test','abc'  # prints testabc
```

# Functions
```ruby
def my_function(arg1, arg2 = "default")
  puts "arg1: #{arg1}"
  puts "arg2: #{arg2}"
end

my_function("test")
# prints arg1: test\narg2: default
```

You can use keyword arguments for functions too...

- Definition: When defining a method, you use a colon (:) before the argument name to indicate it as a keyword argument.
- Calling: When calling the method, you pass the arguments using the key: value syntax.
- Mixing positional and keyword arguments: You can mix positional and keyword arguments, but keyword arguments must come after positional ones.
- Keyword rest arguments: You can use **kwargs to capture any extra keyword arguments as a hash.

```ruby
def greet(name:, greeting: "Hello")
  puts "#{greeting}, #{name}!"
end

greet(name: "Alice") # Output: Hello, Alice!
greet(name: "Bob", greeting: "Hi") # Output: Hi, Bob!

```

"Blocks" are similar to lambda or anonymous functions

```ruby
# takes a block as an argument (indicated by the & before the block parameter).
def my_function(&block)
  # Do something with the block
  block.call(5,6)
  # could also use:
  # yield 5,6
end

# the block is defined between 'do' and 'end'
result = my_function do |x,y|
  x * y
end

puts result  # Output: 30
```
Ruby has lambdas as well, which can be assigned to a variable

```ruby
times_two = ->(x) { x * 2 }
times_two.call(10) # returns 20
# lambdas can be invoked using the following alternative syntax:
# my_lambda.call
# my_lambda.()
# my_lambda[]
# my_lambda.===
```

# Variables

## Declaration/Assignment
```ruby
# no need to declare the variable type before assigning a value
some_variable = 123

# nil conditional assignment
x = nil
x ||= 10  # x will now be 10

y = 5
y ||= 20  # y remains 5, as it already has a value
```

## Ternary Operator
```ruby
# someVariable = 4535 if (otherVariable > 0) else 0
some_variable = other_variable > 0 ? 4535 : 0
```

## Global Variables
```ruby
$global_variable = "I am a global variable"

def changes_global
  $global_variable = "I am a changed global variable"
end

puts $global_variable
changes_global
puts $global_variable # prints "I am a changed global variable"
```

## Data Basic Types
```ruby
my_int = 1
puts my_int.class == Integer  # => true

my_float = 1.0284
puts my_float.class == Float  # => true

my_bool = false || true
puts my_bool.class == TrueClass  # => true

some_var = nil  # Ruby's equivalent of Python's None
```
### symbols

Symbols are unique, immutable objects that represent identifiers. They are often used as keys in hashes and as method names. The symbol value starts with a colon ":", eg. :my_symbol

**Key characteristics:**

- _Immutable_: Once created, a symbol cannot be changed.
- _Unique_: Two symbols with the same name are actually the same object.
- _Efficient_: Symbols are more efficient than strings in terms of memory usage.

```ruby
somevar = :my_symbol
```

## Data Basic Type Comparison
```ruby
a == b # compare if they are equal (both must be the same type)
a != b # compare if they are not equal (both must be the same type)
a > b
a >= b
a < b
a <= b
a <=> b # Returns -1 if the left operand is less than the right, 0 if they are equal, and 1 if the left operand is greater than the right.
/^hello/ === "hello world" #Identity operator
```

## Logical Operators (Boolean Operands)
```ruby
a && b
a || b
!a
```

# Data Structures

## list/array
```ruby
my_list = []  # or my_list = Array.new
my_list = [1, 2, 3]
my_list[2] == 3  # => true
my_list[1..] == [2, 3]  # => true, slice list from an index to the end
my_list[0..1] == [1, 2]  # => true, slice list from first element up to an index
my_list[-2..-1] == [2, 3]  # => true, last 2 elements
my_list << 4  # append value, my_list == [1, 2, 3, 4]
my_list.insert(0, 'start')  # insert(index, valueToInsert), lists can contain dissimilar types
my_list.pop  # my_list == ['start', 1, 2, 3]
my_list.concat([10, 20, 30])  # my_list == ['start', 1, 2, 3, 10, 20, 30]
my_list.shift(2)  # remove first 2 elements, equivalent to del my_list[:2]

#These are convenient ways to define arrays of things without using quotes etc. for each element.
#The delimiters [ and ] can be replaced with variations, like ( and ), |, !, etc
str_list = %w[blah foo bar baz] # is equivalent to ["blah", "foo", "bar", "baz"].
symbol_list = %i[blah foo bar baz] # is equivalent to [:blah, :foo, :bar, :baz].

# check length
my_list.length == 5  # => true

# iteration
my_list.each do |element|
  process_value(element)
end

# check if value is in list (basic data types)
my_list.include?(10)  # => true

# find matching values in list
matches = my_list.select { |x| fulfills_some_condition(x) }

# list comprehension (using map)
doubled_chars = 'abcdefg'.chars.map { |c| c * 2 }  # doubledChars == ['aa', 'bb', 'cc', 'dd', 'ee', 'ff', 'gg']
```

## dictionary
```ruby
my_hash = {}
my_hash = { a: 1, 'b': 2 } # the key is actually referenced as :a (a symbol) not 'a' (a string)
my_hash['c'] = 3  # my_hash == {:a=>1, "b"=>2, "c"=>3}
my_hash[:a] == 1  # => true
my_hash[1234] = 5678  # keys can be any hashable type
my_hash.delete("b")  # my_hash == {:a=>1, "c"=>3, 1234=>5678}

# you can also declare a hash with key => value pairs:
my_hash = {
  "name" => "Alice",
  "age" => 30,
  "city" => "New York"
}

# iteration of keys
my_hash.each_key do |key|
  process_value(my_hash[key])
end

# convert hash keys to array
list_of_keys = my_hash.keys  # list_of_keys == [:a, :c]

# iteration of values
my_hash.each_value do |value|
  process_value(value)
end

# check if a key is in the hash
my_hash.key?(:a)  # => true
```

## set
```ruby
require 'set'

my_set = Set.new([1, 2, 3, 2])  # Creates a set with elements 1, 2, and 3
puts my_set  # Output: #<Set: {1, 2, 3}>
my_set.add(4)  # Adds 4 to the set
my_set.delete(2)  # Removes 2 from the set
puts my_set.include?(3)  # Checks if 3 is in the set (returns true)
other_set = Set.new([3, 4, 5])
union_set = my_set | other_set  # Union of two sets
intersection_set = my_set & other_set  # Intersection of two sets
difference_set = my_set - other_set  # Difference between two sets

# iterate over values
my_set.each do |element|
  puts element
end
```

# Control Flow Statements

## if/else
```ruby
if some_variable < 10
  do_this()
elsif some_variable < 20
  do_that()
else
  puts 'fail'
end
```

## for
```ruby
(0..100).each do |i|
  x = do_something(i)
  if x == 123
    break
  else
    do_something_else(x)
    next
  end
end
```

## while
```ruby
while some_boolean_condition
  x = do_something(i)
  if x == 123
    next
  else
    break
  end
end
```

## switch/case
```ruby
def my_method(value)
  case value
  when 1
    puts "One"
  when 2, 3
    puts "Two or three"
  when 4..10
    puts "Between 4 and 10"
  else
    puts "Other value"
  end
end
```


# Exception Handling
```ruby
import traceback
try:
    doSomething()
except Exception as e:
    if type(e) == KeyError:
      createKey('x')
    else:
      traceback.print_exc() # print stack trace
finally:
    cleanup()
```

# Numbers

## Operators
```ruby
x = 10
y = 5
x + y == 15
x - y == 5
x / y == 2
x * y == 50
x ** y == 100000 #exponent
y % x == 5  #modulo

# in-place operators can be used, eg.:
x += 1 # x = x + 1
x *= 4 # x = x * 4
```

## Absolute Value
```ruby
abs(-x) == 10
```

## Min/Max
```ruby
min(x,y) == 5
max(x,y) == 10
```

## Floor/Ceiling
```ruby
import math
f = 3.634
math.floor(f) == 3
math.ceil(f) == 4
```

## Convert Between Int/Float
```ruby
int(f) == 3
float(x) == 10.0
```

## Parse Number from String
```ruby
int("0x11",16) == 17
int("27") == 27
```

## Random
```ruby
import random
# cryptographic operations should use import secrets
random.randint(0,100) // outputs int between 0 and 100 inclusive
random.random() // outputs float between 0.0 and 1.0
# random string
pw = ''.join([secrets.choice('ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890-_#$%^&()!') for c in range(20)])
```

# Strings

## Literals
```ruby
singleLineStr = "single line" # can use single or double quotes as delimiter
multiLineStr = '''multi line
string'''
```

## Concatenation
```ruby
str1 + str2
```

## Formatting
```ruby
msg0 = f"Something {str1} and string {x.foo()}"
msg1 = 'An int {0} and string {1}'.format(123,'test') # msg1 == 'An int 123 and string test'
msg2 = "Formatting examples, an int {}, a float to 3 places {:.3f}, binary {:08b} and hex {:02x}".format(
     3, 1045.89735, 0x2F, 0b11110101) # msg2 == 'Formatting examples, an int 3, a float to 3 places 1045.897, binary 00101111 and hex f5'
msg3 = f"{someByteValue :08b}" # '11011101'
```

## Convert Types to a String Representation
```ruby
str(someVariable)
repr(someVariable)
f"This will obtain a string representation of {someVariable}"
```

## Strip/Trim
```ruby
x = ',a,b,c,,'
x.strip() # will remove whitespace at both ends of the string
x.strip(',') # will remove the specified character from both ends, in this case the output == 'a,b,c'
x.lstrip(',') # will remove the specified character from start, in this case the output == 'a,b,c,,'
x.rstrip(',') # will remove the specified character from end, in this case the output == ',a,b,c'
```

## Split
```ruby
x = ',a,b,c,,'
xa = x.split(',') # xa == ['', 'a', 'b', 'c', '', '']
```

## Replace
```ruby
x = ',a,b,c,,'
x.replace(',',':') == ':a:b:c::'
y = 'The Trees'
y.replace('Trees','Forest') == 'The Forest'
```
## StartsWith
```ruby
x.startswith(',') // True
```

## EndsWith
```ruby
x.endswith(',') // True
```

## Find a Substring
```ruby
'Trees' in y == True
y.find('Trees') == 4  # find first index of substring
```

## Select a Substring
```ruby
y[4:] == 'Trees'
y[4:6] == 'Tr'
```

## Regex
```ruby
import re

# find a pattern
m = re.search('[a-z]+', stringToSearch)
if(m):
    process(m.group(0)) #.group() returns the string matched but only finds first occurrence

# match multiple groups
line = "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)"
m = re.search('([0-9\.]+).+GET (.+) HTTP', line)
ipaddr = m.group(1)
url = m.group(2)

#find all matches of regex in a string, m will be a list
m = re.findall("http:\/\/([a-zA-Z0-9\-\.]+)",httpresponseString)
for i in range(len(m)):
     print(m[i])
```

# Binary Bytes/Buffers

## Declaration
```ruby
key = 0x65               #hex literal, assigned to an int variable
val = 0b01101101     #binary literal
header = bytes([0xFF,0xC3,0x00,0x00])     #bytes are immutable
header = b'\xff\xc3\x00\x00'  #alternate bytes literal
data = bytearray()      #bytearrays are mutable
```

## Bytearray/Buffer Operations
```ruby
data.append(0xFF)  #append as per list
data.extend([0xE7,0x3A,0x44,0x0C]) #extend as per list
# all other operations as per list
```

## Bitwise Operations

```ruby
a = 0b11011001
b = 0b10011101

```

### And
```ruby
# a & b
f"{a & b:08b}" == '10011001'
```

### Or
```ruby
# a | b
f"{a | b :08b}" == '11011101'
```

### Xor
```ruby
# a ^ b
f"{a ^ b :08b}" == '01000100'
```

### Not
```ruby
# a ^ 0xFF
f"{a ^ 0xFF :08b}" == '00100110'
```

### Shift Left/Right
```ruby
# a >> 4, or a << 5
f"{a >> 4 :08b}" == '00001101'
```

# CLI Argument Parsing
```ruby
# simple arguments use sys.argv:

import sys
#sys.argv[0] could be the filename of the script if it was executable
#or sys.argv[0] could be 'python' and sys.argv[1] is the script filename
#example script invocation command line:
#$ python3 /tmp/test.py argone argtwo
sys.argv == ['/tmp/test.py', 'argone', 'argtwo']
sys.argv[1] == 'argone'

# argparse is more appropriate for release quality tools:

import argparse  # python standard library
# usage: test-argparse.py [-h] [-s] [-v VALUEOFSOMETHING] target
parser = argparse.ArgumentParser(description='Process some stuff.')

#optional flag
parser.add_argument("-s","--someflag", help="this is an optional flag",action="store_true")

#optional value
parser.add_argument("-v","--valueofsomething", help="this is an optional argument that takes a value")

#required value
parser.add_argument("target", help="this is required argument that takes a value")

args = parser.parse_args()

if args.someflag:
    print('someflag option is true')

if args.valueofsomething:
    print('valueofsomething = '+args.valueofsomething)

print('target = '+args.target)
```

# Execute Shell Commands
```ruby
import subprocess
process = subprocess.Popen(['echo', '"Hello stdout"'], stdout=subprocess.PIPE)
stdout = process.communicate()[0]
print(stdout)
#can also merge STDERR/STDOUT
    process = subprocess.Popen(['ls', '-al','/usr/bin'], stdout=subprocess.PIPE,stderr=subprocess.STDOUT)
```

# File Operations
```ruby
# file modes (r+ == read/write) (r == read) (w == overwrite) (a == append) (w+ == write/create if not exist)
```
## Open File and Read
```ruby
# read text lines
with open('c:\\temp\\pB.txt', 'r') as fB:
    filelines = fB.readlines() # an array of strings, one element per file line

# read binary data
chunk_size = 4096 # 4 KiB
with open("/path/to/inputfile", "rb") as in_file:
    while True:
        chunk = in_file.read(chunk_size)
		processChunk(chunk)
        if chunk == '':
		    break # EOF
```

## Open File and Write
```ruby
# write text
with open('c:\\temp\\file.txt', 'w') as f:
    f.write('First line\n')
    f.write('Second line\n')

# write binary data
outdata = b'\xF0\xF1\xF2\xF0\xF0\xF4\x00'
with open('/tmp/od1.dat','wb') as outfile:
    outfile.write(outdata)
```

## Skip To File Location
```ruby
# 'whence' is optional and specifies positioning
# default is 0 == absolute file positioning
# 1 == seek relative to the current position
# 2 == seek relative to the file's end.
fileObject.seek(offset_num_bytes, whence)
```

## Close File
```ruby
fileObject.close()
```

# Networking

## Sockets
```ruby
# client send/receive

import socket
HOST = 'daring.cwi.nl'    # The remote host
PORT = 50007              # The same port as used by the server
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))
s.sendall(bytearray("Hello, world","ascii"))
data = s.recv(1024)
s.close()
print 'Received', repr(data)

# server listen/reply

import socket
HOST = ''                 # Symbolic name meaning all available interfaces
PORT = 50007              # Arbitrary non-privileged port
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((HOST, PORT))
s.listen(1) # arg is backlog, which specifies the number of unaccepted connections that the system will allow before refusing new connections
conn, addr = s.accept()
print('Connection from', addr)
while 1:
    data = conn.recv(1024)
    if not data:
        break
    conn.sendall(data)
conn.close()
```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client
```ruby
import urllib.request
# GET
with urllib.request.urlopen('http://www.python.org/') as f:
     print(f.read())

# POST
postdata = 'a=1&b=test'
with urllib.request.urlopen('https://webhook.site/0b116795ff',data=postdata.encode()) as f:
     print(f.read())

# if you need to set headers:
jobj = {'a':1}
jsondata = json.dumps(jobj)
req = urllib.request.Request('https://webhook.site/0b116795ff',data=jsondata.encode(),headers={'Content-Type':'application/json'})
with urllib.request.urlopen(req) as f:
     print(f.read())

```

### HTTP server
```ruby
from http.server import ThreadingHTTPServer, BaseHTTPRequestHandler

class MyRequestHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        print(handler.client_address[0]) # remote client IP addr
        print(handler.headers['X-Custom-Header'])
        print(handler.path) # requested URI path eg. "/api/v2/user"
        handler.send_response(200)
        handler.send_header('Content-Type:', 'text/html')
        handler.end_headers()
        handler.wfile.write('<h1>Hello World</h1>'.encode('utf-8'))
        handler.close_connection = True

server_address = (LISTEN_ADDR, PORT)
httpd = ThreadingHTTPServer(server_address, MyRequestHandler)
httpd.serve_forever()
```

# JSON Serialization

## Serialize
```ruby
import json
mydict = {'a':[1,2,3],'b':'test'}
json.dumps(mydict) == '{"a": [1, 2, 3], "b": "test"}'
```

## Deserialize
```ruby
import json
jsonstr = '{"a": [4, 5, 6], "b": "foo"}'
json.loads(jsonstr) == {'a': [4, 5, 6], 'b': 'foo'}
```

# Object Introspection
```ruby
type(obj)
dir(obj) #display obj's attributes,methods,variables
var(obj) #similar to dir()
```

# Date/Time
```ruby
#format will be 2016-06-05 18:46:37.879761

import datetime
datetimestring = str(datetime.datetime.now())
# the above is the same as
datetime_object.strftime('%Y-%m-%d %H:%M:%S.%f')

#parse datetime object from string
datetime_object = datetime.datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')

#arithmetic
delta = datetime.timedelta(days=7,hours=8,minutes=5,seconds=27,milliseconds=29000,microseconds=10)
datetime_object + delta
datetime_object - delta

#epoch time format in seconds

import time
str(int(time.time()))
```

# Sleep
```ruby
import time
time.sleep(2.5) # seconds
```

# Multithreading
The threading module runs virtual threads in a single real thread, for true multi-threading use the multiprocesing module and replace 'threading' with 'multiprocessing'
## Threads
```ruby
import threading

def MyThreadFunc(param1):
    runLongProcess(param1)

t = threading.Thread(target=MyThreadFunc, kwargs={'param1':1234})
t.start()
t.join()
```

## Synchronization/Locking Primitives
```ruby
import threading
mutex = threading.Lock()

def processData(data):
    mutex.acquire()
    try:
        print('Do some stuff')
    finally:
        mutex.release()

# threadsafe data structs:
# standard list(), dict(), set()

# multiprocessing requires specific data structs that are threadsafe:
# eg. multiprocessing.Manager().list()
# eg. multiprocessing.Manager().dict()
```
