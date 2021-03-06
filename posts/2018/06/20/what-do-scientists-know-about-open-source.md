<!--
.. title: What do scientists know about open source?
.. slug: what-do-scientists-know-about-open-source
.. date: 2018-06-20 21:19:46
.. tags: open-source,Planet SciPy,science,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>A friend recently pointed out this great talk by Matt Bernius, <a href="https://community.redhat.com/blog/2018/05/what-college-students-know/">What students know and don't know about open source</a>. If you have even a minor interest in open source it's worth a watch, but the gist is: in the US alone, there are about 200,000 students enrolled in a computer science major. Open source communities are a great space to learn real-world programming, so why don't these numbers translate into massive contributions to open source?

At the core of the issue, Matt identifies two main problems: (1) colleges and universities simply don't teach open source, or even collaborative coding; and (2), many open source communities make newcomers feel unwelcome in a variety of ways.

I want to comment about this in the context of programming in science. That is, programming where the code is not the main product, but rather a useful tool to obtain a scientific result, for example in biology or physics. Here, we still see relatively little contribution to open source, for related but different cultural issues.

<!-- TEASER_END -->

I've sent my <a href="https://ilovesymposia.com/2015/12/26/why-scientists-should-code-in-the-open/">scientists should code in the open</a> post to a few people and the response from most remains sceptical. I hope this post will address some of their concerns.

</p><h2>Scientific culture is ridiculously secretive</h2>

The most common objection is to my assertion that people won't scoop you by looking at your code. I remember a tweet (that I sadly can't find now) that really got to the gist of the problem. It went something like this:

<blockquote>
  Someone in science having a new idea: "Ooh, I hope I don't get scooped!"<br>
  Someone in open source having a new idea: "Ooh, I hope someone has implemented this already!"
</blockquote>

<strong>Update:</strong> I found the source! It's <a href="https://twitter.com/tweetotaler/status/884412302098915329">this tweet</a> by Elizabeth Seiver.

This is a huge gap in culture that won't soon go away, but there are encouraging steps towards narrowing it. For example, PLOS Biology, a leading journal, recently <a href="http://twitter.com/PLOSBiology/status/958346565868978176">announced</a> that they would consider "scooped" studies for publication within six months of the "scooping". That goes some way towards re-aligning incentives towards collaborative and open science.

I've come across many collaborations that have started because of open source. I have not heard of someone getting scooped because of open source, but of course that sort of information would be hard to trace and come by. Several people did write to me that they were concerned about very specific groups rifling through their code expressly for the purpose of scooping them. For me it's hard to imagine someone even having that attitude, and my advice is that if you do face such a toxic community, it might be wise to change your chosen field of study.

Nevertheless, I want to emphasise here that open source programming can take many forms, with the zip file attached to the paper being the lowest, coding in the open being the highest, and several other models in between. Any steps you can take towards the higher models will ultimately help you. My preferred mode for code that really does have to be private is to use a private GitHub repository, and <em>just make that repo public once the paper is accepted.</em>

A lot of people prefer the "code dump with no revision history" model of post-publication sharing, but this tosses out a lot of valuable information for people coming after you: what have you tried that didn't work? What issues did you have with the code? Have you considered coding in one style or another? The code dump model also makes you less likely to use GitHub in the first place, depriving you of an opportunity to learn some valuable real-world skills.

<h2>For coding, scientists have even more severe impostor syndrome</h2>

As I mentioned in my original post, and this I find completely uncontestable, <em>publishing shitty code is not a bad thing.</em> <em>Everybody writes bad code,</em> and nearly everybody knows it. Here's Hadley Wickham, creator of <a href="https://dplyr.tidyverse.org">dplyr</a>, <a href="https://tidyr.tidyverse.org">tidy data</a>, <a href="https://ggplot2.tidyverse.org">ggplot2</a>, among other things; in other words, someone who knows a thing or two about elegant code and about as close as one gets to coding royalty in science:

<blockquote>
  The only way to write great code is to write lots of shitty code first.
</blockquote>

Publishing your raw code is a good thing and will absolutely not be a black mark on your career. Indeed, in open source circles, it is often a bare GitHub contribution history that is a black mark. (And this is another problem, but in my opinion a better one.)

<h2>Scientists don't know about open source</h2>

If knowledge of open source is lacking in computer science, what chance does it have in other fields? The truth is that outreach and education need to become a massive part of open source culture, <em>especially</em> in science.

I credit <a href="https://bids.berkeley.edu/people/st%C3%A9fan-van-der-walt">Stéfan van der Walt</a> for my life in open source. After I gave a talk at SciPy 2012, he invited me to join the scikit-image sprint at the end of the conference. If it hadn't been for that, I probably would have just wandered around the hall, too shy to join any sprint (see "impostor syndrome", above), and my life would be very different right now.

