# Example Skeleton Main Program
```python
import datetime
def main():
    print('The date/time is',datetime.datetime.now())

if __name__ == "__main__":
    main()
```

# Comments
```python
# single line comment
''' multi line...
    ...comment '''
```

# Console Input/Output

## Output
```python
print("message",variable,"another literal")
```

## Input
```python
person = raw_input('Enter your name: ')
```

# Namespaces
Python doesn't have namespaces. Use modules as a namespace. For example create `my_module.py`:
```python
class foo(object):
   ...
```
Then import it and reference its internal members:
```python
import my_module
f = my_module.foo()
```

# Classes

## Declaration
```python
class MyClass():

    def __init__(self, param1 = 0):
        self.bar = param1

    def DoSomething(self):
        print(self.bar)

    def GetSomething(self):
        return 123
```

## Inheritance
```python
class MySubClass(MyClass):

    def __init__(self):
       super().__init__(123)

	def DoSomething(self):
		print(super().DoSomething())
        print("In MySubClass.DoSomething()")
```

## Instantiation and Method Invocation
```python
mc = MyClass(1000)
mc.bar == 1000 # True
mc.DoSomething()
```

# Functions
```python
def MyFunction(argument1, argument2='defaultvalue'):
    print('arguments', argument1, argument2)
	return str(argument1) + argument2
```

# Variables

## Declaration/Assignment
```python
# no need to declare the variable type before assigning a value
someVariable = 123
```

## Ternary Operator
```python
# python3 doesn't have a ternary operator but you can use:
someVariable = 4535 if (otherVariable > 0) else 0
```

## Global Variables
```python
globalVar1 = 12345

def ChangesGlobalVar(val):
    global globalVar1
	globalVar1 = globalVar1 + val
```

## Data Basic Types
```python
my_int = 1
type(my_int) == int  # True

my_float = 1.0284
type(my_float) == float # True

my_bool = False or True
type(my_bool) == bool # True

some_var = None  #python version of null
```

## Data Basic Type Comparison
```python
a == b # compare if they are equal (both must be the same type)
a != b # compare if they are not equal (both must be the same type)
a > b
a >= b
a < b
a <= b
```

## Logical Operators (Boolean Operands)
```python
a and b
a or b
not a
```

# Data Structures

## list/array
```python
my_list = list()
my_list = [1,2,3]
my_list[2] == 3 # True
my_list[1:] == [2,3] # True, slice list from an index to the end
my_list[:2] == [1,2] # True, slice list from first element up to an index
my_list[-2:] == [2,3] # True, last 2 elements
my_list.append(4) # my_list == [1,2,3,4]
my_list.insert(0,'start') # insert(atIndex,valueToInsert), lists can contain dissimilar types
my_list.pop() # my_list == ['start',1,2,3]
my_list.extend([10,20,30]) # my_list == ['start',1,2,3,10,20,30]
del my_list[:2] # my_list == [2,3,10,20,30]

# check length
len(my_list) == 5 # True

# iteration
for element in my_list:
    processValue(element)

# check if value is in list (basic data types)
if 10 in my_list: # True

# find matching values in list
matches = [x for x in my_list if fulfills_some_condition(x)]

# list comprehension
doubledChars = [c+c for c in 'abcdefg'] # doubledChars == ['aa', 'bb', 'cc', 'dd', 'ee', 'ff', 'gg']
```

## dictionary
```python
my_dict = dict()
my_dict = {'a':1, 'b':2}
my_dict['c'] = 3 # my_dict == {'a':1, 'b':2, 'c':3}
my_dict['a'] == 1 # True
my_dict[1234] = 5678 # keys can be any hashable type
del mydict['b'] # my_dict == {'a': 1, 'c': 3, 1234: 5678}

# iteration of keys
for key in my_dict:
    processValue(my_dict[key])

# convert dictionary keys to list
list_of_keys = list(my_dict.keys()) # list_of_keys == ['a','c']

# iteration of values
for val in my_dict.values():
    processValue(val)

# check if a key is in dictionary
if 'a' in my_dict: # True
```

## set
```python
set0 = set([1,2,3,'a','b','c']) # can initialise with a list
set0 = {1, 2, 3,'a','b','c'} # alternative initialise syntax
my_set = set()
my_set.add('a') # my_set == {'a'}
my_set.update(['b','c','d']) # my_set == {'a','b','c','d'}
my_set.remove('a') # my_set == {'b','c','d'}
my_set.union(set0) == {1, 2, 3,'b','a','c','d'} # True
my_set.intersection(set0) == {'b', 'c'} # True
my_set.difference(set0) == {'d'} # True

# check if value is in set (basic data types)
if 'd' in my_set: # True

# enumeration/iteration
for element in my_set:
    processValue(element) 
```

# Control Flow Statements

## if/else
```python
if someVariable < 10:
    doThis()
elif someVariable < 20:
    doThat()
else:
    print('fail')
```

## for
```python
for i in range(0,101):
    x = doSomething(i)
	if x == 123:
	    continue
	else:
	    break
```

## while
```python
while someBooleanCondition:
    x = doSomething(i)
	if x == 123:
	    continue
	else:
	    break
```

## switch/case
python doesn't have case/switch statements, but it can be emulated using a dictionary and lambdas or function objects
```python
def caseThree(arg):
    print('case three',arg)
	
cases = {
  'one': lambda arg: print('case one',arg),
  'two': lambda arg: print('case two',arg),
  'three': caseThree
}

inputCondition = 'two'
if inputCondition in cases:
    cases[inputCondition](12345) # prints "case two 12345"
```


# Exception Handling
```python
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
```python
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
```python
abs(-x) == 10
```

