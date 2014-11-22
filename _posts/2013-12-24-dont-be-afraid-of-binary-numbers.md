---
layout: post
title: Don't be afraid of binary numbers
date: 2013-12-24 10:01
author: farezv
comments: true
categories: [Boolean algebra, computer science]
---
I often find my close friends cringe comically when I start talking about numbers in binary, and rightfully so, considering how complicated and mystical those zeros and ones look to a non computer science person.

At the end of the day, numbers are simply an abstract concept and we've chosen decimal, the anatomically convenient way of representing numbers as the "right" way. I'm sure you were tricked into believing you had 11 fingers at family gatherings when you were younger. Those grandparents are crafty.

Let me begin with something really simple. Consider a light bulb, it exists in two states. Off and on. Zero and one. False and true. That's the basis of boolean algebra. The light bulb, is a single bit of information. It can only represent two numbers with a single bit. When I add another bit, I can represent up to 4 numbers (from 0 to 3).
<ul>
	<li>00 is zero</li>
	<li>01 is one</li>
	<li>10 is two</li>
	<li>11 is three</li>
</ul>
Binary is very simple in this sense, and that's why we need multiple bits to represent the vast range of numbers. And no, 64 bit numbers are not two 32 bit numbers put together. Don't let the shady dude at the Big Box retail store sell you something on this false pretense. For instance, adding another bit lets us represent twice as many numbers. So 33 bits let us represent twice as many numbers as 32 bits. Let's add another bit to our example above.
<ul>
	<li>000 is zero</li>
	<li>001 is one</li>
	<li>010 is two</li>
	<li>011 is three</li>
	<li>100 is four</li>
	<li>101 is five</li>
	<li>110 is six</li>
	<li>111 is seven</li>
</ul>
Notice how the 1 moves a single spot left starting from one, to two, to four. So if you have a single bit, moving it one to the left (in this particular representation) doubles the decimal value of the number. If you're wondering why that is, let's take a look at how these numbers are interpreted. The rightmost bit is the least significant and because binary operates on a base of 2 (<em>bi</em> means 2 whereas <em>dec</em> in decimal indicates base 10), each bit holds the value of 2<sup>n</sup> where n is the bit number. Therefore, the rightmost bit holds 2<sup>0</sup> which equals 1. The next bit holds 2<sup>1</sup> which equals 2.

`00000000000000000000000000000000`

`2`<sup>`31`</sup>  `<-----------------------------------------------`  `2`<sup>`0`</sup>

Consider 011. This binary number has the bits 2<sup>0</sup> and 2<sup>1</sup> "turned on." Therefore, we add 1 and 2 to get the decimal value 3. Similarly, the number seven is derived from 2<sup>2</sup> + 2<sup>1</sup> + 2<sup>0 </sup>because bits 0, 1 and 2 are turned on in 111. Therefore, the decimal value of a binary number is computed by adding the values held by "one" bits.

In this way, you can store up to 4,294,967,296 numbers in 32 bits (bit 0 to 31). This is why 32-bit computer systems can only recognize/deal with up to 4 gigabytes worth of memory. This is because they can only read up to (a little over) 4 billion memory (RAM) addresses. So for programs that require more than 4 gigs of RAM, you need to use a 64-bit computer system. You can store up to 2<sup>64</sup> numbers in 64 bits. That's a lot of memory addresses!
