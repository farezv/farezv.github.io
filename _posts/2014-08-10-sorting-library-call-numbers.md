---
layout: post
title: Sorting library call numbers
date: 2014-08-10 18:17
author: farezv
comments: true
categories: [programming, side projects, slcn]

---
So I worked at a library for a couple of years and I started thinking about the computer systems that assign call numbers to library books and the amount of effort it would take to sort through a six million volume collection (<a href="http://en.wikipedia.org/wiki/University_of_British_Columbia_Library" target="_blank">Wikipedia</a>).

<img src="http://farezca.files.wordpress.com/2014/03/slcn-1.png">
`Walter C. Koerner Library, aka a large book upside down`

A few months ago, my supervisor (at the aforementioned library job) asked me if there was a program he could use to train new library student workers to sort these call numbers. He showed me a commercial product he was looking at (which costs a lot of money) and was curious if I could build something like that, or similar. He's essentially trying to reduce shelving mistakes made by new student workers so books don't end up on the wrong spots, which is every librarian's nightmare.

These quizzes would not be six million long of course. We're looking at something along the lines of 20 call numbers per quiz, so even a brute force approach to solving this problem, in this case, would be pretty efficient.

These call numbers look something like this (unsorted list)

<ul>
    <li>QA76.5 M118 2000</li>
    <li>A100 B23 1991</li>
    <li>QA76.5 M2 1998</li>
    <li>PC200 A10 B12 2012</li>
</ul>

Here's the sorted list

<ul>
    <li>A100 B23 1991</li>
    <li>PC200 A10 B12 2012</li>
    <li>QA76.5 M118 2000</li>
    <li>QA76.5 M2 1998</li>
</ul>

You always look at the alphabet first, followed by the numeral. If the first level matches (QA76.5), you move on to the next level. Now here's the tricky part. You might think that <strong>M118</strong> should come after the <strong>M2</strong> call number since 118 &gt; 2, however, call number numerals after the first level are always read as if they're less than 1 (unless it's the year of publication, which is usually the last line). So M2 is read <em>M point two </em>and M118 is <em>M point one one eight</em>. This makes the M118 call number smaller than the M2 call number and makes the former arrive "alphabetically earlier."

<a href="http://ubyssey.ca/wp-content/uploads/2013/01/CUP_BookStacks_KaiJacobson.jpg"><img class="" src="http://ubyssey.ca/wp-content/uploads/2013/01/CUP_BookStacks_KaiJacobson.jpg" alt="" width="1600" height="1067" /></a> 
`A range at Irving K. Barber Library`

So here's the problem. This program needs to solve the quiz, accept a user solution and then compare the two solutions to see if they match. How do you make a computer understand this complicated call number sorting policy?

Here are two approaches I considered to model call numbers.

<h3>Sorting Raw Strings</h3>

There are plenty of ways to sort strings in various programming languages but most of them don't completely satisfy the requirements I've presented above. How do I ensure normal sorting for the first and last (year of publication) levels of the call number strings but then enforce the strange <em>numerals-in-the-middle-are-considered-decimals</em> sorting for the rest of it?

Not to mention I have to compare levels of these call numbers with each other. Breaking down these "levels", sorting the whole strings based on parts of them could work. For instance, I could parse "A100 TA2 2006" as a set of strings ["A", "100", "TA", "2", "1991"].

This method would work even better if the input strings were provided with proper white space. Like this,

<ul>
    <li>A 100 TA 2 1991</li>
    <li>A 100 TA 2 2006</li>
    <li>A 2 TB 4 1989</li>
</ul>

As you may be able to tell, this makes the user experience tedious because it's hard to read and type call numbers with spaces after every alphabet or numeral group.

<h3>Call Numbers as "Objects"</h3>

Another approach was to create a special "Call Number" object that has fields (attributes) such that each corresponds to a level. The problem with this approach lies in the diversity in call number levels. For instance,

<ul>
    <li>A100 B23 1991 (3 levels)</li>
    <li>PC200 A10 B12 2012 (4 levels)</li>
</ul>

This route required creation of a hierarchy of call number objects starting with a base of at least 3 and then sub-classing 4 and 5 level call number objects. While translating the input, with call numbers written in a "A1 B2 C3" format (one CN/line), I could create a new call number object based on the number of white spaces in the string (<em>n</em> white spaces is a <em>n+1</em> level CN).

The problem with this approach is that it leaves the program less robust for a future where there might be more than 5 level call numbers. It also doesn't make much sense to subclass based on a static number of fields that only increase by one.

<h3>Array of Levels</h3>

I finally decided to extend the Button class from Google Web Toolkit (the framework I'm using to build this app) and have with an array of strings with each element corresponding to a level. This array could be populated easily by a regular expression call, like `string.split("\s+")`

```java
public class CallNumberButton extends Button {

    // CallNumber broken into strings. For instance, "A100 TA2 2006" is ["A100" "TA2" "2006"]
	private String[] levels;
	private int rank;

	/* Constructor sets title and fills up the levels array
	 * */
	public CallNumberButton(String string) {

		this.setTitle(string);
		levels = string.split("\\s+"); // splitting by whitespace
		rank = -1;
	}
}
```

This gives me a very easy way to keep track of which call number level I'm dealing with, since all I have to do is check if the index is 0 or `levels.length - 1`

<h3>Todo</h3>

Here's what the app looks like so far. It definitely needs some UI improvements.

<a href="http://farezca.files.wordpress.com/2014/08/sorting-library-call-numbers-april-2014.png"><img class="aligncenter wp-image-1463 size-large" src="http://farezca.files.wordpress.com/2014/08/sorting-library-call-numbers-april-2014.png?w=1017" alt="" width="1017" height="1024" /></a>

I have its core functionality completed, with the "automatic quiz generation" still pending. There are a couple of approaches to this as well.

<h3>Purely random string generation</h3>

It's hard to generate a "real looking" call number quiz based on random parameters. I have tried writing a small snippet of code that randomly picks 3-5 level call numbers with 1-3 alphabet characters per "block" of call number (like A100 or AB20 or ABC3) followed by a random year within the last thousand years for the publication date. This approach shows some promise but it just doesn't feel good (from both a user and developer's standpoint) because they're made up call numbers.

<h3>Storing and randomly retrieving</h3>

I've tried building a small database using Google's app engine datastore by storing the strings that are input in the manual quiz creation (the main functionality my supervisor wanted). The problem with trying to randomly generate these quizzes has to do with the way Google's AppEngine backend stores its data. They're stored in a distributed manner and querying this datastore would require adding additional information (such as a Call Number ID) and then querying based on a range of call number IDs (as <a href="http://stackoverflow.com/questions/3002999/fetching-a-random-record-from-the-google-app-engine-datastore" target="_blank">this stackoverflow post</a> suggests).

<h3>UI Improvements</h3>

The app looks very basic right now and that's mostly due to me prioritizing functionality over form (and not working on it for a couple of months, shh). I've looked into ways to make it look nicer and I will likely incorporate the <a href="http://gwtbootstrap.github.io" target="_blank">gwtbootstrap</a> library. The immediate problem I'm facing there is that I need to use UIBinder for gwtbootstrap because it extends existing GWT buttons and widgets and doesn't provide any programmatic ways to incorporate those widgets into an existing project.

I should have used UIBinder right from the beginning because it provides a more favourable (aka <em>markup</em> based) approach to building user interfaces (much like XAML does for WPF applications) as opposed to what I was taught during my Software Engineering classes (writing widgets and buttons directly in the Java source code, which sounds like a horrible idea in hindsight).

I will likely prioritize UI improvements over the aforementioned random quiz generation problem.
