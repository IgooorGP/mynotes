# Basic Language Syntax

Clojure understands two kidns of structures:

* Literal data representation (strings, numbers, vectors...)
* Operations (how we do things...)

Clojure forms = valid code/ valid expressions!

* Literal representations are valids forms and are evaluated;
* Clojure evaluates EVERY expression.

```clojure
1
"a string"
["a" "vector" "of" "strings"]
```

## Operations

ALL operations have the following form:

```clojure
(operator operand1 operand2 ...)
```

Since this is LISP, there are NO commas. Whitespaces ARE the separators.

```clojure
(+ 1 2 3)
;=> 6
```
```clojure
(str "concat" " this string!")
```

As we can see, just like LISP, clojure uses the prefix notation (no infix).

## Control flow

### if

The bool-form is an expression that evals to a truthy or falsey value.

```clojure
(if bool-form then-form opt-else-form)
```

* nil --> like None or null for some languages;
* opt-else-form is optional

### do

Allow us to eval multiple forms. The LAST form is the result of the
evaluation of the WHOLE do expression.

```clojure
(do arg1 arg2 arg3-and-eval-result)
```

### when

When is a combination of if operator + do operator.