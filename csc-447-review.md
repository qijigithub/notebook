---
description: 'the language included are scheme, c, java, scala, javascript'
---

# CSC 447 review

## Lecture 1

### class notes

#### overview

* machines and programs
  * machines=input simulation+output responce
  * program:perticular responce and store in memory
    * computing=control and memory
    * fixed program before computuation begins,and store in the memory
  * time
    * before program: compile time
    * after program:run tiime
  * finer grained
    * compiler: compile,link, load
    * memory: allocation, initialization, deallocation
    * function: call, return
  * memory
    * copying and sharing
    * allcation and deallocation
    * categories
      * globe
      * local
      * unmamanaged
      * ARC and garbage collection
* interpretation and translation
  * machine language to program language
  * transforamed into equivalent program: c\# to visual machine language
* designing a language
  * resource constraints
  * programming model
  * error
  * module

#### c:statements versus expression, strict versus non strict and undefined behavior

* statement and expression
  * statements:executed for side-effects, eg: literal,operators,function call
  * expression:executed for their value\(for side effect\),eg:loop ifelse, return and expression statements\(include assignment\)
  * PS: side-effect eg: x++, x plus 1 and change x value
    * side-effecting expression:x++
* comma operator \(e1,e2,e3\)
* sequencing:

```text
1
int x=5;
x*=2;
print (x)

2
int x=5;
print("%d\n",(x*=2,x))
the semantics is not same,but result is same
```

* strict and non strict
  * strict: if it evaluate all of its operands before it runst
    * eg: operators:+,-,\*,/ \| comparson:&lt;,&lt;=,==, != \| bitwise:\|,&&lt;,!,^\|function call\| etc

```c
int fcond (int b, int t, int f) { return b ? t : f; }//function
int main () {
  for (int i=0; i<10; i++) {
    int x = 0;
    int y = 0;
    int z = fcond (rand()%2,  (x=1,111), (y=2,222));
    printf ("x=%d, y=%d, z=%d\n", x, y, z);
  }
}

outout:
x=1, y=2, z=111
x=1, y=2, z=222
```

* non strict:evaluate part of its operands
  * eg &&,\|\|,e1?e2:e3 \(ternary operator\)\| macro expansion 

```c
define mcond(b, t, f) (b)?(t):(f) //macro expansion
int main () {
  for (int i=0; i<10; i++) {
    int x = 0;
    int y = 0;
    int z = mcond (rand()%2,  (x=1,111), (y=2,222));
    printf ("x=%d, y=%d, z=%d\n", x, y, z);
  }
}
output:
x=1, y=0, z=111
x=0, y=2, z=222
```

Because function is strict, before execute function mcond, x=1, and y=2, z=111or 222. Macro call is not strict, so that the function mcond execute first when meeting b go back find the parameter b, random/2!= 0 true, and meeting t, t=1,111 and ignore y=2, 222. macro calls are evaluated in the compiler, by textual substitution.

* conditional  statment and conditional expression
  * conditional statement

```text
int fact(int n){
    if(n<1){
        return 1;
    }
    else{
       return n*fact(n-1); 
    }
}
```

* conditional expression

```text
int fact(int n){
    return (n<1)?1:n*fact(n-1);
}
```

Because  conditional expression use expression and didn't use statement.

* undefined behavior
  * under/overflow

```text
#include <stdio.h>

int isMinValue (int x) {
  return (x-1) > x;
}
int main () {
  int i = -2000000000;  //c no boolean type  integer 0 or 1
  while (!isMinValue(i))
    i--;
  printf ("Min value is %d\n", i);
}
 output1:
 $ gcc -O1 undefined.c && ./a.out 
Min value is -2147483648
 
```

```text
$ gcc -O2 undefined.c && ./a.out 
^C #infinite loop
```

O1 explanation: when execute isMinValue\(x\): i-1, and compare with x, if x-1 &gt;x  return 1, else return 0, -2000000000-1-1-1-1...,x-1 always less than x, return false. until i=-2,147,483,648, which minus 1 is underflow, the result is 2147483647. the function return value is true, and print -2,147,483,648 is min value.

O2 explanation: the result is infinit loop, because the underflow in c is undefined behavior, what the compiler do is x-1&gt;x, is always true,so it is infinit loop.

* order of operations

```text
#include <stdio.h>
int count = 0;
int f () {
  count += 1;
  return count;
}
int main () {
  int z = f() + f();
  printf ("%d\n", z);
  z = (z += 1) + (z = z*z);
  printf ("%d\n", z);
}
 output：
$ clang -Wall undefined3.c 
undefined3.c:11:21: warning: unsequenced modification and access to 'z'
  z = (z += 1) + (z = z*z);
         ~~         ^
1 warning generated.
$ ./a.out 
3
20
```

