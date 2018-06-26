# CS246 Course Notes
# Linux File System
* /  root of your local system
  * bin
  * etc
  * home
  * usr (your username)

## some common-used commands:
**pwd**: path of your current directory\
**cd..**: go back to parent path\
**cd \~**: go back to home directory\
absolute path: from root\
relative path: from username

hidden files: we don't want to damage and corrupt them\
**ls -a**: list all the files in the folder including hidden files.\
**mkdir dname**: make director\
**rmdir name**: remove director\
**rm fname**: remove file\
**rmdir -r name**: remove all the files\
**cat fname**: print the content of the file without opening it\



## wildcard matching
**\***: match any sequence of character\
**\?**: match only 1 character\
**ls \*.txt**: show any text file\
**echo word**: show the word itself\
**echo \*.txt**:\
**echo "\*.txt"**: string, just print it\
**[cf]\***: print all the files starts with c or f\
**[]**: print all options\
e.g: cs{246, 1} first and second character cs follwed by more than one chatacter 246 or 1, not only just one parameter\
**cs[!1]\***: starts with cs but not followed by 1\
**cs[!1!2]**: starts with cs but not followed by 1 or 2

create txt: for each line, press enter to end the line
process is connected to 3 streams:\
**stdin**: default: keyboards --> redirect by < (from screen to other places)\
**stdout**: default: screen --> can be redirected by using >\
**stderr**: default: screen --> can be redirected by 2>

**cat -n fname**: number the line\
**cat < fname**: shell open the file for cat and pass it to cat, so it can only open one file\
**cat fname**: cat open the file, so it can open more than one file at one time\
**cat fl.txt > res.txt**: redirect result of fl.txt to res.txt file, similar to copy and paste (cp fl.txt rex.txt)\
**cat blah.txt > rex.txt 2> err.txt**: the error info is in err.txt file, it will override the content in err.txt file

## Pipe
definition: one stdout becomes another file's stdin, such as a | b | c\
**head -4**: print the first 4 lines of a file\
**tail -5**: print the last 5 lines\
**wc fname**: number of line, number of words, number of characters, including new line character\
**wc -l**: only num of line\
**wc -c**: only num of character\
**wc -w**: only num of word\
**head -4 fname | wc -c**: count the characters if the first 4 lines in fname\
**uniq**: eliminate adjacent repeated character, same with new line character\
**sort**: sort it line by line\
**cat words*.txt | sort | uniq**: display all the files starts with words and then sort then and remove the repeated character

## Pattern Matching in Text Files
**egrep**: extended global regular expression print\
**egrep pattern fname**: this is for regular expression, different from globbing pattern and it is case sensitive\
**egrep cs246 index.html**: print all the lines from index.html that contains "cs246"\
**egrep "cs246 | CS246" index.html** equal to **egrep "(cs|CS)246"**

### regular expression option
**"[cC][sS]246"**: all combinations\
**[^......]**: matches any one character that is not in the set\
**[cC]\[sS] ?246**: first char c or C, second char s or S, 0 or 1 char of previous president\
**-rn**: reverse order\
**col?r**: color or colour\
**\.$**: The end "." match\
**\\\.$**: means \. at the end\
**\s**: space\
**\***: matches 0 or occurences of the preceding expression\
**"csa\*246"**: cs and any a then 246\
**+**: indicates 1 or more occurrences of the preceding expression\
**csa+246**: minimum one a\
**.**: matches any single character\
**".+"**: non-empty string\
**".\*"**: any string including empty string\
**^**: matches the beginning of the line, only for the first ^, [^e^]: neither for e nor ^\
**^[A~Z]**: match the first character with capital\
**$**: matches the end of the line\
**egrep "^cs246" index.html**: start with string cs246\
**egrep "^cs246$"**: specific line only with cs246\
**egrep "^(..)\*$" index.html**: print any lines with even number of characters, including empty and new line characters\
**egrep "^[^a]\*a[^a]\*$"**: any number that is not started as a including empty lines contain only one a\
**egrep "^e....$"**: any lines with 5 characters with the first one is e\
**egrep -n**:\
**egrep -v**:\
**egrep -c**:\
**ls -l**: with more details\
**-rw-r_ _ _ _ _ j2smith j2smith 2s seq 9 15:27 fl.txt**:
* \- file, d directory
* a bits
  * rwx: user permission (owner)
  * rwx: group
  * rwx: others
* owner name: j2smith
* group: j2smith
* size: 2s

# Shell Scripts
variables: x = value
* to fetch the value of a variable: use $
* to set the value of a variable: dont use $
* $x == ${x}
* all variables have the type String

**echo $X**: $ to fetch the variables\
**echo $PATH**: print the path\
dir =~/cs246\
**echo $dir**\
**ls $dir**

**How to create a script file?** via vi or vim
**vi fname(no extention name)**: everytime you create you need to get the permission\
**#! /bin/bash**: exit \:wq\ 
**./script**: call, in my current directory to find it, otherwise, it is in PATH, can not find it there\
**chmod u+x fname**: u: user, +: add permission, x: execute\
**whoamI**: print the user name\
**egrep "^$1$"**: /user/...\
**$1**: store the variable in it\
**$2**: store the 2nd parameter in it\
**./isItAWord wow**: allows you to have one parameter (check in this directory)\
**$1, $2, ..etc**: include the arguments passed to the script\
**egrep "^$1$" /... > /dev/null**: they dont want the result printed on screen

for egrep, if found, return 0, not fount, return 1. In unix, 0 means success, 1 means failure

