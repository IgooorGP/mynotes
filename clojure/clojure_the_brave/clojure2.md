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

If operator but without the else branch. Useful when we want the else branch
to eval to nil

(when true (println "true"))

### nil

Represents no value in Clojure (like None in Python). To test for nil:

(nil? exp)

### Truthy and Falsey

Truth and Falsy => how an expression if treated in a boolean-expected expression.

* In clojure, only ```nil```and ```false```are evaluated to false;
* Everything else (numbers, ints, strings, etc.) are evaluated to true;
* ```true``` and ```false```are the literals for booleans in Clojure.

### Equality operator

* nil = nil!

```
(= 1 1)
```

### Boolean operators

Clojure uses ```and``` + ```or``` as boolean operators.

```clojure
(and exp1 exp2)
(or exp1 exp2)
```

# Binding names to values (var bindings)

The operator ```def``` is used for value binding to variables.

```
(def x 2)
```

* Clojure, like other functional languages, uses binding as the term
and not var assignment. And that is because here multiple assignments
to the same var is ```rarely necessary```. For some very special cases,
there are some tools to deal with mutability.

# Simple functions

The operator ```defn``` is used to create function bindings.

(defn test [number] 
      (if (> number 2) (println "bigger than 2") (println "smaller than 2")))