it runs in clang, the compiler gives a warning because it is not run in the right order

* dangling pointers

```c
#include <stdio.h>
#include <stdlib.h>

int* foo (int n) {
  int result = 2 * n;
  return &result;
}

int main () {
  int* p = foo (5);
  int* q = foo (7);
  printf ("%x %d\n", p, *p);
  printf ("%x %d\n", q, *q);  
}
output: 
in clang: 
14 and big number
in gcc:
segment error
```

p and q in main function point into an activation record, which is stack, and when the function executing finished, the memory of that function will be deallocated.

#### scheme

* literal
  * number:5
  * String:"hello world"
  * symbol:'hello world'
* arithmetic 
  * using prefix notation: \(\* \(+ 1 2\) 3\)
* function
  * define: \(define \(square n\) \(\* n n\)\)
    * \(define \(f p1 p2 p3...\) \(e1 e2...\)\)
  * invoke: \(square 5 2\)
* boolean and condiaitonals
  * \#t or \#f
  * if is not strict
* recursive function
* con cells

  * pair of numbers:\(cons 1 2\)\| strings:\(cons "hello" "world"\)\| a number and  a string: \(cons 1 "world"\)
  * car: head \| cdr: tail
  * for linked list

    * empty list: \(\)
    * singleton list: \(cons 3 \(\)\)
    * regular list:\(cons 1 \(cons 2 \(cons 3 \(\)\)\)\)

  * list
    * list only has one structure type: the pair
    * a non-empty list is just a special type of pair
    * pairs are a kind of symbolic-expression

* syntax sugar for lists

  * quote: special form prevent evaluation: \(quote \(1 2 3\)\)
  * ' :shorthand for quote '\(1 2 3\)
  * list function evaluates args, put results in a list: \(list 2 3 \(+1 2\)\)

* equals problem
  * eq? compares two pointers are same, same to java ==
  * equal? compares two structure are same to java equals
* read-eval-print loop
  * quote:delay evaluation
  * eval: evaluates an expression
  * read function reads the expression

### worksheet

### homework

### quiz

## Lecture2

### class notes

* types：used to describe when operations allowed

  * "+" in java: string, numeric types
  * "-" in java: numeric types

* dynamic type checking and  static type checking
* **dynamic type checking**: tracks and stores type of data at runtime\(before applying an operation\).
  * how do we know this is dynamic? answer: no type checking before execution.
    * when writing an expression in function, invoke function there is no error message, but fails when call function.

```scheme
(define (f) (- 5 "hello"))  no error at here
(f)
output:
Error in -: expected type number, got '"hello"'.
```

* * Language use:
* **static type checking**:compiler analyzes code for type error
  * how do we know this is static; answer: javac and get the error message.

```java
class Typing01 {
  void f () {
    int a = 5;
    String b = "hello";
    System.out.println ("Result = " + (a - b));
  }
}

output:
$ javac Typing01.java
Typing01.java:5: error: bad operand types for binary operator '-'
    System.out.println ("Result = " + (a - b));
                                         ^
  first type:  int
  second type: String
```

* Language : C, Java, Scala, Haskell, Rust, Coq, etc

```c
int main () {
  char a[] = { 0x85, 0x86, 0x87, 0x88 };
  int b = a[0];
  char *p = &a[0];
  int *q = p;       /* type checker complains */
  int c = *q;
  printf ("b = %08x, p = %p, q = %p, c = %08x\n", b, p, q, c);
  return 0;
}
```

if char convert to int, it is allowed, but if the pointer of char convert to the pointer of int, it is not allowed.

Different from the code above,  we can use upcast, it gets this to compile but a\[0\] will be wrong value.

```c
int main () {
  char a[] = { 0x85, 0x86, 0x87, 0x88 };
  int b = a[0];
  char *p = &a[0];
  int *q = (int *) p;  /* type checker accepts explict cast */
  int c = *q;
  printf ("b=%08x, c=%08x, p=%p, q=%p\n", b, c, p, q);
  printf ("b=%d, c=%d\n", b, c);
  return 0;
}

output:
clang -m32 typing-02.c && ./a.out
b=ffffff85, c=88878685, p=0x7ffee09a01e8, q=0x7ffee09a01e8
b=-123, c=-2004384123
```

In the code,   char pointer \*p upcast to int pointer \*q, but and store the value of p in variable c. but even it doesn't get compiler error, the result is wrong.

* **shape error: the same memory location is read and interpreted at an imcompatible type, called unchecked runtime type error**
  * cause by upcasting, downcasting, ~~unproper assignment~~\(it can be checked when compile time\)
  * shape error are silent and allow access.