**$?**: status of the most recently executed command
```bash
if [$? -eq 0]; then
 echo ...
else
 echo ...
fi

#a chain if
if [condition] then
elif [condition] then
elif [condition] then
fi ...
```
**${#}**: number of elements, with argument 0, without argument 1\
**$0**: the command line itself\
**ne**: check for not equal\
**>&2**: stderr\
**$?**: the status of last executed command\
**$1, $2**: command arguement
```bash
if [${#} -ne 1]; then ...
 echo "usage: $0 password" >&2 # redirect the result, need to add "" otherwise, it will print in the screen
 exit 1
fi

for word in $( cat, "$2"): do # "$2" pass it as string to cat, since cat consumes string
 if [$word == $1]; then
  x = $((x + 1))
 fi
 
x = 1
while [$x le $1]; do
 echo $x
 x = $((x + 1)) # $(()) for doing arithmetic
done

for name in *.c; do # rename, name.c will no longer exists
 mv ${name} ${name % c}cc # replace the name with cc
done
```
default error message: screen\
**./goodPassWordCheck 2>err.txt**: if the password in null then no output it will store in err.txt\
**cal**: show calender

# C++
difference with c: .c and .cc
```C++
<studio.h>
<iostream> // in terms of input and output
using namespace std; // with this sentence, we can write cout and cin directly, otherwise: std::cout std::cin
int main() // need to return 0 in main
cout<< "..."<<endl;
```

* main must return int in C++
* main by default return 0

**g++ -std=C++14 hello.c -o hello**: the -o program indicates the name that you would like the resulting executable program to have. If not specified, the compiler uses the name a.out

## Input / Output
* 3 I/O stream
  * cout: stdout, no direct, then print in screen
  * cin: stdin
  * cerr: stderr
  
* I/O operations
  * <<: "put to" (scanf), cout<<x; cerr<<x
  * \>>: "get from" (printf) cin>>x; get x from stdin

**>>**: in C, this is bitshift operator\
**cin>>**: ignores whitespace: space, tab, newline\
**cin>>x>>y**: skip whitespace\
**++i/--i**: increment or decreament i by 1 before fetching the value of i\
**i++/i--**: increment or decreament i by 1 after fetching the value of i\
**cin>>i**: if read fails, cin implicitly is set to bool, cin is regarded as True can be used as condition (!cin)\
if the read fails, then (cin.fail) will be true\
if EOF, cin.eof() and cin.fail will be true, but not until the attempted read fail
```c++
int main{
 int i;
 while (True){
  if (!(cin>>i)) break;
  cout<<i<<endl;
}
}

int main(){
 int i;
 while (cin>>i){
  cout<<i<<endl;
 }
}

// reads all ints and echo them until EOF skips non-integer input
int main{
 int i;
 while (True){
  if (!(cin>>i)){
   if(cin.eof()) break;
   cin.clear() // clear the fail bit
   cin.ignore(); // ignore the current input
  }
  else{cout<<i<<endl};
 }
}
```
Website resources: cplusplus.com

## Manipulators
```C++
include <iomanip>
setw // to specify the minimum space for the next numeric or string value
int x = 15;
int y = 124;
cout<<x<<y<<endl; // 15124
cout<<setw(4)<<x<<set(6)<<y<<endl; // _ _15 _ _ _ 124
```
* default is decimal notation
* hex: causes all subsequent integers to be printed in hexadecimal
  * cout<<96<<endl; 95
  * cout<<hex<<95<<end; 5F
  * cout<<dec: switch back to decimal
* dec
* oct

## String in C++
* In C, array of characters (char\* or char[]), terminated by \\0
  * must explicitly manage memory allocate more memory as strins get larget
  * easy to overwrite \\0 and corrupt memory
* In C++
  * string grows as needed, no need to manage memory
  * safe to manipulate string s = "hello"

* string operatons
  * == !=
  * < <= > >=
  * s.length(): returns the length of s, include <string>
  * fetch individual chars: s[0], s[1] ... 
  * concatenation s3 = s1 + s2; s += s2;
 
**String s**: don't need to include library string\
**cin>>s;**: reads a string, skips leading whitespace, stop reading at next whitespace, e.g: read a word\
**cout<<s;**:\
**getline(cin, s);**: if we want to read the whitespace, this reads to the next newline character into s

Anything you can do with cin cout, you can do with an ifstream and ofstream\
**std::ifstream**: to read data from a file\
**std::ofstream**: to write data to a file\
**ostringsream**: writes to a given string, store there and nothing printed\
**istringstream**: 
You should include <fstream>, <iostream> for stdinp / out / err\
An ostringstream is useful (should include sstream) when we need to build up our output a little at a time but do not want to print the output until later. We can, "write" the output to an in-memory ostringstream. Iostream classes handle IO to console (should incude iostream)
 
 ```C++
 int main(){
  ifstream file {"suite.txt"}
  string s;
  while (file>>s){
   cout<<s<<endl;
  }
 } // file is closed when it goes out of scope
 
 istringstream
 int hi = ...;
 int lo = ...;
 ostringstream ss;
 ss<<"enter a number between "<<lo<<" and "<<hi<<""endl;
 string s = ss.str() // ostringstream into str
 cout<<s; // can not be stringstream only int or string, so need to convert it into ss.str()
 // why stringstream? for better manipulation
 
 //same result
 string s = "hi\n";
 cout<<s;
 
 string s = "hi";
 cout<<s<<endl;
 
 //default function parameters
 void printSuiteFile(string name = "suite.txt"){ // default value
  print the content of the name;
 }
 
 int main(){
  print SuiteFile(); // prints from suite.txt
  print SuiteFile("suite2.txt"); // prints from suite2.txt optional parameter must be last
 }
 ```
## Overloading
Even though the number of parameters ot the type of parameters are different, you can still have same function name
```C++
int neg(int a);
bool neg(int a);
// This is not allowed since the int and bool is not enough to distingui the function
// Overloading must differ in number of types of arguments, they can not differ on just the return type

void f(int a, int b = 1){...} // call it as f(5), it is confusing which to call, because b is initialized
void f(int a){...}
```
We can't define function twice but can declare it more than once. You can not use something beforeit has been declared, so declaration before using
```C++
declare int odd(int a) // recursive
int even(int a){
 it calls odd()
 
int odd(int a){
 it calls even()
}
}
```
## Pointers
```C++
int f(int x){
 x = x + 10
}

int main(){
 int y = 10;
 f(y) // in stack, y = 10, x = 20 but out of the scope, x is not accessible anymore
}
```
| variable | stack |
| --- | --- |
| x | 20 |
| y | 10 |
```C++
int n = 5;
int *p = 8n; // initialize p with address of n, 

// two ways of changing the value that ptr pointing at
n = 50
*p = 100

m = 1000
p = 8m // p points to m now
*p = 20 // m becomes 20 now, not 1000 anymore

int a[] = {10, 20, 30, 0}
int *p = a // wrong, a must be int

a[0] = *a
a[1] = *(a+1)

int **pp = 8p; // means pointer to an integer pointer

struct Node{
 int data;
 Node *next; // pointer to type Node,  pointer to something is not completely defined is find
};

// constants: not to change as variable, then declare them as constants
const int n = 5;
      n = ... // not allowed
      
const int m; // wrong, should initialize m
const int n = 100;
const int *p = 8n; // type should match, so n is const, the content of const is not changeable
const int *const p = &n; // can not make p point somewhere else, can not change the data p points to

const int *p = &n; // p is pointer to const int, p can be reassigned but *p can not be assigned  *p = 6 is wrong
int *const p = &n; // p is const pointer to non-const int, cannot change what p points to, can change the data p points to *p = 6

const int m = 20;
const int p = 8m; // correct
const int *p = 30; // wrong 
// right to left, p is a const pointer to an integer, so cannot change what p is pointing to

int * const p = 8n;
p = 8m; // wrong
*p = 100; // correct

const int * const p = 8n
p = 8m; // correct
*p = 100; // wrong

Node n{5, nullptr};
const Node n2 = n1; // n2 is constant but n1 is not, immutable copy of n1

void inc(int n){
 n = n + 1;
}

int main(){
 int x = 10;
 inx(x); // n goes out of scope here
 cout<<x<<endl; // so, x = 10, 10 will be printed not 11
}

// Solution:
void inc(int *n){
 *n = *n + 1;
}

int main(){
 int x = 10;
 inc(&x) // pass the address to the pointers, so not to pass the name of variable
 cout<<x<<endl;// 11 will be printed
}
```
| variable | stack |
| --- | --- |
| m | 1000 |
| p | 8n |
| n | 50 |

## Reference
C++ has another pointers like: reference. The type is reference, but it is similar to pointer
```C++
int y = 10;
int &z = y; // just write the name of the variable, name to pass not the address
// z is like const pointers, can not change what it points to and y is not constant, but z is

z = 100; // change y value, instead of *z = 100
// so now, z is like a alias to y, a is lvalue reference

&z = y; // when initializing, y can be only lvalue
int *p = &z; // taking the address of z gives the address of y, so z behaves exactly like y

int &x = 3; // wrong
int &x = y + z; // wrong, expression is not lvalue
int &*p; // wrong, pointer to a reference is not fine
int *&z = ... // correct, reference to a pointer is fine
int &&r; // correct, but not recommend for using

int &r[3] = {n, n, n} // wrong

void inc(int &n){
 n = n + 1;
}

int main(){
 int x = 10;
 inc(x) // initialize &n = x
 cout<<x<<endl; // print 11
}

int f(int &n){...} // &n is const
int g(const int &n){...} // int is const

f(5) // X
g(5) // in c++, V

int f(int &n){...}
int g(const int &n){...} // but const int *const x: not correct
f(5) // X, can not initialzed lvalue ref n to a literal value, if n changes, cannot change the literal 5
g(5) // V, since n never change. the compiler create a temporary location in memory to hold the 5, so the reference n has something to point at 


```
* lvalue: left-hand value, y = 5, y is lvalue, the value which you can assign something to it
* rvalue: right-hand, 5, which you can not assign something to it
* Things you can not do with lvalue references
  * leave them uninitialized: must initialize them with sth that has an address
  * create a pointer to a reference: int &\*x is not OK
  * a reference to a pointer is OK: int \*&x
  * create a reference to a reference: int &&r
  * create an array of references int &r[3] = {n, n, n} not OK
  
* Pass as function parameters. Prefer pass-by-const-ref over pass-by-value for anything larger than an int, unless the function needs to make a copy anyway. Then possibly use pass-by-value.

## Dynamic Memory Allocation
In C++, malloc / free are available, but do not use them, use new / delete instead
* all local variables resides on the stacj
* allocated memory resides on the heap
```C++
struct Node{
 int data;
 Node *next;
};

Node *np = new Node; // np is stored in stack, they are not free until you freeze them, different from stack
delete np; // np still have address, but not accessible

Node(n);
np = &n; // then np points to n

Node *myNode = new Node[10]; // array of 10 nodes in the heap, myNode is on the stack, point to the beginning of the array
delete [] myNode; // remember the []
```
* if memory is allocated with ordinary new, it must be deallocated with ordinary delete
* if memory is allocated with array new, it must be deallocated with array delete []

## returning by value / pointer / reference
```C++
Node getMeNode(){
 Node n;
 ...
 return; // returning a node by value, if node is large, then it is very expensive, so return a reference
}

Node *getMeNode(){
 Node n; // definition in the function, but out of it, it is not accessible, so bad solution
 ...
 return &n;
}

Node *getMeNode(){
 Node *np = new node; // allocated on the heap
 ...
 return np; // the address of the node on the heap
}
//need another function to free the memory later on
```

## Operator Overloading
```C++
struct vec{
 int x, y;
};
```
vec v1{1, 2};\
vec v2{3, 4};\
vec v3 = v1 + v2;\
vec v4 = 3 + v3;\
vec v5 = v3 * 10;

```C++
operator+(const vec &v1, const vec &v2){ // overload, type is vec, original function name for "+" operation
 vec res{v1 x + v2 x, v1 y + v2 y};
 return res;
}

struct Grade(){
 int theGrade;
}

cout<<x<<y<<z<<endl; // cout stream

ostream &operator<<(ostream &out, const Grade &g){
 cout<<g theGrade<<% // add the %
}
istream &operator>>(istream &in, Grade &g) // Grade is not const we need to put something into it
```
## Processor
1. **#include \<iostream>**: copy into your program, insert the content of file iostream here. <> means look in standard include library (usr/include/c++/...)
2. **#include "filename.ext"**: the file you have, it is in the same directory. "" means look in the current directory
3. #include <\/../../../library>: you can not do this by importing your own file name

\#define VAR VALUE\
Set a preprocessor variable. Then all occurrences of VAR in the source file are replaced with VALUE

C++: const VAR = VALUE
```C++
// gcc14 program.cc
// gcc14 -D debug1 Program.cc
#if O(false)
  skip
#endif
#ifdef debug1 // It means if debug1 is defined, then True
 print... // execute without this section: program.cc
#endif

#define FLAG // set the variable FLAG, its value is the empty string
#if 0 // This is never true, so all of the inner text is removed before the compiler sees it
...
#endif
```
## Seperate Compilation
split program into modules, each module provide:
* interface (.h)
  * type definitions and prototypes for functions
* implementation (.c)
  * full details, function definitions allocating spaces
```C++
//vector.h
struct vec{
 int x;
 int y;
}

//vector.cc
#include "vector.h"
vec operator+(...){
 vec V;
}
extern int globalNum; // declaration here, when main imports vector then it is defined more than once, fix it: add extern 

//main.cc (cliend)
#include "vector.h"{
vec v1 = {1, 2};
v = v + v;
globalNum = ...; // use it here
}

//linearAlg.h
#include "vector.h"
...

//linearAlg.cc
#include "vector.h"
#include "linearAlg.h"
..

//main.cc
#include "vector.h"
#include "linearAlg.h" // it contains the vector.h again, so it is defined more than once

// Solution: for every .h file include a guard for any interface

#ifndef -VECH-
#define -VECH-
...
//the content of .h file here

#endif
```
g++14 -c vector.cc\
g++14 -c main.cc\
for these two, they do not link together, produce .O object file do not create execute code\
vector.o, main.o\
**g++14 vector.o main.o -o main**: link all objects files together and create an executable main, otherwise, default is a.out
* Never ever compile .h file
* Never ever include .c or .cc file

What if we want a module .h to provide a global variable?\
put the variable in the .cc file\
for const: add const in .h and .cc and assign value in .cc (only use in main)

* Always include guards in .h files
* Never put using namespace std in .h files. The using directive will be forced upon any client who includes the file. Always refer to cin, cout, endl, string, etc. as std::cin, std::cout, etc. in header files.
* Must put all header files in \#include guards

# Class
The big innovation in OOP, can put functions inside structures. Class means a structure that can potentially contain functions. The function inside the class is also called method or member function. An object is an instance of a class. 
```C++
struct Student{ // class
 int assns, mt, final;
 float grade(){ // this is method member function
  return assns * 0.4 + mt * 0.2 + final * 0.4;
 }
}

int main(){
 Student s{70, 80, 90};
 Student s2{20, 10, 30};
 cout<<s.grade()<<endl;
 cout<<s2.grade()<<endl; 
}
```
Formally, methods take a hidden extra parameter called **this** which is a parameter to the object upon which the method was invoked\
```C++
// three ways to represent
int x = 5;
string s = "hello";

int x(5);
string s("hello");

int x{5}; // this is preferred
string s{"hello"};

Student(int assns, int mt, int final){ // overloading the old one, constructor function
 this --> assns = assns; // first one is field, second one is value, it means parameters
 this --> mt = mt;
 this --> final = final;
}

Student *pJame = new Student{99, 99, 99}; // pJame is in stack, {99, 99, 99} is in heap
```
## Constructor
* Advantage of creating constructors
  * default parameters
  * overloading
  * sanity check
  * logic other than the simple field initialization
  
```C++
student(int assns = 0, int mt = 0, int final = 0){
 this --> assns = assns;
 ...
}

student Mary{100, 90, 98};
student Dave; // default: 0, 0, 0
student Jane{100, 99}; // the last parameter is 0

struct MyStruct{
 const int myConst; // reference const has to be initialized, so this is a problem
 int &myRef;
}

// Solution
int z = 0;
stuct MyStruct{
 const int myConst = 5;
 int &myRef = z;
}
```
Even though it solves the problem, but it is not acceptable in real life because everyone might have their own value of structure. Where do we initialize? If it is in constructor body, then it is too late. Because fields must be fully constructed by the time the constructor body runs
* What happens when an object is created?
  * space is allocated
  * fields are constructed 
  * constructor body runs
  
So, it should be initialized in the second step when fields are constructed. Solution: **MIL** Member Initialization List
```C++
struct student{
 const int id;
 int assns, int mt, int final; // fields
 student(int id, int assns, int final):id{id}, assns{assns}, mt{mt}, final{final} // id: field, {id}: parameter
 {} // if you dont initialize in last line, then should be initialized here
}
```
* Notes
  * you should initialize **any** fields using MIL, not just const and refs
  * fields in MIL are initialized in the order in which they are declared in the class even if the MIL orders them differently
  * MIL can be move efficient than doing initialization in the constructor body. For fields that are objects, if they are not listed in the MIL, they are default-constructed. If you initialize them in the body, they are initialized twice which is wasteful
```C++
struct student{
 string name;
 student(const string & name){
  this --> name = name;
 }
}

struct vec{
 int x = 0, y = 0;
 vec(int x):x{x}
 {}
}

vec v1{5}; // the init in MIL takes precedence over the in-class init

student billy{60, 70, 80};
student bobby = billy // copy constructor
student(const student &other){
assns{other.assns}, mt{other.mt}, final{other.final} // other refers to billy, passing to a reference, we pass the actual value not address
{}
}

Struct Node{
 int data;
 Node *next;
 Node(int data, Node *next): data{data}, next{next} // here pass the parameter. constructors
 {}
}

Node *n = new Node{1, new Node{2, new Node{3, nullptr}}};
Node m = *n; // the value m is pointing to the structure, this is copy constructors, a shallow copy
Node *p = new Node{*n}; // Node{*n} is the value that n is pointing at
```

| variable  | content |
| ------------- | ------------- |
| p  | --> 1  |
| m  |  --> 2 --> 3 |
| n  | 1 --> 2 --> 3  |

```C++
struct Node{
 Node (const Node &other): data{data}, next{other.next? new Node{*other.next}:nullptr} // deep copy
 {}
}
```
* By default, every class comes with
  * a default constructor (default-constructs all fields that are objects, lost if you define any constructor, default constructor should be able to consumes zero argument)
  * a copy constructor (just copy all fields)
  * a copy assignment operator
  * a destructor
  * a move constructor
  * a move assignment operator
* The copy constructor is called
  * when an object is initialized by another object int x = y
  * when an object is passed by value
  * when an object is returned by a function
Be careful with constructors that can take one parameter
```C++
struct Node{
 ...
 explicit Node(int data): data{data}, next{nullptr}{}
}

Node n(4); // ok
Node n = 4; // n is a Node, but 4 is not a Node, 4 can be regarded as an object
// if int f(Node n){...}, f(4), then it is not valid anymore, so add explicit in the above code
// for only passing the node value

Node n; // default constructor
Node m = n; // copy constructor two step: 1. need to copy constructor in stack 2. copy value of n to m

Struct Node{
 int data;
 Node *next;
 Node(int data = 0, Node *next = nullptr)
 {}
 // or Node(): data{0}, next{nullptr}{} // other overloading constructors
}

Node(const Node &other){ // copy constructor
 data{other.data};
 next{other.next ? new Node{other.next} : nullptr} // next{other.next} shallow copy constructors
 {} // n: 1 --> 2 --> 3 m: 1
}

Node *np --> 1 --> 2 --> 3 (on heap) // chain of node
delete np; // it will free 1 but not 2 and 3
```
## Destructor
* object is destroyed
  * stored in stack, after out of scope, destructor is invoked
  * stored in heap, when delete it in heap, destructor is invoked
* a method destructor runs
  * destrcutor body runs
  * destructor is invoked for fields that are objects in reverse declaration order
  * space is deallocated
```C++
~ Node(){delete next}; // this is defined inside the structure, the last node destructed first, then the second to last

//these are defined, so need to free them
Node n{5, nullptr}; // constructor is invoked
Node m = n; // copy constructor, creating memory in stack and then copy value
//need to free as well
Node w; // default constructor
w = n; // no creating memory, copy assignment operator, just assignment

Node w{5, nullptr};
w = n; // in linkedlist, shallow copy only the first Node
```
## Copy Assignment Operator
Classes come with a copy assignment operator that copy-assigns each field (a shallow copy). Thus you may need to write you own. When writing operator=, always be wary of self-assignment
```C++
Node &operator:(const Node &other){ // self assignment
 if (this == &other): return *this;
 data = other.data; // without this it is the same as copy constructors
 delete next; // destroy all the linked list
 next = other.next ? new Node(*other.next) : nullptr;
 return *this;
 // lose old data
}
```
| variable  | content |
| ------------- | ------------- |
| n  | 1 --> 2 --> 3  |
| w new |  1 --> 2 --> 3 |
| w old  | 1 --> 5 --> 6  |

w points to new list but 5 and 6 still in the heap, this is a problem\
Solution: a pointer points to old data, after successfully run, delete the old pointer
```C++
Node &operator = (const Node &other){
 if (this == &other) return *this;
 Node *tmp = next;
 next = other.next ? new Node(*other.next) : nullptr;
 data = other.data;
 delete tmp;
 return *this;
}

#include <utility>
struct Node{
 ...
 void swap(Node &other){
  using std::swap
  swap(data, other.data)
  swap(next, other.next)
 }
 
 void &operator = (){
  Node tmp = other; // deep copy, copy constructor
  swap(tmp); // swap between tmp & this
  return *this; // new data of w
 }
}
```
Differences between copy constructors and copy assignment operators\
n = n; // no, because it delete all the data in the linked list if delete one
## Rvalues and rvalue refernces
const int x= = 5; // x is the const lvalue\
rvalue can not be put in the left side of the assignment operator, so can not define a reference to rvalue
```C++
Node plusOne(Node n){
 for (Node *p = &n; p; p = p-->next){
  ++p --> data;
 }
 return n;
}

Node n{1, new Node{2, nullptr}};
Node m3 = plusOne(n); // plusOne(n) is rvalue, so this is not copy constructor, because for ctor, it should be lvalue
// this is a move constructor
```
* move constructor takes rvalue
* copy constructor takes lvalue
```C++
Node (Node &&other){ // move constructor, &&other: define a reference to rvalue
 data{other.data}; // the result of the rvalue will be assigned in memory, tmp position anywhere, then other is pointing to that address, so stealing it
 next{other.next}; // to actual stack for storage, so copy from temporary result to m3, m3 pointing to the same list
 other.next = nullptr; // disconnect result and other, so m3 can continue to point to that list even though other is destructed, without this, when it is out of scope, the list would be removed and m3 has nothing to point to, so need to disconnect it
}

Node &operator = (Node &&other){ // rvalue reference, destroyed by destructor
 using std::swap;
 swap(data, other.data);
 swap(next, other.next);
 return *this; // cascading same as y = x = z = w = 0
 
}
```
* if move exists with rvalue, then move would be invoked
```C++
struct vec{
 ...
}

vec makeAVec(){
 return{0, 0}; // invoke constructor vector
}

int main(){
 vec v = makeAVec(); // move constructor, if dont define move then copy constructor will be invoked. here initialize with v = {0, 0}
}

void doSomething(vec){ // pass by value
 
}

doSomething(makeAVec); // what is passing? m

struct vec{
 int x, y;
 vec operator+(const vec &other){ // 1 parameter method + this = 2 parameters, because it is defined inside the structure, so it has this parameter
  return {x + other.x, y + other.y};
 }
 
 vec operator*(const int k){
  return {x * k, y * k};
 }
}
// outside the structure, standalone
vec operator*(const int k, const vec &v){
 return v * k; // not self calling
}

v1 + v2; // 2 arguements
3 + v2; // 3 is not a vector, so not invoked

3 * v2; // wrong
v2 * 3; // correct
```
v<<cout; if methods, then v must be object vector\
v>>cin; it is better to define them as standalone
* some operators must be members
  * operator =
  * operator []
  * operator -->
  * operator ()
  * operator T
```C++
//Node.h
#ifndef _Node.H_
#define _Node.H_
struct Node{
 int data;
 Node *next;
 explicit Node(int data, Node *next = nullptr); // be careful only one argument in a method, in this structure, no other methods defined, only accept integer as input, add explicit
 Node(const Node &n);
}

//Node.c
#include "Node.h"
Node::Node(...){}
Node::Node(...){}
...
// :: scope resolution operator, for methods not for standalone

struct vec{
 int x, y;
 vec(int x, int y): x{x}, y{y}{}
 vec(): x{0}, y{0}{} //
}

// make sure to have this vector with only one parameters
vec *vp = new vec[10]; 
vec moreVectors[15];

// solution 1
vec moreVectors[3] = {vec{0, 10}, vec{-1, 0}, vec{5, -3}};

//solution 2
vec **vp = new vec * [5];
vp[0] = new vec{0, 0};
vp[1] = new vec{-1, 10};

for (int i=0; i<5; ++i){
 delete vp[i];
}

struct Student(){
  int assns, mt, final;
  mutable int numMethodCalls = 0; // for counting how many times this mehod grade is called, only this field is mutable now
  float grade() const{ // it is a constant object
    ++numMethodCalls // we can not do this since it is consstant if no "mutable"
    return assns * 0.9 + ..;
}    
  Student Mary(){...}   
}
```
how to track how many times Mary() call the grade method?
```c++
static int numInstances; // initialize it outside this code, should be in implement file\
float ...
Student ()
  ++numInstances; // count how many student objects are created
```
for static: it means it belongs to a class, not specific object
```c++
static void printInstance(){ //static method can access static field
cout<<numInstances<<endl; // or cout<<printInstance()<<endl;
}
```
* **notes**
  * mutable fields can be changed even if the object is constant
  * static field is associated with a class and not with a particular object (instance of class)
  * static field should be initialized outside the struct definition
  * static method functions doesn't have implicit this and can only access static fields, so don't define struct as static. static function can only call other static functions

int Student: Instances=0; // method can call other methods
```c++
int main() {
  Node n1 {1, newNode} {2, nullptr}; 
  Node n2 {3, nullptr};
  Node n3 {4, &n2};
  ...
}
```
for destructor, when we call delete next, next should be one of the following type so destructor can work successfully: 
1. nullptr
2. allocated in heap

```c++
Struct Node{
  private:
  int data
  Node *next;
  
  public:
  Node(int data, Node *next):data{data}, next{next}{}
  ~Node(){delete next;}
}
```
default visibility should be private and make something to be public if I want
```c++
class Node{ // with class, the default visibility is private
  ... // this is private
  public: // this is public
  private: // should declare it as private again
}
```

## Encapsulation
```c++
auto x = y; // the type of x is the same as y
```
C++'s built in support the iterator pattern: the range based loop
``` C++
//include iterator in class list, including begining, end, advancing
for (auto &n: lst){ // n is the copy of any item in the list. For will go to the beginning and go through the list
 cout<<n<<endl; // if you want to change the value in lst, then change it to &n, otherwise you can not change lst, because n is just a copy
}

auto it = list::Iterator(nullptr);// conflict, make it private

class list{
 ...
public
class Iterator{
  Node *p
  explicit Iterator ...; // Iterator is private, users can not access it, but list needs access to it
 public
  friend class list; // solution: create friendship between different classes, so list has all the access to iterator now
  ... // gold rule: don't make so many friends, otherwise conflict privacy then
 }
}
```
```C++
class Vec{
 int x, y;
 
 vec...
public:
 int getX(){
 return x; // ancessor to the field, no need to have "this", since this is a method, so return a copy from x
 }
 void setY(int newY){ // mutator
  y = newY;
 }
}

int main(){
 Vec v1(1, 10);
 cout<<v1.getX()<<endl;
 
friend header operator; // if we want to override <<: use standalone function and define it outside the struct definition
//only declare it as friend here
}

//actual definition of operator

class Vec{
 int x, y;
 public
 friend ostream &operator... //have access to the class, declare it inside the function
}  //either provide ancessor or make it a friend to have access into that class

ostream &operator<<(ostream &out, const Vec&v){
return out<<v.x<<" "<<v.y;
}
```
## UML

| name  | Vec |
| ------------- | ------------- |
| fields (optional) | - x: int, - y: int |
| methods (optional)  | + Vec() + getX(): int + getY(): int|

-: means private\
+: means public

## Composition
This kind of relationship is called **OWN**: Basis "own-a" 2 Vec objects
```C++
class Vec{
 int x, y;
 public
 Vec(int x, int y): x{x}, y{y}{}
}

class Basis{
 Vec v1, v2
}

Basis b; // wrong, invoked default constructor: three steps will be invoked. fields are constructed
//but the fields are vec, default constructor for vec can not be invokded because it will overload. so generate errors 

class Basis{
 Vec v1, v2
 public
 Basis(): v1{0, 0}, v2{0, 0}{} // solutions here: MIL, so Vec default constructor will be invoked
}
```
* A "owns-a" B
  * B has no identity beyond A (B does not exist if A does not exist)
  * if A is destroyed then B is destroyed
  * if A is copied then B is copied as well

## Oggregation
**"has-a"**: weaker relationship
* A "has-a" B
  * B has an existance outside of A
  * if A is destroyed, B lives on
  * if A is copied, B is not
  * B does not know about A
  * B can belong to more than one object at a time
```C++
clss Teacher{
 string name;
 public:
 Teacher(string name): name{name}{}
 string getName(){
  return name;
 }
}

class Department{
 Teacher * m_teacher[100];
 int t_Num;
 public:
 Department(): t_Num{0}{}
 void add_teacher(teacher *teacher){ // shallow destructor because i dont have a defined destructor
  m_teacher[t_Num]: teacher;
  ++t_Num;
 }
};

int main(){
 Teacher *teacher = new Teacher("Bob"); // defined before department, prove that B does not know A
 Teacher *teacher = new Teacher("Mary");

 {
 Department dept;
 dept.add_teacher(teacher1);
 dept.add_teacher(teacher2);
 }
 
 cout<<teacher1-->getName()<<endl;
 delete teacher1;
 delete teacher2;

};
```
Another example
```C++
class Book{ // superclass, Base class
 string title, author;
 int numPage;
 public:
 Book(...); // constructor for Book
}

class Text{
 string title, author;
 int numPage;
 string topic;
 public:
 Text(...);
}

class Comic{
 string title, author;
 int numPage;
 string hero;
 public:
 Comic(...);

};
```
* How to have one array with different types of element (OK solutions)
  * void
  * create a new type to be Book, Text, Comic
  
* subclasses inherit fields and methods from superclass
* every subclass only has access to the public fields and public methods from the super class

Two problems
* When the superclass part is constructed, the compiler will call the default constructor of superclass, but there is no constructor in the superclass Book. (Solution: MIL to construct the superclass)
* The fields of Book are private, subclass does not have access to that. 
```C++
class Book{ // superclass, Base class
 string title, author;
 int numPage;
 public:
 Book(...); // constructor for Book
}

class Text: public Book{ // subclass, Derived class
 string topic;
 public:
 Text(string title, author, numPage, topic):
 Book{title, author, numPage}, topic{topic} // step 2 and step 3
 {} // step 4
}

class Comic: public Book{
 string hero;
 public:
 Comic(...);

};
```
* When an subclass object is constructed
  * memory is allocated
  * the superclass part is constructed (call the default constructor of superclass)
  * the subclass's fields are constructed 
  * the subclass's constructor body runs

* If the superclass has no default constructor, then the subclass **must** invoke a non-default superclass constructor in MIL

* To access superclass fields:
  * make the fields public
  * make subclass as a friend
  * define ancestor
  * protected the fields, only subclass can access it (can also protect methods)
```C++
class Book {
 protected:
  String title, author;
  int numPages;
 public:
  Book(...);
}
```
**is-a relationship**
publiv inheritance\
A Text "is-a" Book\
A Comic "is-a" Book
    Book
     |
   _____
  |     |
Text   Comic

```C++
\\ it is ok for subclass to have same name for their methods
class Book {
public:
bool isItHeavy() const{return numPages > 200;}
};

class Text: public Book {
public:
bool isItHeavy() const{return numPages > 50;}
};

class Comic: public Book {
public:
bool isItHeavy() const{return numPages > 30;}
};

Book b{" ", " ", 50};
Comic c{" ", " ", 40};
b.isItHeavy() ==> False
c.isItHeavy() ==> True

Book b = Comic{" ", " ", 40, " "}; // the Comic value is sliced or pressed into a Book value, but it will lose some information
b.isItHeavy() ==> False // b is now a Book object
```
When accessing object through pointers, slicing does **NOT** happen\
```C++
Comic c {" ", " ", 40, " "};
Book *pb = &c; // Book object, does not press into Comic object
Comic *pc = &c;

pb --> isItHeavy() ==> False
pc --> isItHeavy() ==> True
```
The object behaves differently depending on the type of the pointer that points at it. But this does not solve the problem that if a pointer points to both. In following example, we want c to be treated as both Comic. 
```C++
Solution:
class Book{
 bool isItHeavy() const{...}
};

class Comic: public Book{
 bool isItHeavy() const override{...}
};

Book * myBooks[10];
...
for(int i = 0; i<10; i++){
 cout<<myBooks[i]-->isItHeavy()<<endl; // then it will output based on the type of the object not based on Book 
}
```
**What we do to pointers here is also True to reference**
```C++
Comic c{" ", " ", 40, " "};
Book *pB {&c};
Book &rB{c}; // reference, no need to do dref, so rB.isItHeavy() is fine
Book *pC{&c};

pB --> isItHeavy() ==> True
Pc --> isItHeavy() ==> True
rB.isItHeavy() ==> True
```
The code accommodates multiple types under one abstraction is called **polymorphism**
f(istream &in)

```C++
class X {
 int *x;
public:
 X(int n): x{new int [n]} {}
 ~X() {delete [] x;}
};
class Y: public X {
 int *y;
 public:
 Y(int m, int n): X{n}, y{new. int [m]} {} // the constructor of Y is correct, it will call X constructor automatically
 ~Y() {delete [] y;}
};

X *myX = new Y{10, 20};
delete myX; // X destructor will be invoked, get memory leak. Solution: desctructor of superclass should be virtual destructor (always do this)

class Y final: public X {
// Y is not superclass to any other subclasses, so always add final to this kind of classes
}
```

```C++
class One {
 int x, y;
 public:
 One(int x = 0, int y = 0): x{x}, y{y} {}
};

class Two: public One { 
 int z;
 public:
 Two (int x = 0, int y = 0, int z = 0): One{x, y}, z{z} {}
};

void f(One *a) {
 a[0] = One{6, 7};
 a[1] = One{8, 9};
}

Two myArrays[2] {{1,2,3}, {4,5,6}};
f(myArray);
```
Details are misaligned: myArray{{6, 7, 8}, {9, 5, 6}}\
Never us arrays of objects polymorphically\
Solution: use array of pointers to objects instead

```C++
class Student {
 protected:
  int numCourses;
 public:
 virtual int fees() const; // make it virtual
 ...
};

class Regular: public Student {
public:
 int fees() const override;
 student fees
};

class coOp(): public Student {
 public:
 int fees() const override;
 fees
}
```
Only allow student to be regular or coop, but can not create an object of type student\
Solution: abstract class
```C++
class Student {
 ...
public:
 virtual int fees() const = 0; // pure virtual method. not implementing this method, so make the student to be abstract class
}
```
A class with a pure virtual method is an absctract class\
An abstract class can **NOT** be instantiated\
Student s; --> error\
Its purpose is to organize the subclasses\
Subclasses to abstract class are also abstract unless they implement the pure virtual methods\
UML: all abstract classes' names should be italic
```C++
class Book {
 public:
 //defines copy / move constructors, and copy / move assign
}

class Text: public Book {
 string topics;
 public: // but compiler has default construtor for us
 // does not define copy / move operations
};

Text t {"Algorithms", "CLRS", 500, "CS"};
Text t2 = t; // no copy constructor in Text, what happens?
```
Since Text inheritates from Book, so the copy constructor from Book would be invoked.But we dont want this, because the fields from Book would be copied but other fields would be ignores. Solution: define the copy constructor and other constructor in Text as well
```C++
// define Book constructor first and initialized from other, t2 is other which means copy other to this
Text::Text(Const Text &other): Book{other}, topic{other.topic} {} 
// copy constructor for Text, the number of fields of other is larger than number of fields in Book, so slicing! So only the fields of Book would be copied

Text &Text::operator = (const Text &other){ // copy assignment operator
 Book::operator=(other); // copy all the operators in other to this
 topic = other.topic;
 return *this;
}

Text::Text(Text &&other): Book{std::move(other)}, topic{std::move(other.topic)} {} // move constructor

Text &Text::operator = (Text &&other){
 Book::operator = (std::move(other)); //
 topic = std::move(other.topic);
 return *this;
}

//in the move operator, the type is rvalue reference for Book

Text t1{...}, t2{...};
Book *pb1 = &t1, *pb2 = &t2;

*pb1 = *pb2; // copy assignment operator of Book, compiler looks at the pointer, the pointer is Book, so Book::operator= runs
// The result is a partial assignment, ignore topic, Solution: make it virtual in superclass

Text t{...}
Book b{...}
Text *pt = &t; //  
Book *pb = &b; //  
*pt = *pb; // copy assignment operator of Text, but only Book part is copied
// Compiler also allows assigning Text object to Comic, but we dont want this
```































