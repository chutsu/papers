- Survey GPU and GP


In computer sciencewe are used to the trade offs between compiling and
interpreting code. This is because we expect the sum of the run times of the
program to exceed the compilation time. However GP programs tend to be
lightweight and possibly run few times, so it is common to use a purpose built
interpreter.

One way to reduce compilation overhead is to convert whole population into a
single program and compile it. The complete population program contains
(non-evolved) subcode to run each of its component individuals and record their
fitness. By running the compiler only once, rather than once per member of the
population, considerable savings are made and typically the compiled code runs
much faster than interpreting it.

It turns out that the Nvidia nvcc CUDA compiler is particularly slow and so
interpreting usually wins hands down. However Harding and Banzhaf decribe a way
of reducing the compilation  overhead by running the compiler in parallel
across a cluster of workstations.
