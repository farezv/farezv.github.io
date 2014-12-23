---
layout: post
title: Styling priorities in Android
date: 2014-12-22 16:08
author: farezv
comments: true
categories: [programming, android]

---

While working on my Android app, I came across a strange styling problem. My app displays a few lines of text vertically in a `LinearLayout` as part of a search result & the results activity is a `ListView` of these "Book Results." Here's what it looks like.

`Misaligned corners!`
![](https://s3-us-west-2.amazonaws.com/farezcablog/img/bookResults_v0.2ListViewProblem.png)

Here's the XML code for the ListView

    <ListView android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#FFFFFF"
        android:layout_weight="1"
        android:drawSelectorOnTop="false"
        style="@style/ListView"/>
          
And here's the style rule

    <style name="ListView">
        <item name="android:background">#E6E6E6</item>
    </style>
    
The problem was that I completely missed the `android:background` already set as `#FFFFFF` in the `ListView` XML code and turns out that code will overwrite the style sheet (as I discovered by accident).

So I fixed it by changing the desired line to the color I wanted right in the XML code (just for this case, since I only have a single style rule; if I had multple style rules, I'd put them all in `styles.xml`).

Here's what it looks like now. I'm still looking for ways to have the area inside the border be of a white background but that requires messing around with adapter code & changing each `ListView` row's color.

![](https://s3-us-west-2.amazonaws.com/farezcablog/img/bookResults_v0.2.png)