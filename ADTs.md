# ADTs

## Objects and Closures

- object is a collection of methods (code) and fields (data)
- closure is a function body (code) and environment (data)

```java
    class C {                           
        int x = 0;
        void set_x(int y) { x = y; }                // setter function
        int get_x() { return x; }                   // getter function
    }
    
    C c = new C();
    c.set_x(3);
    int y = c.get_x();
```
```ocaml
    let make () =                                   (* ocaml's version of a "constructor" *)
        let x = ref 0 in
        ( (fun y -> x := y), (fun () -> !x) )       (* setter and getter functions *)
    ;;
    
    let (set, get) = make ();;
    set 3;;                                         (* updates x since x is in the environment of the setter closure *)
    let y = get ();;
```

## Modules

```ocaml
    module IntSet =                                             (* implementation of a set *)
        struct
            type set = Empty| Ins of int * set                  (* definition of set data type *)
            let empty = Empty                                   
            let isEmpty s = (s = Empty)                     
            let insert s i = Ins(i,s)
            let rec contains s i =
                match s with
                    | Empty -> false
                    | Ins(j,r) -> i = j || (contains r i)
        end;;
```
```ocaml
    empty;;                                                     (* error: unbound value empty *)
    # IntSet.empty;;                                            (* - : IntSet.set = IntSet.Empty printed out *)
    # IntSet.contains IntSet.empty 1;;                          (* - bool = false is printed out *)
    # open IntSet;;                                             (* add IntSet names to curr scope *)
    # empty;;                                                   (* - : IntSet.Empty printed out *)
```
```ocaml
    module type FOO =                                           (* defines module signature and exposes add function *)
        sig                                                     (* signature names are all caps by convention *)
            val add : int -> int -> int                         (* default signature exposes everything in module *)
        end;;
        
    module Foo : FOO =                                          (* creates a module of the above type *)
        struct
            let add x y = x + y
            let mult x y = x * y
        end;;
    
    Foo.add 3 4;;                                               (* ok, since add is exposed by the type definition *)
    Foo.mult 3 4;;                                              (* not accessible since mult is not exposed *)
```

## ADT

- Hide data's internal representation from clients while exposing functionality to operate on that data

```ocaml
    module type INT_SET =
        sig
            type set            (* abstract so that it can be used to define function signatures but not be exposed *)
            val empty : set
            val isEmpty : set -> bool
            val insert : set -> int -> set
            val contains : set -> int -> bool
        end;;

    module IntSet : INT_SET =
        struct
            type set = Empty | Ins of int * set     (* clients can define their own set data type *)                 
            ...
            let insert s i = Ins(i,s)
        end
```
```ocaml
    IntSet.insert IntSetBST.empty 0;;   (* mixing IntSet, IntSetBST not allowed since they may have different representations *)
```

## Types

### Static Typing
- well-typed programs are accepted by the language's type system
- well-defined programs are defined by the language's semantics (i.e. `char buf[4]; buf[4] = 'c';` isn't well-defined)
- type-safe language is one in which well-typed programs are always well-defined (i.e. well-typed => well-defined)

```ocaml
    4 + "hi";;                                  (* undefined *)
    if true then 0 else 4 + "hi";;              (* well defined since expression always evaluates to 0, but still rejected by type system *)
    let f4 x = if x <= abs x then 0 else 4+"hi" (* well defined since expression always evaluates to 0, but still rejected by type system *)
```

### Dynamic Typing
- type of an expression is checked as needed at runtime 
- values keep a tag, set at creation, that indicate its type
- disallowed operations cause runtime exceptions rather than compilation issues

```ruby
    def f(x, y)                                 # execution defined in all cases (throws a TypeError exception if input isn't valid
        if x < 0 then z = "0" else z = x end
        z / y
    end
```

### Implications
- languages that are dynamically typed are basically well-defined in every case (when viewed from a static typing perspective)
- in Ruby everything is an object but some operations throw exceptions (can't use all operations on all data types)
  - runtime environment knows how to deal with issues (by throwing an exception) so execution is well-defined

### Properties
- soundness property: well-typed => well-defined
- complete property: well-defined => well-typed
  - dynamic type systems are often complete since everything is well-defined and passes static type checking
  - static type systems not usually complete since not all behaviors are well-defined (i.e. previous C example)
