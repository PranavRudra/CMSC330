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
