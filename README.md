# CS246 Course Notes
# Shell
shell --> program, os --> interface\
check your own shell: echo $0

# Linux file system
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
**cat blah.txt > rex.txt 2> err.txt**: the error info is in err.txt file, it will override the content in err.txt file\


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
