# OCaml


## Records

```ocaml
    type date = { month: string, day: int, year: int };;        (* fields are accessible by name *)
    let today = { day = 16; year = 2017; month = "f"^"eb" };;
    today.day;;                                                 (* fields accessible by dot notation *)
    let { day = d } = today;;                                   (* fields accessible by pattern matching (d has the value 16) *)
```