```c
void update (int* p) {
    *(p+1) = 0xf0800000;
}
int main () {
    float f = 10;    
    int a[]  = { 10 };
    int i = 10;
                printf ("f=%f, a[0]=%d i=%d\n", f, a[0], i);
    a[-1] = 47; printf ("f=%f, a[0]=%d i=%d\n", f, a[0], i);
    update (a); printf ("f=%f, a[0]=%d i=%d\n", f, a[0], i);
}

output:
clang -m32 typing-03.c && ./a.out
f=10.000000 a[0]=10 i=10
f=10.000000 a[0]=10 i=47
f=-316912650057057350374175801344.000000 a[0]=10 i=47
(from high to low: a[1], a[0], a[-1])
```

When stack of main has 3 variables, f, a\[\], and i, when a\[-1\] is assigned, it change i, and when executing update function, the address of a is as parameter of the function the address a+1 is changed, which original belong to i, so i is changed.

* strong and weak
  * strong type guaratees no shape errors\(upcasting or downcasting ,and array problem causes incompatible type\)
  * weak type may permit shape error

```text
void unsafeCommand () { printf ("ouch!\n"); }
void safeCommand ()   { printf ("hurray!\n"); }
int guess () { return &unsafeCommand - &safeCommand; }
int main () {
    printf ("%d\n", guess ());
    void (*c) () = &safeCommand;

    c();
    c += -48;
    c();
}

output:
clang -m32 typing-04.c && ./a.out
-48
hurray!
ouch!
```

UnsafeCommand function print ouch!, safeCommand function print hurray!, guess ,using ouch!-hurray!=-48, the main function print guess\(\) result, which is -48, and save safeCommand function in a pointer c. When invoking function c\(\), the result is hurray!, and add -48 to c\(\), the result is ouch!  In this way, if your guess function is correct you could call anything, super unsafe. 

😂: That is shape error: reading memory in an incompatible type or data used contrary to type. However c is weak type, so it could run and no warning and error message.

```
void floatCommand (float f) { printf ("f=%f\n", f); }
void intCommand (int i)     { printf ("i=%d\n", i); }
int guess () { return &floatCommand - (void (*)(float)) &intCommand; }
int main () {
    printf ("%d\n", guess ());
    void (*c) (int) = &intCommand;
    int j = 0xf0800000;
    c(j);
    c -= 64;
    c(j);
} 

output:
clang -m32 typing-05.c && ./a.out
-64
i=-260046848
f=-316912650057057350374175801344.000000
```

function floatCommand take a float number and return, intCommand function take a int and return, guess\(\) return the floatCommand address- upcasting intCommand address. In the main function guess return -64, and j is the number -260046848, intCommand function is store in a pointer c, c take a int number and return correct number, when c is try to become floatCommand, it return an incorrect number.

* Strong and static language
  * Java, C\#, Scala, Rust, etc
* weak  and static language
  * C
* Strong and dynamic language
  * Scheme, python, ruby, etc

😂: if a language is static type, it is hard to find upcasting and downcasting causes problem, but dynamic type     is easy to find. To compensate that problem, static type checking language also add dynamic type checking.

```text
class A { int x; }
class B extends A { float y; }
class C extends A { char c; }

class Typing02 {
  public static void main (String[] args) {
    B b = new B ();
    A a = b;
    C c = (C) a;
  }
}

output:
javac Typing02.java && java Typing02
Exception in thread "main" java.lang.ClassCastException: B cannot be cast to C
```

B and C extends A, So, B and C type object could upcast to A type, and A type Object could downcast to B and C type. In the main function, b is B type object and upcast to A type, then downcast to a type.  What we know is Java is static type language, but the exception message indicate Java also have dynamic type checking to make sure it was a C type before , thus, it could find a downcast to a is wrong.

Java doesn't allow shape error, so it is strong type. Java's type checking is mostly static, runtime tests for only in certain cases.

* java dynamic checks in java 

```text
class A { int x; }
class B extends A { float y; }
class C extends A { char c; }

class Typing03 {
  public static void main (String[] args) {
      B[] bs = new B[1];
      A[] as = bs;
      as[0] = new C();
      B b = bs[0];
  }
}

output:
javac Typing03.java && java Typing03
Exception in thread "main" java.lang.ArrayStoreException: C
```

the main function create a new array, B type with 2 length,  and upcast array to A type, as\[0\] is the first cell are defined to C type, it is not allowed, and set b to bs\[0\],which is as\[0\].One array contains two type of element, which is not allowed.

```text
class A { int x; }
class B extends A { float y; }
class C extends A { char c; }

class Typing04 {
  public static void main (String[] args) {
    B[] bs = new B[4];
    C c = new C();
    B b = bs[-1];
  }  
}

output:
javac Typing04.java && java Typing04
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: -1
```

