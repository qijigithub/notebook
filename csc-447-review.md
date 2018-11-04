# CSC 447 review

## Lecture 1

### class notes

#### overview

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

* contio!!nal  statment and condictional expression
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

* undfined behavior
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
* 
### worksheet

### homework

### quiz

## Lecture2

### ppt

### class notes

### worksheet

### homework

### quiz

## Lecture3

### ppt

### class notes

### worksheet

### homework

### quiz

## Lecture4

### ppt

### class notes

### worksheet

### homework

### quiz

