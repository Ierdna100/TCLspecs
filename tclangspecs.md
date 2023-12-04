# TC Lang (TCL) specifications - Version 0.5

## Table of contents

1. [General Specifications](#general-specifications)  
2. [Variable declaration](#variable-declaration)  
3. [Array declaration](#array-declaration)
4. [Array indexing](#array-indexing)
5. [Memory management](#memory-management)
6. [Data types](#types)
7. [Flow control](#flow-control)
8. [User defined methods](#methods)
9. [Casting](#casting)
10. [Operands](#operands)
11. [Operator precedence](#operator-precedence)  
12. [Built-in methods](#reserved-built-in-methods)
13. [Literals](#literal-declaration)
14. [Usage of goto](#usage-of-goto)
15. [Full lists of specified reserved words](#full-list-of-reserved-words)  
16. [Preprocessor directives](#preprocessor-directives)

## General specifications

Statement separator: `;`  
Comments: `//`

Everything runs on the top level method for our needs. Internally do whatever you want.

- You MUST push the stack when your program starts and pop it when your program exits.
- If you use goto exiting a function call or such, you must leave a clean stack behind as if the code executed normally.

## Variable declaration

Declaring a variable consists of:

`[const] [Type] [Var Name] = [Literal / Other Var Name]`

`const`: Defines a variable as immutable  
`Type`: Type of the variable  
`var name`: The name of the variable, may not start with a digit and may only contain alphanumeric characters and underscores.  
`Literal / Other var name`: Value of variable

```c
const int two = 2;
```

Undefined variable declarations are also valid (applies to any data type):
```c
int two;
MyStruct myStruct;
```

## Array declaration

`<const> <type>[<array length>] <var name> = { <comma-separated elements> }`

```c
const int[3] arr1 = {13, 14, 15};
```

Dynamic array:
```c
len = 3;
int[len] arr2;

arr2[0] = 13;
arr2[1] = 14;
arr2[2] = 15;
```

## Array indexing

```c
const int[3] arr = {13, 14, 15};

arr[2]; // is equal to 15
```

## Memory management

### Pointer referencing:

Let `x = 5` at mem address `0d240`
```c
int ptr = *x; // ptr = 240
```

### Value dereferencing:
```c
int val = &ptr; // val = 5
```

### Allocating and freeing memory:

See: [List of reserved methods](#reserved-built-in-methods)

## Types

`int`: integer with a length to be specified later  
`char`: 8 bit wide integer useful for whatever requires knowing it is a character  
`bool`: 1 bit wide integer useful for not confusing it with other stuff  
`struct`: defines a class-like structure that may hold variables and methods.

Possibly for later implementations:  
`float`: floating point number with a length to be specified later  

### Special non-primitive types that are part of the language
#### `string`
`string`: Array of chars

#### `struct`
**Note: structs have to be defined individually, not inline**  
`struct`: 
```c
struct Struct1 = {
    int val3;
}

struct Type {
    int val1;
    int val2;

    int method1() {

    }

    Struct1 iveranoutofnames;
}
```

Accessing memebers of structs

From pointer of a struct `struct1`:
```c
int val = struct1->val1;
```

From a struct `struct1`:
```c
int val = struct1.val1;
```

You may also stack struct accessers, let struct `struct1`:
```c
int val = struct1->val1.val2;
int val = struct1.val1.val2;
```

You may reference to the current struct instance in a method by the keyword `this`:
```c
struct Type {
    int num1;
    int num2;

    void method1(int num1) 
    {
        this.num1 = num1;
    }
}
```
#### `enum`

`enum`:  
With specific values for each key:
```c
enum HTTPMethods {
    GET = 0,
    POST = 1,
    TRACE = 2
}
```

With ordered values for each key, starting at 0:
```c
enum HTTPMethods {
    GET,
    POST,
    TRACE
}
```

## Flow Control

### With blocks:
`if` statements
```c
if (boolean == true) 
{

}
elseif (boolean == true)
{

}
else
{

}
```

`while` loops
```c
while (boolean == true)
{

}
```

`for` loops
```c
for (statement1; statement2; statement3;)
{

}
```
`statement1`: initial state of the iterator  
`statement2`: condition for for loop to run  
`statement3`: operation on iterator at the end of the loop (including when called with `continue`)

The first statement may be omitted, we then assume the iterator is the variable name provided in the 2nd statement and is an integer:
```c
for (; statement2; statement3;)
{

}
```

The first and third statements may be omitted, we then assume the iterator is the variable name provided in the 2nd statement and is an integer, and that at the end of the loop we do `iterator++`:
```c
for (; statement2;;)
{

}
```

### Without blocks:
`if` statements
```c
if (boolean == true) 
    var += 1;
elseif (boolean == true)
    var += 2;
else
    var -= 1;
```

`while` loops
```c
while (boolean == true)
    method();
```

`for` loops
```c
for (statement1; statement2; statement3;)
    method();
```

`break`: Inside of a loop, exits the loop  
`continue`: Inside of a loop, jumps to beginning of loop

## Methods

Does not support:
- Varargs
- Encapsulation
```c
int method(int arg1, int arg2) {
    return arg1 + arg2;
}
```

```c
int val = method(2, 4);
```

If our method is a single statement, we may write it as such:
```c
int method(int arg1, int arg2) arg1 + arg2;

int method(int arg1, int arg2)
    arg1 + arg2;
```

`return` is implicit, but you may still specify it (it looks pretty):
```c
int method(int arg1, int arg2) return arg1 + arg2;

int method(int arg1, int arg2)
    return arg1 + arg2;
```

## Casting

Casting may always be done to a higher order type, and may be specified as allowed to a lower order type.

Order of types:
1. bool
2. int
3. char
4. float

Allowing casts:

```c
int num = 1;
bool b1 = (bool) num; // If non-zero is true like in C++
```

## Operands

| Operand | Symbol |
| --- | --- |
| Addition | `+` |
| Subtraction | `-` |
| Multiplication | `*` |
| Division | `/` |
| Modulo | `%` |
| Negation | `-` |
| Bitwise OR | `\|` |
| Bitwise AND | `&` |
| Bitwise XOR | `^` |
| Bitwise NOT | `~` |
| Logical OR | `\|\|` |
| Logical AND | `&&` |
| Logical NOT | `!` |
| Equality | `=` |
| Inequality | `!=` |
| Less than | `<` |
| Less than or equal | `<=` |
| Greater than | `>` |
| Greater than or equal | `>=` |
| Assignement | `=` |
| Addition assignement | `+=` |
| Subtraction assignement | `-=` |
| Multiplication assignement | `*=` |
| Division assignement | `/=` |
| Postifx increment | `++` |
| Postfix decrement | `--` |

You are free to implement the other 12 assignement operators but I don't think they're too useful in general, so they're not part of the spec.

## Operator Precedence

Based on C-like languages  
**Note 1: lowest precedence means it gets interpreted first. Highest precedence means it gets interpreted last.**  
**Note 2: parentheses denote a priority over another operation, like in mathematics. In `(2 + 3) / 2`, `2 + 3` is computed first, then `5 / 2`.**  
**Note 3: precedence level 13 is left intentionally empty if we ever want to add ternary operators.**
| Precedence | Operator | Description | Associativity |
| --- | --- | --- | -- |
| 1 | `()` | Function calls | L to R |
| 1 | `[]` | Array indexing | L to R |
| 1 | `.` | Struct member indexing | L to R |
| 1 | `->` | Struct pointer member indexing | L to R |
| 1 | `++` | Postfix increment | L to R |
| 1 | `--` | Postifix decrement | L to R |
| `2` | `! ~` | Logical/bitwise NOT | `R to L` |
| `2` | `-` | Negation | `R to L` |
| `2` | `(type)` | Explicit cast | `R to L` |
| `2` | `*` | Dereference (value from ptr) | `R to L` |
| `2` | `&` | Address-of (ptr from var) | `R to L` |
| `2` | `sizeof` | Size of built-in method | `R to L` |
| 3 | `* / %` | Multiplication and division | L to R |
| `4` | `+ -` | Addition and subtraction | L to R |
| 5 | `<< >>` | Bitwise shifting | L to R |
| `6` | `< <=` | Less than [or equal] | L to R |
| 6 | `> >=` | Greater than [or equal] | L to R |
| `7` | `== !=` | Equality | L to R |
| 8 | `&` | Bitwise AND | L to R |
| `9` | `^` | Bitwise XOR | L to R |
| 10 | `\|` | Bitwise OR | L to R |
| `11` | `&&` | Logical AND | L to R |
| 12 | `\|\|` | Logical OR | L to R |
| `13` |  |  | |
| 14 | `=` | Assignement | `R to L` |
| 14 | `+= -=` | Addition and subtraction assignement | `R to L` |
| 14 | `*= /= %=` | Multiplication and division assignement | `R to L` |

## Reserved (built-in) methods
- `string typeof(var)` - Returns type of variable as string
- `int sizeof(struct)` - Returns size of struct in bytes
- `int lengthof(array)` - Returns length of array
- `int lengthof(string)` - Returns length of string
- `int nextbyte()` - Reads the next byte from TC input stream
- `void outputbyte(byte)`- Outputs the next byte to TC output stream
- `int malloc(size)` - Allocates memory and returns the pointer to the start of the allocated space. Returns -1 if it fails to do so
- `int realloc(ptr, size)` - Reallocates memory at ptr with the new specified size, then returns the pointer. Returns -1 if it fails to do so
- `int calloc(size)` - Allocates memory, sets all memory values to 0, then returns pointer. Returns -1 if it fails to do so
- `void free(ptr)` - Frees memory at pointer
- `int read(hardwareID)` - Returns next int from input buffer
- `void write(hardwareID, int)` - Writes an int to the end of the input buffer
- `int getOutputSize()` - Returns size of output buffer in B
- `int getInputSize()` - Returns size of input buffer in B
- `void memcopy(srcPtr, destPtr, byteSize)` - Copies a chunk of memory to another

Think about:  
TC supports multiple IO types, they are:
- Keyboard
- Network (may be just `outputByte()` as sandbox doesn't actually have an ouput and we need to check what our program does).
- Specific file
- UNIX time (64 bit)
- Sprite display
- Console
- 7-seg displays
- Pixel display

## Literal declaration

`____` denotes any numerical value (including A-F for hex)  
`----` denotes any alphanumeric value  

`0d____`: decimal number (redundant, used for clarity)  
`0b____`: binary number  
`0o____`: octal number  
`0x____`: hexadecimal number  
`____`: decimal number  
`"----"`: String  
`'-'`: char  

## Usage of `goto`

Because our code may suck, we may want to use `goto`

A line beginning with a colon (`:`) defines a label

`goto` may then be used to jump to that part of the code. Try not to obliterate the stack.

```c
if (yourMom) {
    :yourMom
    
    // shit code

    goto yourMom
}
```

## Full list of reserved words
```c
bool
break
calloc
char
const
continue
else
elseif
enum
false
for
float
free
goto
if
int
lengthof
malloc
nextbyte
outputbyte
realloc
return
sizeof
static
string
struct
true
typeof
void
while
```

### Just keywords
```c
break
const
continue
else
elseif
for
goto
if
return
static
while
```

### Just types
```c
bool
char
enum
float
int
string
struct
void
```

### Just methods
```c
lengthof
nextbyte
outputbyte
sizeof
typeof
malloc
realloc
calloc
free
```

### true/false
```c
true
false
```

# Preprocessor directives
In order to preprocess our directives, the hash symbol followed by one of the following reserved words is used:

```c
#import ./file/path/like
#export MyType1
#export MyStruct, YourStruct, OurStruct
#exportall
```

Filepath structures:
* `.` designates current directory
* `..` designates parent directory
* `/` or `\` designates a subdirectory
* `*` designates everything inside of a directory

When filepath reaches a file and not a directory, we may specificy the import of specific variables.

These may be placed anywhere and however many times wanted.

Top-level variables may not contain the same name if they are imported into the same file. Check for those collisions or I'm telling your mom what you bought this Christmas.