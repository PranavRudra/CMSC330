# OCaml


## Records

```ocaml
    type date = { month: string, day: int, year: int };;        (* fields are accessible by name *)
    let today = { day = 16; year = 2017; month = "f"^"eb" };;
    today.day;;                                                 (* fields accessible by dot notation *)
    let { day = d } = today;;                                   (* fields accessible by pattern matching (d has the value 16) *)
```

## Custom Types

```ocaml
    type coin = Heads | Tails;;                                 (* coin is basically an enum with two options *)
    type mylist = int * (int list);;                            (* mylist is a tuple of an int and an int list *)
    let add x ((n, xs) : mylist) : mylist = (n+1, x::xs);;      (* type annotations indicate type should be mylist and not int * (int list) *)
```
```ocaml
    type shape = Rect of float * float | Circle of float;;
    let area s =
        match s with
        |   Rect (w, l) -> w *. l                               (* custom data types are deconstructed with pattern matching *)
        |   Circle r -> r *. r *. 3.14
```
```ocaml
    type 'a mylist = Nil | Cons of 'a * 'a mylist;;             (* recursive data type *)
    let lst = (Cons (10, Cons (20, Cons (30, Nil))));;          (* instantiating a mylist *)
```

## Imperative

### Refs

```ocaml
    let z = 3;;                                                 (* z is immutably bound to the value 3 *)
    let x = ref z;;
    let y = x;;                                                 (* x, y both point to the area of memory holding 3 *)
    x := 4;;                                                    (* modifying x does not change z since it is immutable *)
    !y;;                                                        (* modifying x does change y since they both point to the same area of memory *)
```
```ocaml
     let f =
        let y = ref 0 in                                        (* y is repeatedly incremented by the argument z *)          
        fun z -> y := !y + z; !y                            
    ;;
    
    let g = f ();;                                              (* g now equals fun z -> y := !y + z; !y *)
    g 1;;
    g 1;;                                                       (* returns 2 since enclosed y is incremented by 1 *)
```
```ocaml
    type 'a rlist = Nil | Cons of 'a * ('a rlist ref);;         (* refs enable cyclical data structures like rlist *)
    let newcell x y = Cons(x, ref y);;
    let updnext (Cons (_, r)) y = r := y;;
    
    let x = newcell 1 Nil;;                                     (* creates an rlist where the next pointer points to Nil *)
    updnext x x;;                                               (* changes the next pointer to point back to the rlist itself *)
    x == x;;                                                    (* physical equality check evaluates to true now *)
    x = x;;                                                     (* structural equality check hangs since data type is recursive*)
```
