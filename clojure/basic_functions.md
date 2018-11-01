# Functions

Lisp's charm is its expressivity for it allows us to create complex programs
with simple and primary building blocks: ```functions```!

* Lisp is beautiful, elegant and simple!


## 1. Calling Functions

All clojure operations (expressions) have the SAME syntax:

```
(operator operand1 operand2 ...)
```

* Function call is just an expression whose operator is
a ```function``` or an ```expresion that returns a function.```

Example of an expression that returns a function:

```
(or + -)

;=> +
```

This returns the + function since everything (besides nil and false)
evals to truthy and is the returned value of the or function:

```
((or + -) 1 2)  ;; (or + -) evals to + function which can be used to add 1 2
;=> 3
```

* or function expression => evals to the FIRST TRUTHY argument (doesnt eval the rest)

* and function expression => evals to the last TRUTHY argument or the first which is FALSEY
  because the and function will not eval the arguments after there's a FALSEY.

```
(= 1 1) ;; evals to true
+       ;; evals to true and is the last truthy argument

(and (= 1 1) +) ;; evals to +

((and (= 1 1) +) 1 2) ;; evals to 3
;=> 3
```

The following are NOT valid because the violate the basic syntax:

```
(1 2 3)  ;; 1 is NOT a function
("hi" 2 3) ;; "hi" is NOT a function
```

Numbers and strings cannot be cast to functions!


### Higher-order functions

Clojure obviously treats functions as first-class citizens because
they can be passed as arguments, returned by functions, etc.

This allows powerful and expressive abstractions!

As an example:

```
(map inc [1 2 3])
;=> (2 3 4)
```

* Map does not return a vector! Later on we'll find the reason.

Clojure EVALS all functions arguments recursively before
passing them to the function. Hence, function CALLS
triggers the recursive evaluation process.

## Macro calls and special forms:

* Function calls have a ```function expression``` as the operator;

### Special forms

They are "special" because they dont ALWAYS evaluate ALL of their operands.
For example, ```if``` does not else part if the condition is true.

* We've seen some: if expressions, definitions, etc.

Another important thing is: they cannot be used as function arguments

### Macros

They also evaluate their operands in a diffent way than functions. We'll see more later.


## Functions definitions

Defining a function requires 5 main parts:

* defn operator;
* function name;
* doctstring (optional);
* parameters (listed in [square brackets] == vector);
* function body.

```
(defn say-hi
 "Returns a hi to the user"
  [name]
  (str "hey there " name "!")) 
```

To read a doc of a function:

```
(doc fn-name)
```

### Parameters and Function Arity

* Clojure functions --> zero or more parameters!
* The number of parameters --> arity of the function!

```
(defn no-params
 "no params fn ==> --arity"
 []
 "I take no params")
```
```
(defn two-param
 "has 2 params fn ==> 2-arity"
  [x y]
   (str "I take two params: " x y))
```

### Arity overloading

We can define multiple bodies for a single function so that
a specific body will be executed depending on the arity.

This is the right way to define "default values" for function
arguments.

```
(defn hit-someone
 "Hits someone with something."
 ([who something]
   (str "You've hit " who " with " something))
 ([who]
   (hit-someone who "default item")))
```

Syntax for arity-overloading:

* write EACH body wrapped in ();
* each body may do a completely different thing!

```
(def fn-name
  "doc string of the fn def"

  ([arg1 arg2]
    (body1))

  ([arg1]
    (fn-name arg1 "default-2")))
```

### Variable-arity functions

One can define functions that have arities that vary by using the
```rest parameter``` which is ```&```.

* The rest parameters are stored in list with the following name.
* We can mix rest parameters (stored in a list) with normal ones,
  but the rest parameters must be the last ones!

```
(defn cast-one
  [one]
  (println (str "get out: " one)))

(defn cast-out-many
  [& xs]
  (map cast-one xs)) ;; mapping is necessary because xs is a LIST
```

Mixing parameters with rest parameters (list params):

```
(defn fav-things
  "Greets someone and says his fav things"
  [name & things]
  (str "hey " name ", here are my fav things:" (clojure.string/join ", " things)))
```

Remeber, when dealing with rest parameters --> you must deal with a LIST!

