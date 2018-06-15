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
  * logic other than the simple field init
  
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
Even though it solves the problem, but it is not acceptable in real life because everyone might have their own value of structure. Where do we initialize? If it is in constructor body, then it is too late. Because fields must be fully constructed by the time the constructor body runs\
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
  * you should initialize any fields using MIL
  * fields in MIL are initialized in the order in which they are declared in the class
  * MIL can be move efficient than doing initialization in the constructor body
```C++
struct student{
 string name;
 student(const string & name){
  this --> name = name;
 }
}
```





# Object
```c++
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
1. nullptr\
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
```
