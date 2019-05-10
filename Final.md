# Rust

## Basics

```rust
fn main() {
    println!("Hello world!");   // prints "Hello world!"
    println!("{} is 5," 5);     // prints "5 is 5"
}
```
```rust
fn fact(n: i32) -> i32 {        // function that takes in a 32-bit integer and returns a 32-bit integer
                                // type annotations are required for function signatures
    if n == 0 { 1 }              
    else {                      // last executed statement is automatically returned (unless there's a semicolon)
        let x = fact(n - 1);    // also, no parentheses are present around the if-guard
        n * x                   // variables are assigned using let
    }
}
```

### Variables
```rust
fn main() {
    let mut a: i32 = 0;         // variables are immutable by default in Rust
    a = a + 1;
    println!("{}" , a);
} 
```
```rust
fn main() {
    let n = 5;
    let x = if n < 0 {          // if-expressions must return the same type of data (this will cause compile error)
        10
    } else {
        "a"
    };                          // semicolon required or else compiler will think result of expression is returned
    print!("{:?}|",x);
}
```
```rust
{
    let x = 37;
    x = x + 5;                  // error since x is not mutable
    x
}
```
```rust
{                               
    let x:u32 = -1;             // error since u32 means unsigned integer
    let y = x + 5;
    y
}
```
```rust
{
    let x = 37; 
    let x = x + 5;              // block returns 42 (redefining a variable shadows it)
    x
}
```
```rust
{
    let x:i16 = -1;             
    let y:i32 = x + 5;          // error since there is no automatic type conversion
    y
}
```

### Looping

```rust
    for x in 0..10 {            // ranges a..b are values in [a, b)     
        println!("{}", x);      // prints 0 through 9 (10 is excluded)
    }
```
```rust
    let mut x = 5;
    let y = loop {              // infinite loop that executes until broken out of
        x += x - 3;
        println!("{}", x);      // 7 11 19 35
        x % 5 == 0 { break x; } // break can take an expression, which becomes the final result of the loop
    };
    print!("{}", y);            // prints 35
```
```rust
    let mut x = 1;
    for i in 1..6 {
        let x = x + 1;
    }
    x                           // returns 1 since all of the x's in the loop are scoped to that loop
```

### Tuples

```rust
    let tuple = ("hello", 5, 'c’);      // tuples are collections of disparate data
    assert_eq!(tuple.0, "hello");       // tuple fields can be accessed by indexing
    let (x, y, z) = tuple;              // tuple fields can be accessed by pattern matching
```
```rust
fn dist(s:(f64,f64), e:(f64,f64)) -> f64 {  // accesses tuple fields via pattern matching
    let (sx,sy) = s;                       
    let ex = e.0;                           // accesses tuple fields via indexing
    let ey = e.1;
    let dx = ex - sx;
    let dy = ey - sy;
    (dx*dx + dy*dy).sqrt()                  // returns euclidean distance between two points
}
```
```rust
fn dist2((sx,sy):(f64,f64),(ex,ey):(f64,f64)) -> f64 {  // can include patterns in parameters directly
    let dx = ex - sx;
    let dy = ey - sy;
    (dx*dx + dy*dy).sqrt()
}
```

### Arrays

```rust
    let nums = [1,2,3];                                 // default (immutable) array declared
    let strs = ["Monday", "Tuesday", "Wednesday"];      
    let x = nums[0];                                    // x is assigned 1
    let s = strs[1];                                    // s is assigned "Tuesday"
    let mut xs = [1,2,3];                               // declaration of a mutable array
    xs[0] = 1;                                          // allowed since xs is mutable
    let i = 4;
    let y = nums[i];                                    // fails (panics) at run-time since index must be known
```
```rust
    fn f(n:[u32]) -> u32 {                              // won't pass type-checking since array size is unknown at compile-time
        n[0]
    } 
    
    fn f(n:[u32; 5]) -> u32 {                              // will pass type-checking since array size is known at compile time
        n[0]
    } 
```