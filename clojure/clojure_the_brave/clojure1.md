# Clojure Intro

## Clojure Language

Clojure is a lisp dialect (crafted at the MIT). Because of LISP,
it's very expressive and excellent for concurrent programming.

Also, Clojure is a ```hosted language``` --> executed within JVM
and relies on it for garbage collection, threading, etc.

Clojure may also be executed by Microsoft Common Language Runtime (CLR).

It's independent of any implementation.

# Clojure Compiler

A jar file (clojure.jar) which compiles code to bytecode for the JVM.


```source code``` ---> clojure.jar ---> ```java bytecode``` ---> Executed JVM


## Leiningen

After installing JVM on your os, install lein script (shebanged to use bash) at /bin (unix). After that, lein can be used as an executable (interpreted by bash). To generate a new app with a default folder structure:

```
lein new app <app-name>
```

### Leiningen folder structure

* ```project.clj```: config file for clojure (packages, dependencies, fn to run first, etc);
* ```/assets```: static files like images;
* ```/src```: source code;
* ```/src/app_name/core.clj```: contains the -main function (defined in project.clj) in a given namespace;
* ```/test```: tests

To run the code (eval):

```lein run```

To create a jar file:

```lein uberjar```

Finally, to execute the jar file with the JVM:

```java -jar target/uberjar/something.jar```

## REPL for clojure

Go to the root of the clojure app and execute:

```lein repl```

REPL development is an essential part of the LISP experience!! Use it!