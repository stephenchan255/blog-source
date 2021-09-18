---
title: Cpp
date: 2020-09-04 20:23:20
tags: [IT, Programming, Cpp]
categories: [IT, Programming, Cpp]
---

Read progress: finish read 1.6, 1.7&Exercise skipped 2.9 Exercise skipped
org next: ch 3.2 Iterator

# Header file (& Namespace) & Program text file 
ref: p11, p54 param default value, 57 inline func, 63 set up a header file

**header file**: declaration of obj/func (can be mtp dec [p63]) + def of an inline func
- included in each program text file that wish to use the func/obj
- Set up a Header File [2.9]
	- without `extern`, obj dec in header file interpreted as the def 
	- `const` obj no need `extern`: only visible inside the file it defined
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

# Arrays, Vectors & List
```c++
int elem_seq[seq_size] = { ... };
int elem_seq[] = { ... }; // let the compiler calculate the array size

vector<int> elem_seq(seq_size);
elem_seq[0] = 1 ; // not support an explicit initialization list

vector<string> svec; // an empty vector of string elem

// alternative is to init...
vector<int> elem_seq(elem_vals, elem_vals+seq_size); // --- 3.1
elem_seq.size();

list<int> ilist(ia, ia+asize); // --- 3.2
```

# Pointers
- evaluation of an object's name: evaluates to its associated value
- address-of op(`&`), deref `*` (null pointer: `pi = 0`)

```c++
fibonacci.empty() // vector<int> fibonacci
if (pv && ! pv->empty() && ((*pv)[1] == 1))
```

# Function

Invoking a Func
- **program stack**: (with memory to) hold the value of each function parameter & local objects
	- function completes --- this area of memory is discarded (popped from the program stack)
- pass by value -> pass by reference

## Pass by Ref
```c++
// ref
int ival = 1024; // an object of type int
int *pi = &ival; // a pointer to an object of type int
int &rval = ival; // a reference to an object of type int 

int jval = 4096;
rval = ival; // 此处应该为 rval = jval: 将ival的值从1024修改为4096
pi = &rval; // 4096

// pass by ptr | ref
void display(const vector<int> *vec) // a pointer may or may not actually address an obj
void display(const vector<int> &vec) // A reference always refers to some obj
```

- A function can return only one value --- second parameter of type ref
	- example: final implementation of `fibon_elem()`...
- reference cannot be reassigned to refer to another obj...manipulation of a reference acts on the object
- declare a parameter as a reference...modify directly the actual object being passed...eliminate copying a large obj
- recommend not passing built-in types by ref

## Default Param Values
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

# Containers
- Standard Template Library (STL), (seq | assoc) container classes, generic algos ...
- container ops... [3.3]
	- `begin()/.end()` (e.g. vector, list) [p71]
	- `size()`, `empty()`, `clear()`
- Sequential Containers: `vector`, `list`, `deque` [3.4]
	- `push_/pop_back()`, `push_/pop_front()` --- `insert()/erase()`
	- `front/back()`
	- 5 ways to define a sequential container obj...
- Generic Algos & `#include <algorithm>`... [3.5]
	- Four possible generic search algorithms...
	- `max_element()`, `copy()`, `sort()`

## Pointer Arithmetic
```c++
// array: (passed) as a pointer to its first elem, can still indexing as bf
int min(int array[24]) { ... } // == (int *array)
array[2] == *(array + 2) // // subscripting is carried out by...
```
- Pointer arithmetic presumes that the elements are contiguous (in a contiguous area of memory, e.g. arr/vector)
- `find()` example [p66~70]

## Pointers -> Iterators
```c++
vector<string> svec;
vector<string>::iterator iter = svec.begin()/.end();  
```
- if vec empty, `svec.begin()/.end()` are equal
- double colon [`::`]: nested type
- iterator sam syntax as pointer, e.g. deref by `*iter`, `iter->size()`

const vector & const iterator
```c++
const vector<string> cs_vec; 
vector<string>::const_iterator iter = cs_vec.begin()/.end();
```

reimplementation of `display()`, `find()` ...

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
- bubble sort
- terminate the program: `exit(-1);` & `#include <cstdlib>`
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
- template class
- element indexing `[]` & **off-by-one** err
- function prototype [p39]
- ineluctable modality of the computer & `#include <limits>; int max_int = numeric_limits<int>::max()`
- forward declaration
- randomize: `rand()` and `srand()` functions: `#include <cstdlib>`