# Lambda Calculus

## Rules

- scope of lambda extends as far right as possible
- function application is always left-associative
- alpha conversion can only rename bound variables (not free ones)

## Evaluation

- call-by-value is eager evaluation (evaluate arguments before substituting into the function)
- call-by-name is lazy evaluation (delay argument evaluation until substitution into function)