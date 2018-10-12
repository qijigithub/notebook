# Introduction

## Scala

### closures: runtime support for nest function

1. Closures store inner function and environment, allocate on the heap

```text
val constant : Int => () => Int = {
    // TODO: Complete the definition.
   (y)=>()=>y;
  }
```

