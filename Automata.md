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