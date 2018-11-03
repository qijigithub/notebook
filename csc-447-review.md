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
* sequencing:who will execute
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
  * order of operations
  * dangling pointers

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

