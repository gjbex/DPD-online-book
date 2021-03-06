# GCC C/C++ compiler flags

By default, the `gcc` and `g++` compilers issue some warnings, but it can produce more.  It is always good practice to switch on "all warnings" with `-Wall`.  Contrary to expectations, `-Wall` doesn't activate all warnings though.  A number of extra warnings on top of those enabled by `-Wall` can be switched on by specifying `-Wextra`.  It is highly recommended to always use these two compiler options, and eliminate all warnings reported by them, i.e.,

~~~~bash
$ gcc -Wall -Wextra ...
~~~~


## Language specification conformance

The `gcc` compiler can also check for language specification conformity."  The `-Wpedantic` flag will activate this, and you should specify the specification it should check, i.e.,

  * `-std=c89`
  * `-std=c99`
  * `-std=c11`
  * `-std=c17`

For `g++`, these values are:

  * `-std=c++98`
  * `-std=c++11`
  * `-std=c++14`
  * `-std=c++17`

For example, to check for C++14 compliance, use

~~~~bash
$ g++ ... -Wpedantic -std=c++14 ...
~~~~


## Additional warnings

It is almost always a bad idea to test floating point numbers for equality, the `-Wfloat-equal` will warn you about this.

Although not necessarily a problem, shadowing outer scope variable declarations may lead to confusion.  The `-Wshadow` compiler flag will alert you to this.

A warning can be issued when a `switch` statement is based on named `enum` values, but not all of those are cases in the `switch`.  To enable, compile with `-Wswitch-enum`.

When you cast away a `const` qualifier from a pointer, you may inadvertently modify a value you really shouldn't.  The compiler can check for this if you add the `-Wcast-qual` flag.

For C code, the options `-Wbad-function-cast` will warn when the result of a function call is cast to an inappropriate type.

Although some conversions from a larger to a smaller type are legal, they may be unintended, use the `-Wconversion` flag to be warned about these.

The preprocessor can also issue warnings when required.  `-Wundef` will warn when a macro variable that is used has not been assigned a value.  `-Wunused-macros` will warn about macros that were defined in the main file, but never used.

If-statements with many many branches are generally hard to read, and it is easy to make mistakes by repeating the same condition twice, or even duplicate code in branches.  This typically results from copy/pasting code fragments.  The options that help you detect such issues are `-Wduplicated-cond` and `-Wduplicated-branches` respectively.


Although the option `-Wmaybe-uninitialized` is activated by `-Wall` it is worth noting that it is only enabled when the compiler also optimises the code, i.e., at least at `-O1`.  


## C++ specific warnings

Specifically for C++, the `-Wnon-virtual-dtor` option is very useful as a class derived from a virtual base class that has no virtual destructor may lead to memory leaks.

The `-Woverloaded-virtual` option will warn you when a virtual function in a base class is overloaded, rather than overridden in the derived class, which may not have been your intention.

C++ has a safer way of performing type cast than C using `static_cast`, `dynamic_cast` and so on.  It is good practice to use appropriate C++ style casting, rather than C h style casts.  Warnings can be generated using `-Wold-style-cast`.
