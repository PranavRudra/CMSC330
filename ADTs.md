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
            type set = Empty | Ins of int * set                 
            ...
            let insert s i = Ins(i,s)
        end
```