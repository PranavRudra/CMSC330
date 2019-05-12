# OCaml


## Records

```ocaml
	type date = { month: string, day: int, year: int };; 		(* fields are accessible by name *)
	let today = { day = 16; year = 2017; month = "f"^"eb" };;
	today.day;;													(* fields accessible by dot notation *)
	let { day = d } = today;;                                   (* fields accessible by pattern matching (d has the value 16) *)
	empty;;                                                     (* error: unbound value empty *)
    # IntSet.empty;;                                            (* - : IntSet.set = IntSet.Empty printed out *)
    # IntSet.contains IntSet.empty 1;;                          (* - bool = false is printed out *)
    # open IntSet;;                                             (* add IntSet names to curr scope *)
    # empty;;                                                   (* - : IntSet.Empty printed out *)
```