Class B and C extend Class A. In main function,  A b type array are initialized,  and a variable c is c type, b  is bs\[-1\], which is out of the bound of array bs. 

```text
class A { int x; }
class B extends A { float y; }
class C extends A { char c; }

class Typing05 {
  static void f (B b) {
    A a = b;       // upcast
  }
  static void g (A a) {
    B b = (B) a;   // downcast
  }
  public static void main (String[] args) {
      f (new B());
      g (new C());
  }  
}

output:
javac Typing05.java && java Typing05
Exception in thread "main" java.lang.ClassCastException: C cannot be cast to B
```

function f is for upcasting, function g is for downcasting. In the main, g take a C type  parameter treated as A type, and down casting to B type, it is not allowed.

* 😂 dynamic checking in java: what is checking for? 

  * the error which is not syntax error and not operator error which static checking could detected
  * such as array out of bounds exception, dividing 0 exception, stack overflow or array overflow exception, which is not error but will cause problem. In c it is called undefined behavior.

* 😂 Why java restrict use of pointers, instead using garbage collection?
  * if use pointer, user have to free the memory to deallocate, which will reuse the memory using before and cause safety problem.

#### Scala introduction

* Scala： functional and object-oriented PL
  * Boolean, numeric, and string
  * static type checking
* variable:
  * immutable variable: val x=10
  * mutable variable: var x=10
* expression oriented:no statements, like scheme, ML, etc
* comma expression:{e\_1;e\_2;e\_3...;e\_n} or {e\_1 e\_2 e\_3... e\_n}
* method：
  * def plus\(x:Int, y:Int\): Int=x+y
  * def also could use to define global variable
* Object: everything is object in Scala
  * 5.toDouble
  * 5.+\(5\) or 5+6
  * 5.max\(6\) or 5 max 6
* tuple: Immuable
  * val x:\(Int, String\)=\(5,"hello"\)
* Linked List:varying number of homogeneous items
  * \(cons 1\(cons 2 \(\)\)\)
  * 1：：（2：：Nil）
  * List（1，2，3）
  * xs.head; xs.tail
  * wrong mistake: xs\(1\)=7
* pattern matching

```scala
//pattern matching
val p : (Int, String) = // ...
p match {
  case (x, y) => "Int is %d".format (x)
}
```

```text
val xs : List[Int] = // ...
xs match {
  case Nil   => "List is empty"
  case y::ys => "List is non-empty, head is %d".format (y)
}
```

recursive function

* functional programming has immutable programming, which is different from regular programming
* wildcard\_:relative ignore head

```scala
// length: input a list,return a int
//count
def length [X] (xs:List[X]) : Int = xs match {
  case Nil   => 0
  case _::ys => 1 + length (ys)
}
```

* List build in appending function
* \(:::\):append between element and list
* \(::\): append between list and list

### worksheet

### homework

### quiz

## Lecture3

### class notes

#### background knowledge:parametric polymorphism

* 😂why using generic type: for no need to overload one function for every type of parameter, the object in java could be used, but when specify type, we have to downcast and downcasting will cause shape error, so Java create generic type.
* generic linked list in C

```c
//Use (void *) type in C
typedef struct node_int_ptr node_int_ptr;

struct node_int_ptr {
  int *item;
  node_int_ptr *next;
};

int length_while_int_ptr (node_int_ptr *xs) {
  int result = 0;
  while (xs) {
    result += 1;
    xs = xs->next;
  }
  return result;
}
```

Use `(void *)` type in C,Can cast/store any pointer type at type `(void *)`But it must cast from `(void *)` before dereferencing 



#### function programming

#### for expression



### worksheet

### homework

### quiz

## Lecture4

### class notes

#### type

#### scala introduction

### worksheet

### homework

### quiz

## Sala method

* List method

```scala
// isEmpty:input a list return boolean, spliting to two situation
// if list has element return true, else return false
def isEmpty [X] (xs:List[X]) : Boolean = xs match {
  case Nil   => true
  case y::ys => false
}
```

```scala
//head: input a list return a element of list(head)
//return first element of list, else throw exception
def head [X] (xs:List[X]) : X = xs match {
  case Nil   => throw new NoSuchElementException ()
  case y::ys => y
}
```

```scala
//tail:input a list return a list(without head)
//return all element without head, else throw a exception
def tail [X] (xs:List[X]) : List[X] = xs match {
  case Nil   => throw new NoSuchElementException ()
  case y::ys => ys
}
```

```scala
//append:intput two list with same type element,return one list
//if list a has element, append that element with new list recursively,
//if list a has no element, append list b to new list
def append [X] (xs:List[X], ys:List[X]) : List[X] = xs match {
  case Nil   => ys
  case z::zs => z::(append (zs, ys))
}
```

