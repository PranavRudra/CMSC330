# Rust

## Overview

- memory management without garbage collection

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
    
    fn f(n:[u32; 5]) -> u32 {                           // will pass type-checking since array size is known at compile time
        n[0]
    } 
```

## Ownership

### Overview

- each value in Rust has a variable that is its owner
- there can only be one owner at a time
- when the owner goes out of scope, the value will be dropped (freed)

```rust
{
    let mut s = String::from("hello");              // s's contents are allocated on the heap
    s.push_str(", world!");                         // appends to s
    println!("{}", s);                              // prints hello, world!
}                                                   // s goes out of scope so its data is dropped (freed)   
```
```rust
    let x = String::from("hello");                  
    let y = x;                                      // heap-allocated data is copied by reference 
                                                    // x and y point to the same string "hello"
                                                    // y is now the owner though (x has been moved to y)
                                                    // x can't be used to access the string "hello" on the heap now
```
```rust
    let x = String::from("hello");                  
    let y = x.clone();                              // x is no longer moved since a deep copy of it was made
    println!("{}, world!", y);                      // ok, since x and y are owners of two different strings
    println!("{}, world!", x);                      // ok
```
```rust
    let x = 5;
    let y = x;
    println!("{} = 5!", y);                         // ok, since primitives (like i32) are copied automatically
    println!("{} = 5!", x);                         // ok, since x and y are owners of two different values
```
```rust
    fn main() {
        let s1 = String::from(“hello”);
        let s2 = id(s1);                            // calling function moves s1 to argument of that function
        println!(“{}”, s2);                         // ownership of the "hello" string is given to s2
        println!(“{}”, s1);                         // fails, since s1 wasn't made owner of string again (s2 is the owner)
    }
    
    fn id(s:String) -> String {
        s                                           // ownership of s given to caller, on return
    }
```

### References

- explicit, non-owning pointer to the original data
- cannot modify the data (immutable by default)
- creating a reference is called borrowing and is done through the & operator

```rust
fn main() { 
    let s1 = String::from(“hello”);
    let len = calc_len(&s1);                        // lends a non-owning pointer to the argument of the function
    println!(“the length of ‘{}’ is {}”, s1, len);
}

fn calc_len(s: &String) -> usize {
    s.push_str(“hi”);                               // fails, references can't mutate underlying data by default
    s.len()                                         // s is dropped but not its referent (owner)
}
```

- at any time, there can be one mutable reference OR any number of immutable references
- invariant is that the content pointed to won't change while my reference is alive

```rust
{ 
    let mut s1 = String::from(“hello”);                     // s1 is the owner of the mutable string "hello"
    { 
        let s2 = &s1;                                       // creates immutable reference s2 to the mutable string
        println!("String is {} and {}", s1, s2);            // ok, since this is read-only behavior
        s1.push_str(" world!");                             // can't modify original value until references dropped
    }                                                       // s2 is dropped at this point (mutation now allowed)
    s1.push_str(" world!");                                 // ok, since s1 is owner and no references exist now
    println!("String is {}", s1);                           // prints "hello world!"
}
```
```rust
    let mut s1 = String::from(“hello”);                     // s1 is the owner of the mutable string "hello"
    { 
        let s2 = &s1;                                       // s2 is an immutable reference to the mutable string "hello"
        s2.push_str(“ there”);                              // disallowed since s2 is an immutable, read-only reference
    }                                                       // s2 is dropped at this point (but not referent)
    let mut s3 = &mut s1;                                   // ok since s1 is a mutable reference
    s3.push_str(“ there”);                                  // ok since s3 is a mutable reference
    println!(”String is {}”, s3);                           // ok
```
```rust
    let mut s1 = String::from(“hello”);                     // s1 is the owner of the mutable string "hello"
    { 
        let s2 = &mut s1;                                   // s2 is a mutable reference to the mutable string "hello"
        let s3 = &mut s1;                                   // fails, cannot have more than one mutable reference at a time
        s1.push_str(“ there”);                              // fails, push_str takes a mutable reference (not allowed for above reason)
    }                                                       // s2 is dropped here
    s1.push_str(“ there”);                                  // ok: s1 is owner, string is mutable, no references are alive
    println!(”String is {}”, s1);                           // ok
