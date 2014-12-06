---
layout: post
title: Berlin Clock
---

Recently I was driving to work and I was listening to an [NPR news story](http://www.npr.org/2010/11/22/131520768/-kryptos-sculptor-drops-new-clue-in-20-year-mystery) about a new clue for the [Kryptos](http://en.wikipedia.org/wiki/Kryptos) sculpture on the grounds of the CIA:

[![](//upload.wikimedia.org/wikipedia/commons/e/e0/Kryptos_sculptor.jpg)](http://commons.wikimedia.org/wiki/File%3AKryptos_sculptor.jpg)
<cite>[Kryptos sculptor](http://commons.wikimedia.org/wiki/File%3AKryptos_sculptor.jpg) [CC-BY-SA-3.0](http://creativecommons.org/licenses/by-sa/3.0), by Jim Sanborn (Jim Sanborn), from Wikimedia Commons.</cite>

[The clue?](http://www.wired.com/2014/11/second-kryptos-clue/)

> The 12-foot-high, verdigrised copper, granite and wood sculpture on the grounds of the CIA complex in Langley, Virginia, contains four encrypted messages carved out of the metal, three of which were solved years ago. The fourth is composed of just 97 letters, but its brevity belies its strength. Even the NSA, whose master crackers were the first to decipher other parts of the work, gave up on cracking it long ago. So four years ago, concerned that he might not live to see the mystery of Kryptos resolved, Sanborn released a clue to help things along, revealing that six of the last 97 letters when decrypted spell the word “Berlin”—a revelation that many took to be a reference to the Berlin Wall.

> To that clue today, he’s adding the next word in the sequence—“clock”—that may or may not throw a wrench in this theory. Now the Kryptos sleuths just have to unscramble the remaining 86 characters to find out.

###What is a Berlin Clock?

Turns out, a Berlin Clock (or a Set Theory time clock), is a pretty cool way to visually represent time:

[![Mengenlehreuhr.jpg](http://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Mengenlehreuhr.jpg/429px-Mengenlehreuhr.jpg)](http://commons.wikimedia.org/wiki/File:Mengenlehreuhr.jpg#mediaviewer/File:Mengenlehreuhr.jpg)
<cite>[Mengenlehreuhr](http://commons.wikimedia.org/wiki/File:Mengenlehreuhr.jpg#mediaviewer/File:Mengenlehreuhr.jpg)" by [Muritatis](//commons.wikimedia.org/wiki/User:Muritatis "User:Muritatis") - Own work. Licensed under Public domain via [Wikimedia Commons](//commons.wikimedia.org/wiki/).</cite>

> Telling the time based on the "set theory principle", the Mengenlehreuhr consists of 24 lights which are divided into one circular blinking yellow light on top to denote the seconds, two top rows denoting the hours and two bottom rows denoting the minutes.

> The clock is read from the top row to the bottom. The top row of four red fields denote five full hours each, alongside the second row, also of four red fields, which denote one full hour each, displaying the hour value in 24-hour format. The third row consists of eleven yellow-and-red fields, which denote five full minutes each (the red ones also denoting 15, 30 and 45 minutes past), and the bottom row has another four yellow fields, which mark one full minute each. The round yellow light on top blinks to denote even- (when lit) or odd-numbered (when unlit) seconds.

###That would look pretty cool in a browser...

I clearly needed to make one, so I opened up my text editor and thought about how I wanted to do this.

The clock has 5 types of lamps:

* 1 second x 1
* 5 hour x 4
* 1 hour x 4
* 5 minute x 11
* 1 minute x 4

I decided it would be easiest to represent these lamps as HTML elements and then treat each set as a collection. Each collection would then be controlled by some JavaScript that adds or removes CSS classes to light or dim the lamps.

To identify each individual lamp, I decided to apply a naming convention that prefixed the ID with the collection (i.e. "h5" for five hour lamps) followed by the index of the lamp (seeded at 1 instead of zero for no particular reason):

{% highlight html %}
<div class="lamp-row">
  <div id="h5-1" class="rectangle-large red off"></div>
  <div id="h5-2" class="rectangle-large red off"></div>
  <div id="h5-3" class="rectangle-large red off"></div>
  <div id="h5-4" class="rectangle-large red off"></div>
</div>
{% endhighlight %}

Then, to tell the time, I wrote a function in JavaScript that calculated how many sets of five hour blocks had occurred. Below is an example of the idea I used for each of the sets. I simply take the number of hours for the current time, divide by five, and round down to the lowest whole number. Then, if it is more than zero, I just use a for loop to turn on the correct number of lamps:

{% highlight javascript %}
var clockDate = new Date(),
    clockHours = clockDate.getHours(),
    fiveHours = Math.floor(clockHours / 5);

if (fiveHours > 0) {
		for (var i = 1; i <= fiveHours; i++) {
				$("#h5-" + i).removeClass("off");
		}
}
{% endhighlight %}

I apply the same logic to the other sets, and this is what you get:

A [Berlin Clock](http://3mpire.github.io/berlin-clock/) in your browser!

###What's next?###

This was cool, but it can be better. I would like to make a jQuery plugin so that it's easy to add it to any site. I am also thinking about adding some chimes. I'm sure I could spend some time refactoring the JS to be more refined.

However, I have bigger plans:

![Alt text](http://pbs.twimg.com/media/B23D-P8IAAAAqN3.jpg:medium "Arduino powering 24 LEDs")
<cite>My Arduino powering 16 LEDs using two 74HC595 shift registers.</cite>

The past several weeks I have been getting into the fun world of Arduino projects. The photo above was taken just a few days before I wrote the browser-based clock in this post. If you look closely, you will see that there are 16 LEDs on that breadboard controlled by two 74HC595 shift registers. There is a third unused shift register as well. This means I can control up to 24 LEDs using just a few pins off the Arduino controller.

Coincidentally, there are 24 lamps in the Berlin Clock.

Stay tuned!