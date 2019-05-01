<!--
.. title: Design Patterns
.. slug: design-patterns
.. date: 2008-06-11 03:19:59
.. tags: c++,design patterns,gamma,gang of four,helm,johnson,object-oriented design,object-oriented programming,oo design,oo programming,programming,vlissides
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>I'm reading <a href="http://www.amazon.com/Design-Patterns-Object-Oriented-Addison-Wesley-Professional/dp/0201633612/">this amazing book</a> on object-oriented design:

<a href="http://www.amazon.com/Design-Patterns-Object-Oriented-Addison-Wesley-Professional/dp/0201633612/"><img class="aligncenter" src="http://ecx.images-amazon.com/images/I/51%2BYHTzhheL._SL500_BO2,204,203,200_PIsitb-dp-500-arrow,TopRight,45,-64_OU01_AA240_SH20_.jpg" alt=""></a>

Now, it's not like I taught myself programming and am only now beginning my formal education. I effectively completed a minor in computer science at the University of Melbourne, taking a substantial number of CS subjects every semester for three years. But I feel like a total n00b going through this book. For all the wonderful theory I'd been taught about algorithm complexity, and all the disparate programming languages (Haskell, C, Prolog, Assembly, and others), I don't remember once hearing about object-oriented (OO) programming during my undergrad at UniMelb.

<em>[Ed's Note: a fellow UniMelb alumnus told me it sounded like I was attacking the CS program here. That was not at all my intention: all the CS subjects I took were electives, so I cherry-picked whichever ones I wanted to do based on whether they sounded interesting, not whether they were a core of the program. For all I know, Melbourne Uni's CS department might have the best OO programming course in the country. What's more, at least I <strong>was</strong> taught about modular, well-documented code with informative variable names, which is more than I can say for the majority of coders I have encountered. I merely wanted to illustrate that it's possible to be exposed to a wide range of Computer Science and yet not know about OO programming.]</em>

Once I started writing my own code, however, I began using Marshall Cline's <a href="http://www.parashift.com/c++-faq-lite/">C++ FAQ Lite</a>, a wonderful resource for C++ code tips. Cline focuses more on implementation than on good OO design, but every now and then his answer to an FAQ was puzzling without prior knowledge of OO programming. <a href="http://www.parashift.com/c++-faq-lite/big-picture.html#faq-6.9">For example</a>:
</p><blockquote>
<div class="FaqTitle">
<h3>[6.9] Are <tt>virtual</tt> functions (dynamic binding) central to OO/C++?</h3>
</div>
<em>Yes!</em>

Without <a title="[20] Inheritance -- virtual functions" href="http://www.parashift.com/c++-faq-lite/virtual-functions.html">virtual functions<!--rawtext:[20]:rawtext--></a>, C++ wouldn't be object-oriented. [...]</blockquote>
Reading these, I would think to myself, "Hmm, I should look into that," and be on my merry way with my (inflexible, error-prone) program. Eventually, I stumbled upon <a href="http://www.parashift.com/c++-faq-lite/how-to-learn-cpp.html#faq-28.8">a question <em>about</em> learning OO design</a>, in which Cline calls Design Patterns "must-read."

The purpose of this post is simply to agree wholeheartedly with Mr. Cline. Design Patterns is essentially a catalog of the most successful designs in OO programming—those that have stood the test of time and yielded the most flexible, adaptable, reusable code in the world. Anybody who thinks that they can come up with something better just off-the-cuff, from their own intuition, is most likely arrogant and delusional. I've only covered two patterns so far (Singleton and Composite, for the curious), and I am awed by their ingenuity but also by their simplicity. These patterns elegantly solve problems that every programmer faces at one point or another. What's more, people who have read the book can more effectively communicate about their designs by explicitly naming their components and how they interrelate.

Bottom line: if you do any programming at all, buy and read this book.

You can thank me later.</body></html>
