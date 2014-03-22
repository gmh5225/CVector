
CVECTOR
=======

This is a relatively simple ANSI compliant C vector library with specific structures and
functions for int's, double's and string's and support for all other types
using a generic structure where the type is passed in as void* and stored in a byte array
(to avoid dereferencing void* warnings and frequent casting) .
The generic vector is very flexible and allows you to provide free and init functions 
if you like that it will call at appropriate times similar to the way C++ containers
will call destructors.

Other modifiable parameters are at the top of vector.c
<pre>
size_t VEC_I_START_SZ = 50;
size_t VEC_D_START_SZ = 50;
size_t VEC_VOID_START_SZ = 20;
size_t VEC_STR_START_SZ = 20;

#define VEC_I_ALLOCATOR(x) ((x) * 2)
#define VEC_D_ALLOCATOR(x) ((x) * 2)
#define VEC_STR_ALLOCATOR(x) ((x) * 2)
#define VEC_VOID_ALLOCATOR(x) ((x) * 2)
</pre>
The allocator macros are used in all functions that increase the size by 1.
In others (constructors, insert_array, reserve) VEC_X_START_SZ is the amount
extra allocated.


There are also 2 templates, one for basic types and one for types that contain
dynamically allocated memory and you might want a free and/or init function.
In other words the first template is based off vector_i and the second is based
off of vector_void, so look at the corresponding documentation for behavior.

They are located in vector_template.c/h and vector_template2.c/h respectively.

To generate your own c and h files for a type just run:
<pre>
python3 generate_code.py yourtype
</pre>

which will generate vector_yourtype*.c and vector_yourtype*.h
It generates the results for both templates so just delete the pair
you don't want.

vector_short and vector_f_struct are examples of the process and
how to add it to the testing.


Building
========
I use premake so the command on linux is premake4 gmake which
will generate a build directory.  cd into that and run make
or make config=release.  I have not tried it on windows though
it should work (well I'm not sure about CUnit ...).

There is no output of any kind, no errors or warnings.


It has been relatively well tested using Cunit tests which all pass.
I've also run it under valgrind and there are no memory leaks.

<pre>
valgrind --leak-check=yes ./vector
 
==17650== 
==17650== HEAP SUMMARY:
==17650==     in use at exit: 0 bytes in 0 blocks
==17650==   total heap usage: 5,146 allocs, 5,146 frees, 936,924 bytes allocated
==17650== 
==17650== All heap blocks were freed -- no leaks are possible
==17650== 
==17650== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 2 from 2)
</pre>




I plan to continue to improve/modify it but probably only in minor ways and
only if I think something should be changed.  This library is mostly
for my own use but I thought I would share it and use it as a platform
to learn TDD, CUnit and Doxygen.


You can probably get Cunit from your package manager but
if you want to get the most up to date version of CUnit go here:

http://cunit.sourceforge.net/index.html
http://sourceforge.net/projects/cunit/

I used version 2.1-2.



To actually use the library just copy the appropriate c/h file pair(s) to your project
or just use cvector.h.
To get a good idea of how to use the library and see it in action and how it should
behave, look at vector_tests.c


CVector is licensed under the MIT License.  See LICENSE