Anyway, at that point I'd made my code "open source", which meant it was on GitHub. I had only added a license to submit to the conference. As a reminder, unlicensed code <a href="http://www.astrobetter.com/blog/2014/03/10/the-whys-and-hows-of-licensing-scientific-code/">doesn't count as open source</a>. But I had never really collaborated in open source. My idea of collaboration was my workflow with my colleague: a single branch (master), from which we both pulled and to which we both pushed. When I sat down with Stéfan and <a href="http://tonysyu.github.io">Tony Yu</a>, and I figured what I wanted to work on, I asked: "So, should I just push to master, or what?" I still remember, with some embarrassment, the dubious look Stéfan and Tony exchanged, as they silently figured out which of them would introduce this newbie to <a href="https://help.github.com/articles/creating-a-pull-request/">pull requests</a>.

But that's the thing: I shouldn't feel embarrassment. Scientists for the most part don't get introduced to coding in their education, much less to open source.

<h2>What can scientists in open source do?</h2>

A lesson from my continued contributions to the SciPy ecosystem, I hope, is that some light mentorship can yield enormous dividends later on. Stéfan and Tony took the time to walk me through the open source contribution process, when they could have dismissively sent me a link to some page explaining it. I'm a big fan of writing good documents for newcomers, but nothing beats a good hand-holding. It's very easy for me to imagine an alternate reality where I had not felt welcome or rewarded by the scikit-image project and my life had not taken this productive turn.

Continuing on imaginary themes, it is only slightly less plausible that the open source scientific world should be awash with new contributors at every level of science. How do we turn this dream into a reality?

If you are a scientist and this post is among your first encounters with the term "open source", and you think you might be interested in learning more, here are a few things I recommend, in order of easiest to hardest:

<ul>
<li>Read the <a href="https://github.com/elegant-scipy/elegant-scipy/blob/master/markdown/preface.markdown">preface</a> and <a href="https://github.com/elegant-scipy/elegant-scipy/blob/master/markdown/epilogue.markdown">epilogue</a> of my book with Stéfan and <a href="http://harrietdashnow.com">Harriet Dashnow</a>. (Free online!) I feel a bit icky recommending my own book, but why repeat myself? In those chapters I tried to distill my thoughts on joining the SciPy community, which is a fantastic, rewarding space in which to do open source programming as a scientist. I expect many things we wrote generalise well to e.g. <a href="https://www.tidyverse.org">the tidyverse</a>.</li>
<li>Look for upcoming <a href="https://software-carpentry.org">software carpentry</a> workshops near you. These are free two-day programming boot camps to introduce you to computational thinking, and, crucially, to version control with git.</li>
<li>Go to a <a href="https://conference.scipy.org">SciPy conference</a>. I know of SciPy, EuroSciPy, and SciPy India, but I have a vague memory of offshoots in Africa and South America.</li>
</ul>

If you are in a boat similar to mine (intermediate/advanced open source contributor in science), and you feel like you would like your work to feel a bit more crowded, I can tell you what I'm going to be doing in response to this talk:

<ul>
<li>Sign up to deliver (more) software carpentry training (or similar). Getting the word out is the number one thing.</li>
<li>In software carpentry, emphasise the role of git in collaboration. (I think the official program does not go far enough in this direction, and focuses instead on the initial linear history.)</li>
<li>If you are located in a university, talk to your CS department to see whether they have any courses in open source development. If not, see whether you can guest lecture in a suitable course to make students aware of the open source opportunities out there.</li>
<li>Similarly, follow up software carpentry with more advanced sessions on open source collaboration. I gained an enormous fraction of my programming skills from collaborating on open source. I really think there is no better tool for long-term learning in this space. An idea that I'd like to try out is to curate a bunch of open issues on prominent repos and get SWC students to sprint on them for a day<sup id="fnref-1111-1"><a href="#fn-1111-1" class="jetpack-footnote">1</a></sup>. I know about the "good first issue" tag on GitHub. Unfortunately, my experience with it is mixed. I think many repos are overly optimistic with theirs (this includes scikit-image), and, furthermore, a large proportion of these tagged issues get "claimed" quickly — and often half-heartedly!</li>
<li>Write, write, write! Did you get a cool PR merged? Write a blog post about it! Or at least tweet! We need to get the message out that writing PRs is for everyone. =)</li>
</ul>

If you have any further ideas, I'd love to hear them.

<div class="footnotes">
<hr>
<ol>

<li id="fn-1111-1">
Actually I drafted this post a while back, and tried this yesterday, with mixed success. I'll write about that experience soon. ;) <a href="#fnref-1111-1">↩</a>
</li>

</ol>
</div></body></html>
