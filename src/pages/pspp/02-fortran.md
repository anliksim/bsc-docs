---
path: "/pspp/fortran"
title: "PSPP Fortran"
date: "2019-12-08"
draft: false
author: Simon Anliker
tags:
  - pspp
  - en
  - fortran
---

# Fortran

**FOR**mula **TRAN**slation

Developed at IBM (John Backus)

Development cycle: Write, compile, link, execute, test/debug, change

## Fortran I

* Names up to six chars
* Post-test counting loop (do)
* Formatted I/O
* User-defined subprograms
* Arithmetic IF (3-way - IF negative, zero, positive)
* No data typing - i,j,k,l,m,n prefixed are int, others float
* Very fast for that era

## Evolution

* Before 90, it required a fix format (historically due to the use of punch cards)
  * Line 1-5: Label (C indicates comment line)
  * Line 6: Any char => continuation line
  * Line 7-72: Statements
  * Line 73-80: Optional Line #s
* Compiled language
* Before 90, memory is allocated statically at compile time 
* Before 90, no recursion support
* Before 90, all in capital
* Since 90, free format allowed, ! for comments

## Structure

* Program units
  * Program
  * Function
  * Subroutine
  * Block data
  
Program units may contain internal functions and subroutines.

* Variables are typed (nowadays)
  * integer: `integer :: value`
  * real/double precision: `real :: value`
  * complex: `complex :: value`
  * character: `character*20 :: value` `value = 'abcd'`
  * logical: `logical :: err` `err = .false.`
* Case insensitive (var is same as VAR)
* Arrays: `integer :: a` `dimension a(10)`
  * Shortcut: `real :: a(10, 10, 10)`
  * Linear allocation of memory
  * Up to 7 dimensions
  * Index starting at 1
* Constants (parameter statement)
  * Old: `PARAMETER (MAX=20)`
  * New: `integer, parameter :: max=20`

* Control statements
  * Branching (goto)
  * Comparison (if)
  * Looping (do)
  * Subroutine invocation (call)
  * Do-nothing (continue)


**Implicit typing**: If variable is not declared when used, then it is created at compile time based on first character. Starting from 77, `implicit none` can be used to require explicit typing (good practice).


### Subroutine

* Internal or external
* No return value
* All arguments are passed by reference

```
    subroutine mult(x,y,result)
    [...]
    end
```
```
    call mult(2,3,result)
    ! result contains 6
```

### Function

* Internal or external
* Has return value

```
    function mult(x,y)
    [...]
    end
```
```
    result = mult(2,3)
    ! result contains 6
```

### Block Data

* Named shared memory areas

```
    pogramm a
    common /myblock/ a,b,c

    programm b
    common /myblock/ a,b,c
```

### Branching

* Computed goto can be used to create switch statements

```
    if (i .eq. 0) go to 100
    [...]
100 a = 1
    [...]
200 a = 2
```

### I/O

* Formatted or unformatted/binary
* Sequential or random


### Parallel programming

* Support via OMP