```
## Data Structures

### Strings

- stored on the stack as a 3-tuple (pointer to heap data, length, capacity)
- heap data is dropped when the owner of the data goes out of scope
- length is always <= capacity

### Slices

- string slice (type `&str`) is stored on the stack as a 2-tuple (pointer to heap data, length)
- if `s` is a string then `&s[range]` is a slice where range has following properties
  - `i..j` is the range [i, j)
  - `i..` is the range from i to the current length (basically i to the end)
  - `..j` is the range [0, j) (basically from 0 to j excluding j)
  - `..` is the range from 0 to the current length (basically entire string)
 
```rust
    let mut s = String::from("hello world");        // s is the owner of the mutable string "hello world"
    let word = first_word(&s);                      // slice argument of first_word borrows from s (slices are references)
    s.clear();                                      // error can't drop data while reference is alive; violates invariant
```

```rust
    let s:&str = "hello world";                     // string literals ARE slices 
```

### Vectors

```rust
    { 
        let mut v:Vec<i32> = Vec::new();            // v is the owner of a mutable vector of 32-bit integers
        v.push(1);                                  // adds 1 to v
        v.push(“hi”);                               // error – can't add a string literal to a vector
        let w = vec![1, 2, 3];                      // w is the owner of an immutable vector of 32-bit integers
    }                                               // v, w and their elements are dropped here
```
```rust
    let v = vec![1, 2, 3, 4, 5];
    let third:&i32 = &v[2];                         // panics if index is out of bounds
    let third:Option<&i32> = v.get(2);              // returns None if index out of bounds
```
```rust
    let mut a = vec![10, 20, 30, 40, 50];           // a is the owner of the mutable vector
    { 
        let p = &mut a[1];                          // mutable borrow of the second element
        *p = 2;                                     // updates the second element of the vector
    }                                               // restores ownership to a
    println!("vector contains {:?}", a);            // ok
```
```rust
    let a = vec![10, 20, 30, 40, 50];               // a is the owner of an immutable vector
    let b = &a[1..3];                               // b is an immutable slice holding [20,30]
    let c = &b[1];                                  // c is an immutable reference to second element of b
    println!("{}",c);                               // prints 30
```

## Traits

- Rust's version of interfaces

```rust
    pub trait Summarizable {
        fn summary(&self) -> String;                // &self indicates that this is an "instance" method
    }
```
```rust
    impl Summarizable for (i32, i32) {              // (i32, i32) indicates trait is implemented for 2-tuples of 32-bit integers
        fn summary(&self) -> String {               
            let &(x, y) = self;                     // extracting the two 32-bit integers from the tuple via pattern matching
            format!("{}", x + y)
        }
    }
```
```rust
    pub trait Summarizable {
        fn summary(&self) -> String {               // default implementation to be used by arbitrary type
            "random string".to_string()
        }
    }
```
```rust
    impl Summarizable for (i32, i32, i32) {}
    (1, 2, 3).summary()                             // return "random string"
```

### Trait Bounds

```rust
    pub fn notify<T: Summarizable>(item: T) {       // type T can be anything as long as it implements the Summarizable trait
        println!("Breaking news! {}",
        item.summary());                            // since T implements Summarizable, we can call summary() on item
    }
```
```rust
    fn foo<T:Clone + Summarizable>(…) -> i32 {…}            // type T must implement Clone AND Summarizable
    fn foo<T>(…) -> i32 where T:Clone + Summarizable {…}    // alternate syntax for the previous method
```
```rust
    fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {     
        let mut largest = list[0];                          // Copy trait ensures we can get change ownership to largest
        for &item in list.iter() {                          // Copy trait ensures that we can "move out of borrowed"
            if item > largest {                             // PartialOrd trait ensures we can perform this comparison  
                largest = item;
            }
        }
        largest
}
```

## Custom Types

### Structs

```rust
    struct Rectangle {
        width: u32,                 // stack-allocated since size is known at compile-time
        height: u32,
    }
    
    fn main() {
        let rect1 = Rectangle { width: 30, height: 50 };    // rect1 is the owner of the created immutable rectangle struct
        println!("rect1’s width is {}", rect1.width);       // accessing fields done through dot notation
    }
```
```rust
    fn main() {
        let rect1 = Rectangle { width:30, height:50 };
        println!("rect1 is {}", rect1);                     // cannot print since it doesn't know how to
    }                                                       // solution is to add #[derive(Debug)] (uses debug printing)
```