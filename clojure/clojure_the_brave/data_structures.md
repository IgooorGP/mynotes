# Clojure Data Structures

ALL Clojure's data structures are immutable!!

## Numbers

Clojure handles a variety of number types such as:

* Long (~ long int);
* Double (double precision floating points);
* Big Ints (N);
* Big Decimal (M).

```clojure
(type 1)
;=> java.lang.Long
```
```clojure
(type 1.0)
;=> java.lang.Double
```
```clojure
(type 1N)
;=> java.lang.BigInt
```
```clojure
(type 1M)
;=> java.lang.BigDecimal
```

## Strings

Clojure only allows double quotes " for string literals.
There is also NO string interpolation in Clojure. Therefore,
one must concatenate strings using the ```str```function.

* For using double quotes in a string, escape them like ```\"```

```clojure
(def name "igor")
(def name2 "\"name with quotes\"")
```

## Maps

Maps are like dictionaries or hash tables in other languages. There
are two types of hashes in Clojure:

* hash maps;
* sorted maps.

Some map literals:

```clojure
{} ; empty hash
```

Map with some keys and values:

```clojure
{:first_name "igor" :last_name "gp"}
```

The values can be of any type: numbers other maps (nested), functions, etc!

Nested maps:

```clojure
{:name {:first "igor" :last "gp"} :age 27}
```

The function ```hash-map``` can also be used to create hash maps.

```clojure
(hash-map :key1 1 :key2 2)
```

Getting Map values: we use the ```get```function!

```clojure
(get (hash-map :key1 1 :key2 2) :key2)  ; gets the value 2 associated with the :key2
;=> 2
```

* ```get``` function returns ```nil``` if the key is not found;
* one can also set a default value for the get if the key is not found (like Python's get)

```clojure
(get (hash-map :key1 1 :key2 2) :none "nothing found")
;=> "nothing found"
```

Getting values of ```nested maps``` is also possibile --> ```get-in``` function!
Send a list of nested keys!

```clojure
(get-in {:name {:first "igor" :last "gp"}} [:name :last])
;=> "gp"
```

Also, maps can also be treated AS functions to get values from the keys!

```clojure
({:first "igor" :last "gp"} :last)
;=> "gp"
```

### Keywords

They are like LISP symbols. Commonly used in maps as keywords.

* They can also be treated AS functions to look up values in a map which
is sorta like the opposite way of getting values from maps when those
are used as a function.

* Using keywords AS functions is a thing among real clojurists!!

```
(:a {:a 1 :b 2} "default value")
;=> 1

({:a 1 :b 2} :a "default value")
;=> 1
```

### Vectors

Vectors are similar to arrays! They are 0-indexed collections!

```
[3 2 1]
```

Once again, we can use get ```get``` function to get values
from these collections but now using INDEXES rather than keys.

```
(get [1 2 3] 0)

;=> 3
```
Indexes that are bigger than the array's size returns ```nil```.

Arrays can hold different types of data.

```
(get ["a" {:first "igor" :last "gp"} 2] 1)

;=> {:first "igor" :last "gp"}
```
```vector``` function can be used to create vectors

```
(vector "hi" 1 2)
```
* Vectors are dynamic structures! Use the ```conj``` function
to append elements to the end of the array

```
(conj [1 2 3] 4)
```

### Lists 

Lists, like vectors, are LINEAR collection of values. 

```
`(1 2 3 4)

;=> (1 2 3 4)  ;; no single quote, we'll learn later why
```

To get values from this data structure:

```

```
