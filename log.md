# Crafting bytecode interpreter

## 2023-3-12

A global operand stack is not unique, and it can even contain local variables.

Variable that are closed over create a challenge, since they are popped from the
stack as a function returns. How to deal?

Apparently by creating an environment object on return, and redirecting any
surviving closures to it.

Lambda lifting apparently takes quadratic time for the compiler. This technique
seems to push the burden to the runtime somehow.

A closure still uses stack offsets, but these are 'redirected' to runtime values
somehow...

Note the dynamic creation of classes etc.

### bytecode verification

clox doesn't need this, but if compilation & execution are separated, this is
not a luxury: bad bytecode could wreak havoc.

Bytecode verification is a good place for type checking as well, I guess.

### done!

Scare with a last bug, which was due to following the no brace convention
advocated by Henley. Small C context in 'Crafting' was part of the problem
though.

## 2023-3-8

### Crafting ideas

Backpatching: emit placeholders & put an offset in place when that value becomes
known.

Top level implicitly being a function body. This abstraction leaks, but does it
have to?

So the operand stack is not inside the frame of the operator stack, or is this a
Lox thing?

## 2023-3-6

Three takes on encapsulation:

- closure: an object is a hash table containing closures, and the values that
  the object closed over are hidden from the caller.
- images: a instance of a class behaves like an image of its private fields,
  under the function defined by the class declaration.
- generics: but type variables are similarly inaccesible. one can think of a
  'trait' that way, though generics supposedly live on another level.

## 2023-3-4

Trouble: 1+2*3-4/5. Used = instead of == in IS_OBJ macro. Crafting showed hash
tables, which was interesting.

## 2023-3-1

Learning C on the go... Exciting.

C installed, vscode ready.

I cannot geet vscode to compile everything, but the following command compiles
the code: `gcc *.c -o run`. to run: `.\run`. As usual, all the bells and
whistles dirve me nuts. The command line makes live simple. Bfeore running the
code, I had to tell Ariva to make an exception of this folder.

### line numbers

Interesting point: do not put line number between instructions, because they
will get into the cache, where they are not useful. Clox has a line number for
each line of code. Perhaps they could be distances and differences instead (if
they increment infrequently...).
