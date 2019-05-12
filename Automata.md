# Automata

## Precedence

- Kleene closure > concatenation > union

## DFA/NFA

- DFA: only one path for a given letter from a given state, no epsilon transitions are allowed
- NFA: one or more paths for a given letter from a given state, may have epsilon transitions

## CFG

- derivation is a rewriting of the nonterminal using the production rules 
  - left derivation: derivation that rewrites the leftmost nonterminal first
  - right derivation: derivation that rewrites the rightmost nonterminal first

## Parsing

### First Sets

- First(y) is the set of initial terminals of all strings that y may expand to

### Recursive Descent

```ocaml
    let parse_S () =                                            (* where S -> xyz | abc *)
        if lookahead () = "x" then                              (* S → xyz *)
            (match_tok "x";
            match_tok "y";
            match_tok "z")
        else if lookahead () = "a" then                         (* S → abc *)
            (match_tok "a";
            match_tok "b";
            match_tok "c")
        else raise (ParseError "parse_S")
```

### Refactoring

- left-factoring algorithm to refactor overlapping first sets: `S -> ab | ac` is refactored into `S -> aL`, `L -> b | c`
- left-recursion algorithm to refactor left recursive CFGs: `S -> Sa | epsilon` refactored to `S -> L`, `L -> aL | epsilon`
  - trick is to figure out which nonterminal is required in order for recursion to terminate
    - (e.g. `S -> Sa | Sb | c` is refactored into `S -> cL`, `L -> aL | bL | epsilon`, `c` is required)