### Destructuring (Pattern matching)

Clojure offers pattern matching! With it, we can bind names to values
within a collection based on a data structure form!

We can pattern match to Clojure's data strucutures:

* vectors, by supplying a vector as a parameter of the function;
* maps, by supplying a map as a parameter of the function;
* sets, lists, etc.

#### Lists

* Notice that we supply a vector INSIDE the parameters []

```
(defn get-first
  "Gets the first element"
  [[first-elmnt]]  ;; [first-elmnt] => pattern that matches a vector and its first element!

  first-elmnt)
```

```
(defn get-two-first
  "Gets the first element"
  [[first-elmnt second-elmnt]]  ;; [first-elmnt] => pattern that matches a vector and its first element!

  [first-elmnt second-elmnt]) ;; returns a vector with the two first elements
```

```
(defn get-two-first
  "Gets the first element"
  [[first-elmnt second-elmnt]]  ;; [first-elmnt] => pattern that matches a vector and its first element!

  [first-elmnt second-elmnt]) ;; returns a vector with the two first elements
```

```
(defn choose-two
  [[first second & tail]]
  
  (println (str "first choice: " first))
  (println (str "second choice: " second))
  (println (str "rest of the choices: " (clojure.string/join ", " tail))))
```

#### Maps

We can also destructure maps! Now we supply maps to the parameters []

```
(defn get-treasure-location
 [{lat :lat lon :lon}] ;; here we bind the variable lat to the key :lat!

 (println (str "the latitude is: " lat))  ;; since we pattern matched --> lat is available
 (println (str "the longitude is: " lon)) ;; since we pattern matched --> lon is available)
```

* Like vectors, anything that is not pattern match gets bound to nil!

There is also a syntatic sugar form for this:

```
(defn get-treasure-location
 [{:keys [lat lon]}] ;; here we lat and long are bound to the keys!

 (println (str "the latitude is: " lat))  ;; since we pattern matched --> lat is available
 (println (str "the longitude is: " lon))) ;; since we pattern matched --> lon is available
```

* To retain access to the original map --> ```:as``` keyword!

```
(defn get-treasure-location
 [{:keys [lat lon] :as treasure-loc}] ;; here we lat and long are bound to the keys!

 (println (str "the latitude is: " lat))  ;; since we pattern matched --> lat is available
 (println (str "the longitude is: " (:lon treasure-loc)))) ;; since we pattern matched --> lon is available
```

## Function bodies

Bodies can contain ANY form. Clojure automatically returns the LAST form evaluated

```
(defn test
  []
  (+ 1 2)
  30
  "hi") ;; last evaluated form is "returned" (~ like Ruby)
```

```
(defn test
  []
  (println "hi")
  (println "hey")
  (println "last one!")) ;; last evaluated form is "returned" (~ like Ruby)
```

## Anonymous Functions

Functions do not require names! There are two ways to create anonymous fns:

* ```fn``` form;
* using the syntatic sugar form: ```#```

### Using fn

* Treate fn in the same way as defn! Here is the syntax:

```
(fn [param-list] body)
```

An example with map:

```
(map (fn [name] (str "hey there: " name)) ["igor" "aline" "others"])
```

An anonymous function call:

```
((fn [x] (* 2 x)) 10)
```

Associating an anonymous function with a name (~ like Javascript)

```
(def not-anonymous-anymore (fn [x] (* 3 x)))
```

### Using syntatic sugar

* We can also create anonymous functions in a very compact way!

```
#(body %)  ;; % indicates an argument passed to the function
#(body %1 %2)  ;; %1, %2 can be used if the fn takes many arguments
```

```
(map #(str "hey there: " %) ["igor" "aline"])
```

* A rest parameter can also be passed --> ```%&```!

When to use each? As a rule of thumb:

* Use # form for very short functions!
* Use fn form for longer functions that require more readability!

## Returning Functions

Returned functions are actually ```closures```. This means that
Functions are, in fact, stored together with the lexical environment
where they were created at first place!

```
(defn inc-maker
  "Returns a function that knows how to increment values."
  [inc-by]

  #(+ % inc-by)) 

((inc-maker 3) 2) ;; returns a function that remembers its env inc-by = 3
;=> 5
```