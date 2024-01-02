# Example YAML file
```yaml
---
# A YAML doc begins with three dashes and ends with three dots
# A YAML file can have multiple documents, each beginning with three dashes and ending with three dots
# if there is only one doc in the YAML file, the --- and ... are optional
# YAML uses indenting for data structure but you need to use spaces, not tabs
things:
	example_map:
		name: foobar
		somevalue: abc
		someothervalue: 123
something_else:
	- 10
	- 20
# the equivalent data struct is:
# { 'things': {'example_map':{'name':'foobar','somevalue':'abc','someothervalue':123}}, 'something_else':[10,20]}
...
```

# Comments
```yaml
# this is single line comment, there are no multiline comments
# single line comments include everything from the hash to the end of the line
# single line comments are treated as whitespace and can appear anywhere in a YAML doc except between the colon and value in 'key: value'
```

# Classes

YAML doesn't have classes, but anchors and aliases can be used to reduce repetition

## Declaration
```yaml
things:
	example_map_template: &mytemp1
		name: override_me
		somevalue: abc
		someothervalue: 123

	another_map_template: &mytemp2
		something1: blah
		something2: test
	
	thing1:
		<<: *mytemp1 # the "<<" means merge properties from another mapping into this mapping
		name: thing1_name
		# thing1 is { name: thing1_name, somevalue: abc, someothervalue: 123 }

	thing2:
		<<: [*mytemp1, *mytemp2] # merge multiple mappings
		something3: aaaaa
		# thing2 is { name: override_me, somevalue: abc, someothervalue: 123, something1: blah, something2: test, something3: aaaaa}
```

# Variables

## Declaration/Assignment
```yaml
somekey: somevalue
somekey: !!str 12345  # explicit type casting
spaces are allowed in key names: somevalue
? "another way of writing a key"
: somevalue

# using references:
somevalues:
   - &someref  A scalar value to reuse
   - *someref  # Literal "A scalar value to reuse" is inserted here
   # the above data struct is equivalent to { somevalues: ["A scalar value to reuse","A scalar value to reuse"]}
```

## Data Basic Types
```yaml
an_int_value: 1        # integer          
a_float_value: 1.234    # float      
an_octal: 02472256
a_hexadecimal: 0x0A_74_AE # Any "_" characters in a numeric type are ignored, allowing a readable representation of large values.
a_binary: 0b1010_0111_0100_0101_1101
an_explicit_float_type: !!float 123 # also a float via explicit data type prefixed by (!!)

ex_str_1: 'abc'    # string
ex_str_2: "abc"                   
ex_str_3: strings do not require quotes unless the value defaults to a certain type or has certain characters at the start
an_explicit_str_type: !!str 2015-04-05 # type casting is required because the parser will interpret it as a date
or_you_could_use_quotes: "2015-04-05"
an_explicit_str_type: !!str 123 # a string, disambiguated by explicit type
or_you_could_use_quotes: "123"
an_explicit_str_type: !!str Yes # a string via explicit type
or_you_could_use_quotes: "Yes"

a_boolean_value: false    # boolean type 
another_boolean_value: Yes # a boolean True (yaml1.1), string "Yes" (yaml1.2)
h: Yes we have No bananas  # a string, "Yes" and "No" are disambiguated by their context.

a_date_value: 2015-04-05   # date type

a_timestamp: 2001-12-15T02:59:43.1Z  # ISO 8601

# Many implementations of YAML can support user-defined data types for object serialization. 
# Local data types are not universal data types but are defined in the application using the YAML parser library. 
# Local data types use a single exclamation mark (!). 

myObject: !myClass { name: Joe, age: 15 }

a_binary_value: !binary |
 R0lGODlhDAAMAIQAAP//9/X17unp5WZmZgAAAOfn515eXvPz7Y6OjuDg4J+fn5
 OTk6enp56enmlpaWNjY6Ojo4SEhP/++f/++f/++f/++f/++f/++f/++f/++f/+
 +f/++f/++f/++f/++f/++SH+Dk1hZGUgd2l0aCBHSU1QACwAAAAADAAMAAAFLC
 AgjoEwnuNAFOhpEMTRiggcz4BNJHrv/zCFcLiwMWYNG84BwwEeECcgggoBADs=
 
# This mapping has four keys,
# "empty" has an empty value, the others represent null.
empty:
canonical: ~
english: null
~: null key
```

# Data Structures

## array/list
```yaml
# using dashes/hyphens
- one
- two
- three
# the above is ['one','two','three']

# using inline, JSON compatible notation
hooo : [one, two, three]
# the above is {'hooo': ['one', 'two', 'three']}

# an array of dictionaries
- item    : Super Hoop
  quantity: 1
- item    : Basketball
  quantity: 4
- item    : Big Shoes
  quantity: 1
# the above is [{'item': 'Super Hoop', 'quantity': 1}, {'item': 'Basketball', 'quantity': 4}, {'item': 'Big Shoes', 'quantity': 1}]
```

## dictionary

Known in YAML as a Mapping, it is an associative container, where each key is unique in the association and mapped to exactly one value. YAML places no restrictions on the type of keys; in particular, they are not restricted to being scalars. Example bindings include Perl’s hash, Python’s dictionary, and Java’s Hashtable. 

```yaml
# Unordered set of key: value pairs.
Block style: !!map # the explicit type cast is optional
  Clark : Evans
  Brian : Ingerson
  Oren  : Ben-Kiki
Flow style: !!map { Clark: Evans, Brian: Ingerson, Oren: Ben-Kiki } # the explicit type cast is optional

# above is equivalent to JSON:
# {"Block style": {"Clark":"Evans","Brian":"Ingerson","Oren":"Ben-Kiki"}}
# {"Flow style": {"Clark":"Evans","Brian":"Ingerson","Oren":"Ben-Kiki"}}
```

## set
```yaml
# Sets are represented as a
# Mapping where each key is
# associated with a null value
--- 
!!set
? Mark McGwire
? Sammy Sosa
? Ken Griffey
# equivalent to Set object {'Mark McGwire','Sammy Sosa','Ken Griffey'}
# some implementations may result in a map with null keys
```

# Strings

## Literals
```yaml
# multiline literals

data: |
   There once was a tall man from Ealing
       It said on the door

data2: >
   Wrapped text
   will be folded
   into a single
   paragraph

   Blank lines denote
   paragraph breaks

# 'data': 'There once was a tall man from Ealing\nWho got on a bus to Darjeeling\n    It said on the door\n', 
# 'data2': 'Wrapped text will be folded into a single paragraph\nBlank lines denote paragraph breaks\n'}

content: |+                      
   Arbitrary free text with newlines after
# 'content' : 'Arbitrary free text with newlines after\n\n'

content: |-
   Arbitrary free text without newlines after it
# 'content' : 'Arbitrary free text without newlines after'

code: |- # versus key "code" having value 'url: "https://..."'
    url: "https://example.com"
# 'code' : 'url: "https://example.com"'
```