## Min/Max
```python
min(x,y) == 5
max(x,y) == 10
```

## Floor/Ceiling
```python
import math
f = 3.634
math.floor(f) == 3
math.ceil(f) == 4
```

## Convert Between Int/Float
```python
int(f) == 3
float(x) == 10.0
```

## Parse Number from String
```python
int("0x11",16) == 17
int("27") == 27
```

## Random
```python
import random
# cryptographic operations should use import secrets
random.randint(0,100) // outputs int between 0 and 100 inclusive
random.random() // outputs float between 0.0 and 1.0
# random string
pw = ''.join([secrets.choice('ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890-_#$%^&()!') for c in range(20)])
```

# Strings

## Literals
```python
singleLineStr = "single line" # can use single or double quotes as delimiter
multiLineStr = '''multi line
string'''
```

## Concatenation
```python
str1 + str2
```

## Formatting
```python
msg0 = f"Something {str1} and string {x.foo()}"
msg1 = 'An int {0} and string {1}'.format(123,'test') # msg1 == 'An int 123 and string test'
msg2 = "Formatting examples, an int {}, a float to 3 places {:.3f}, binary {:08b} and hex {:02x}".format(
     3, 1045.89735, 0x2F, 0b11110101) # msg2 == 'Formatting examples, an int 3, a float to 3 places 1045.897, binary 00101111 and hex f5'
msg3 = f"{someByteValue :08b}" # '11011101'
```

## Convert Types to a String Representation
```python
str(someVariable)
repr(someVariable)
f"This will obtain a string representation of {someVariable}"
```

## Strip/Trim
```python
x = ',a,b,c,,'
x.strip() # will remove whitespace at both ends of the string
x.strip(',') # will remove the specified character from both ends, in this case the output == 'a,b,c'
x.lstrip(',') # will remove the specified character from start, in this case the output == 'a,b,c,,'
x.rstrip(',') # will remove the specified character from end, in this case the output == ',a,b,c'
```

## Split
```python
x = ',a,b,c,,'
xa = x.split(',') # xa == ['', 'a', 'b', 'c', '', '']
```

## Replace
```python
x = ',a,b,c,,'
x.replace(',',':') == ':a:b:c::'
y = 'The Trees'
y.replace('Trees','Forest') == 'The Forest'
```
## StartsWith
```python
x.startswith(',') // True
```

## EndsWith
```python
x.endswith(',') // True
```

## Find a Substring
```python
'Trees' in y == True
y.find('Trees') == 4  # find first index of substring
```

## Select a Substring
```python
y[4:] == 'Trees'
y[4:6] == 'Tr'
```

## Regex
```python
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
```python
key = 0x65               #hex literal, assigned to an int variable
val = 0b01101101     #binary literal
header = bytes([0xFF,0xC3,0x00,0x00])     #bytes are immutable
header = b'\xff\xc3\x00\x00'  #alternate bytes literal
data = bytearray()      #bytearrays are mutable
```

## Bytearray/Buffer Operations
```python
data.append(0xFF)  #append as per list
data.extend([0xE7,0x3A,0x44,0x0C]) #extend as per list
# all other operations as per list
```

## Bitwise Operations 

```python
a = 0b11011001
b = 0b10011101

```

### And
```python
# a & b
f"{a & b:08b}" == '10011001'
```

### Or
```python
# a | b
f"{a | b :08b}" == '11011101'
```

### Xor
```python
# a ^ b
f"{a ^ b :08b}" == '01000100'
```

### Not
```python
# a ^ 0xFF
f"{a ^ 0xFF :08b}" == '00100110'
```

### Shift Left/Right
```python
# a >> 4, or a << 5
f"{a >> 4 :08b}" == '00001101'
```

# CLI Argument Parsing
```python
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
```python
import subprocess
process = subprocess.Popen(['echo', '"Hello stdout"'], stdout=subprocess.PIPE)
stdout = process.communicate()[0]
print(stdout)
#can also merge STDERR/STDOUT
    process = subprocess.Popen(['ls', '-al','/usr/bin'], stdout=subprocess.PIPE,stderr=subprocess.STDOUT)
```

# File Operations
```python
# file modes (r+ == read/write) (r == read) (w == overwrite) (a == append) (w+ == write/create if not exist)
```
## Open File and Read
```python
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
```python
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
```python
# 'whence' is optional and specifies positioning
# default is 0 == absolute file positioning
# 1 == seek relative to the current position 
# 2 == seek relative to the file's end.
fileObject.seek(offset_num_bytes, whence)
```

## Close File
```python
fileObject.close()
```

# Networking

## Sockets
```python
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
```python
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
```python
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
```python
import json
mydict = {'a':[1,2,3],'b':'test'}
json.dumps(mydict) == '{"a": [1, 2, 3], "b": "test"}'
```

## Deserialize
```python
import json
jsonstr = '{"a": [4, 5, 6], "b": "foo"}'
json.loads(jsonstr) == {'a': [4, 5, 6], 'b': 'foo'}
```

# Object Introspection
```python
type(obj)
dir(obj) #display obj's attributes,methods,variables
var(obj) #similar to dir()
```

# Date/Time
```python
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
```python
import time
time.sleep(2.5) # seconds
```

# Multithreading
The threading module runs virtual threads in a single real thread, for true multi-threading use the multiprocesing module and replace 'threading' with 'multiprocessing'
## Threads
```python
import threading

def MyThreadFunc(param1):
    runLongProcess(param1)

t = threading.Thread(target=MyThreadFunc, kwargs={'param1':1234})
t.start()
t.join()
```

## Synchronization/Locking Primitives
```python
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
