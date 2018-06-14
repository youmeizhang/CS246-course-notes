# CS246 Course Notes
# Shell
shell --> program, os --> interface\
check your own shell: echo $0

# Linux File System
* /  (root of your local system)
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
* - file, d directory
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
In C, array of characters (char\* or char[]), terminated by \\0\
string grows as needed\
safe to manipulate strings

* string operatons
  * == !=
  * < <= > >=
  * s.length(): returns the length of s, include <string>
  * s[0], s[1] ... 
  * concatenation s3 = s1 + s2; s += s2;
 
**String s**: don't need to include library string\
**cin>>s;**: reads a string, skips leading whitespace, stop reading at next whitespace, e.g: read a word\
**cout<<s;**:\
**getline(cin, s);**: reads to the next newline character\
**std::ifstream**: to read data from a file\
**std::ofstream**: to write data to a file\
**ostringsream**: writes to a given string, store there and nothing printed\
**istringstream**: 
You should include <fstream>, <iostream> for stdinp / out / err
 
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
 void printSuiteFile(string name = "suite.txt"){
  print the content of the name;
 }
 
 int main(){
  print SuiteFile();
  print SuiteFile("suite2.txt");
 }
 ```
 
 ## Overloading
 









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
