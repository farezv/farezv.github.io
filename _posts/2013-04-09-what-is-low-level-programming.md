---
layout: post
title: What is low-level programming?
date: 2013-04-09 14:23
author: farezv
comments: true
categories: [Assembly language, Machine code, programming]
---
So in order to understand what low-level programming is, one must know about the language stack. To put it briefly, computers don't understand high level languages like Java, C++, C# etc. Processors understand bits which exist in two states; 0 and 1. There are multiple software mechanisms (aka compilers) that convert high level languages to machine code and then to zeros and ones.

Somewhere in the depths of bits and switches, low-level code looks like this (credit to CPSC 213 at UBC). We have instructions that perform small steps like load (read), store (write) and jump/branch (functions, if statements, and loops) using registers which are essentially temporary storage devices for the processor. Load and store operations also use memory directly, so they're incredibly fast, but also very dangerous if used incorrectly. Consider the following snippet:

```
ld $5 r0        // loading the value 5 in register 1
ld $4 r1
ld $3 r2
not r1          // r1 = - r1 = -4
add r0 r1       // r1 = r0 + r1 = 1
bgt r1 b1       // jump to label b1 if r1 &gt; 0
else: add r2 r0 // execution of this instruction depends on conditional branch
st  r0          // r0 = 8
b1: st  r0      // r0 = 5
```

The first three lines simply load values into registers. Performing subtraction requires 4 steps (lines 1,2,4 and 5) and a comparison requires 5. Keep in mind, we could easily have a pointer to a variable defined earlier (via it's memory address) in line 1 where we load 5 into register 1. "Else" and "b1" are labels, they don't mean anything. Assembly programmers use labels to identify conditional statements and branches.

It's important to realize that the values in these registers are arbitrary. I'm trying to show how small each operation is. Every instruction performs a very small task. While a higher level language may simply compare the value of a variable to zero in order to perform a conditional task, low-level languages have to go through multiple steps to carry out the same task. This is part of the translation process from a high level language, to machine code, something the processor understands.

The processor specification (set of rules, protocols etc.) defines what code each of those operations gets. Imagine machine code defined by 4 digits. The first identifies the type of instruction. Let arithmetic operations have a machine code of 5, the "add" operation can be defined as a 2, and the last two are register numbers. Line <code>add r0 r1</code> can simply be represented as 5201. Processors can therefore, understand what operation to perform using which registers via this simple machine code.

Low level programming is quite powerful due to the various opportunities to optimize performance, such as reusing register values. Perhaps we need register 2's value somewhere later in our program so we don't reassign it. Assembly languages and processor specs will also define rules to optimize performance, such as using certain registers for the same purpose all the time so we don't end up performing unnecessary reads and writes (which slows things down).
