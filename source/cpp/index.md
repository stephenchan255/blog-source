---
title: Cpp
date: 2020-09-04 20:23:20
tags: [IT, Programming, cpp]
categories: [IT, Programming, cpp]
---

Read progress: finish read 1.6, 1.7&Exercise skipped 2.9 Exercise skipped
org next: ch 3 Generic Programming

# Head file (& Namespace) & Program text file 
ref: p11, p54 param default value, 57 inline func, 63 set up a header file

**header file**: declaration of obj/func (can be mtp dec [p63]) + def of an inline func
- included in each program text file that wish to use the func/obj
- Set up a Header File [2.9]
	- without `extern`, obj dec in header file interpreted as the def 
	- `const` obj no need `extern`: only visible inside the file it defined [why?]
	- header file: standard vs user-provided (`.h` suffix & `" "` | `< >`)
**program text file** definition of a func (can only be one def [p63])
- func def need to accsss obj/func in other files? --- include header files 
- compiled once and is linked to...the using files 

```c++
#include <iostream>
#include <string>
using namespace std; // p14, cmt out: string not def [prac1.2]

string user_name;
cin >> user_name;
cout << "\nHello, " << user_name;
```

example: declaration and definition of `display()` 

# Arrays & Vectors
```c++
int elem_seq[seq_size] = { ... };
int elem_seq[] = { ... }; // let the compiler calculate the array size

vector<int> elem_seq(seq_size);
elem_seq[0] = 1 ; // not support an explicit initialization list

// alternative is to init...
vector<int> elem_seq(elem_vals, elem_vals+seq_size); // --- 3.1
elem_seq.size();
```

# Pointers
- evaluation of an object's name: evaluates to its associated value
- address-of op(`&`), deref `*` (null pointer: `pi = 0`)

```c++
fibonacci.empty() // vector<int> fibonacci
if (pv && ! pv->empty() && ((*pv)[1] == 1))
```

# Function

Write a Func
- terminate the program: `exit(-1);` & `#include <cstdlib>`
- A function can return only one value --- second parameter of type ref
- final implementation of `fibon_elem()`...

Invoking a Func
- bubble sort...
- **program stack**: (with memory to) hold the value of each function parameter & local objects
	- function completes --- this area of memory is discarded (popped from the program stack)
- pass by value -> pass by reference

## Pass by Ref
```c++
int ival = 1024; // an object of type int
int *pi = &ival; // a pointer to an object of type int
int &rval = ival; // a reference to an object of type int 

int jval = 4096;
rval = ival; // 此处应该为 rval = jval: 将ival的值从1024修改为4096
pi = &rval; // 4096

void display(const vector<int> *vec) // a pointer may or may not actually address an obj
void display(const vector<int> &vec) // A reference always refers to some obj
```
- reference cannot be reassigned to refer to another obj...manipulation of a reference acts on the object
- declare a parameter as a reference...modify directly the actual object being passed...eliminate copying a large obj
- recommend not passing built-in types by ref

## Default Parameter Values
```c++
void bubble_sort(vector<int> &vec, ofstream *ofil = 0) // place the default value in header file (greater visibility)
void display(const vector<int> &vec, ostream &os = cout) // a ref cannot be set to 0...always refer to some obj
```
- rules about providing default param...

## Obj Memory Management
- Returning the address of one of these local obj: run-time program errors (Returning elems by value is OK...)
- **extent** (time) & **scope** (region)...
- Local Static Obj --- `fibon_seq(int size)` example...
```c++
static vector<int> elems; // memory... persists across function invocations
...
return &elems; // can safely return elems's address
```
- dynamic extent & heap memory: `new` expr (allocated at run-time) & `delete` expr (memory leak if no `delete`)
```c++
int *pi;
pi = new int(1024);
int *pia = new int[24]; // pia: address the first element of this array
                        // array elems remain uninited (no syntax for init an array of elements allocated on the heap)
delete pi; // delete a single obj
delete [] pia; // delete an arr of objs
```

## Pointers to Functions
```c++
const vector<int>* (*seq_ptr)(int); // "seq_ptr" point to any func with return type "vector<int>*" & param "int"
seq_ptr = pell_seq; // assigns seq_ptr the address of pell_seq()

// seq_array is an array of pointers to functions
const vector<int>* (*seq_array[])(int) = { ... }
seq_ptr = seq_array[++seq_index];
```
- use enumerator as index

```c++
enum ns_type { ... } // By default, the first enumerator is assigned a value of 0...
seq_ptr = seq_array[ns_pell];
```

## Inline Function 
- inline function: **request** to the compiler to expand the function at each call point, 
	- prefixing its definition with the `inline`

## Template Functions
```c++
template <typename elemType, ...>
void display_message(const string &msg, const vector<elemType> &vec) 
```

# Generic Programming
- Standard Template Library (STL), sequential | associative container, generic algorithms ...

## The Arithmetic of Pointers
- array: passed...as a pointer to its first elem, can still indexing as bf
```c++
int min(int array[24]) { ... } // == (int *array)
array[2] == *(array + 2) // // subscripting is carried out by...
```
- `find()` example [p66 ~]
-  Pointer arithmetic presumes that the elements are contiguous
- arr/vector: elements in a contiguous area of memory
- `vector<string> svec;`: an empty vector of string elem
- [q] p69 `first != last;` === `first < last` ?

## Pointers -> Iterators
- Each container class provides a `begin()` op...and an `end()` op
- For the list class iterator, for example, the associated increment function...For the vector class iterator, the increment function...

```c++
vector<string> svec;
vector<string>::iterator iter = svec.begin()/.end();  // double colon [`::`]: nested type
*iter; // deref
iter->size()

vector<elemType>::const_iterator iter = vec.begin();
vector<elemType>::const_iterator end_it = vec.end(); 
// if vec is empty, iter and end_it are equal (both 0?)
```

- `vector<string>::const_iterator`...read the vector elements but not write to them
- reimplementation of `display()`, `find()`


# Other note
## Basics
How to Write a C++ Program
- `int main()`, `main` not a language keyword..., `return 0`...inserted auto

Defining and Initializing a Data Object
- `=` & constructor syntax for init [p16]

Expressions 
- integer division, conditional op (`?:`)
- conditional expr: 0 => `false`, non-0 => `true` [p19]
- arithmetic expr: `false` => `0`, `true` => `1` [p22]

Conditional & Loop Stmt
- `if` stmt: stmts with | without `{}` [p23], `if-else if-else` [p24], `switch`(`case` label: constant expr, `break`) [p25]
- `while`, `break`, `continue` [1.4.2]
- `for` loop [p29]

Function
- Function Overloading [2.6]

## Engineering principle
- trust is not a sound engineering principle
- better to communicate between functions using parameters rather than use objects defined at file scope
- to solve a communications problem between functions:
	- introduce an object at file scope: complicate the independence and understandability of individual functions
	- using parameters [p52]

## Miscellaneous
- fundamental data types...
	- character literal: printing & nonprinting... [p12]
	- three floating point size types `float` `double` `long double`
	- `double usr_score = 0.0;`, character type & escape seq, `const double pi = 3.14159;`
- `string word;`, `word.size()` [p27]
- template class
- element indexing `[]` & **off-by-one** err
- function prototype [p39]
- ineluctable modality of the computer...
- randomize: `rand()` and `srand()` functions: `#include <cstdlib>`
- `vector<int> elems;`...`elems.push_back(1);`