<!--
.. title: The road to scikit-image 1.0
.. slug: the-road-to-scikit-image-1-0
.. date: 2018-07-13 04:58:35
.. tags: image analysis,open-source,Planet SciPy,Python,programming,software
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>This is the first in a series of posts about the joint scikit-image, scikit-learn, and dask sprint that took place at the Berkeley Insitute of Data Science, May 28-Jun 1, 2018.

In addition to the dask and scikit-learn teams, the sprint brought together three core developers of scikit-image (Emmanuelle Gouillart, Stéfan van der Walt, and myself), and two newer contributors, Kira Evans and Mark Harfouche. Since we are rarely in the same timezone, let alone in the same room, we took the opportunity to discuss some high level goals using a framework suggested by Tracy Teal (via Chris Holdgraf): <em>Vision, Mission, Values</em>. I'll try do Chris's explanation of these ideas justice:

</p><ul>
<li>Vision: what are we trying to achieve? What is the future that we are trying to bring about?</li>
<li>Mission: what are we going to do about it? This is the <em>plan</em> needed to make the vision a reality.</li>
<li>Values: what are we <em>willing</em> to do, and <em>not</em> willing to do, to complete our mission?</li>
</ul>

So, on the basis of this framework, I'd like to review where scikit-image is now, where I think it needs to go, and the ideas that Emma, Stéfan, and I came up with during the sprint to get scikit-image there.

I will point out, from the beginning, that one of our values is that we are <em>community-driven</em>, and this is not a wishy-washy concept. (More below.) Therefore this blog post constitutes only a preliminary document, designed to kick-start an <em>official roadmap</em> for scikit-image 1.0 with more than a blank canvas. The roadmap will be debated on GitHub and the mailing list, open to discussion by anyone, and when completed will appear on our webpage. <em>This post is not the roadmap.</em>

<!-- TEASER_END -->

<h2>Part one: where we are</h2>

scikit-image is a tremendously successful project that I feel very proud to have been a part of until now. I still cherish the email I got from Stéfan inviting me to join the core team. (Five years ago now!)

Like many open source projects, though, we are threatened by our own success, with feature requests and bug reports piling on faster than we can get through them. And, because we grew organically, with no governance model, it is often difficult to resolve thorny questions about API design, what gets included in the library, and how to deprecate old functionality. Discussion usually stalls before any decision is taken, resulting in a process heavily biased towards inaction. Many issues and PRs languish for years, resulting in a double loss for the project: a smaller loss from losing the PR, and a bigger one from losing a potential contributor that understandably has lost interest.

Possibly the most impactful decision that we took at the BIDS sprint is that at least three core developers will video once a month to discuss stalled issues and PRs. (The logistics are still being worked out.) We hope that this sustained commitment will move many PRs and issues forward much faster than they have until now.

<h2>Part two: where we're going</h2>

Onto the framework. What are the vision, mission, and values of scikit-image? How will these help guide the decisions that we make daily and in our dev meetings?

<h3>Our vision</h3>

We want scikit-image to be <em>the</em> reference image processing and analysis library for science in Python. In one sense I think that we are already there, but there are more than enough remaining warts that they might cause the motivated user to go looking elsewhere. The vision, then, is to increase our customer satisfaction fraction in this space to something approaching 1.0.

<h3>Our mission</h3>

How do we get there? Here is our mission:

<ul>
<li>Our library must be <strong>easily re-usable.</strong> This means that we will be careful in adding new dependencies, and possibly cull some existing ones, or make them optional. We also want to remove some of the bigger test datasets from our package, which at 24MB is getting rather unwieldy! (By comparison, Python 3.7 is 16MB.) (Props to Josh Warner for noticing this.)</li>
<li>It also means providing a <strong>consistent API.</strong> This means that conceptually identical function arguments, such as images, label images, and arguments defining whether an input image is grayscale, should have the same name across various the library. We've made great strides in this goal thanks to Egor Panfilov and <a href="https://github.com/scikit-image/scikit-image/issues/2538">Adrian Sieber</a>, but we still have some way to go.</li>
<li>We want to <strong>ensure accuracy</strong> of our algorithms. This means comprehensive testing, even against external libraries, and engaging experts in relevant fields to audit our code. (Though this of course is a challenge!)</li>
<li>Show utmost <strong>care with users' data</strong>. Not that we haven't cared until now, but there are places in scikit-image where too much responsibility (in my view) rests with the user, with insufficient transparency from our functions for new users to predict what will happen to their data. For example, we are quite liberal with how we deal with input data: it gets rescaled whenever we need to change the type, for example from unsigned 8-bit integers (uint8) to floating point. Although we have <a href="https://github.com/scikit-image/scikit-image/issues/2677#issuecomment-309717979">good technical reasons</a> for doing this, and rather <a href="http://scikit-image.org/docs/dev/user_guide/data_types.html">extensive documentation about it</a>, these conversions are the source of much user confusion. We are aiming to improve this in <a href="https://github.com/scikit-image/scikit-image/issues/3009">issue 3009</a>. Likewise, we don't handle image metadata at all. What is the physical extent of the input image? What is the range and units of the data points in the image? What do the different channels represent? These are all important questions in scientific images, but until now we have completely abdicated responsibility in them and simply ignore any metadata. I don't think this is tenable for a scientific imaging library. We don't have a good answer for how we will do it, but I consider this a must-solve before we can call ourselves 1.0.</li>
</ul>

<h3>Our values</h3>

Finally, how do we solve the thorny questions of API design, whether to include algorithms, etc? Here are our values:

<ul>
<li>We used the word "reference" in our vision. This phrasing is significant. It means that <strong>we value elegant implementations</strong>, that are <em>easy to understand for newcomers</em>, over obtaining every last ounce of speed. This value is a useful guide in reviewing pull requests. We will prefer a 20% slowdown when it reduces the lines of code two-fold.</li>
<li>We also used the word <em>science</em> in our vision. This means our aim is to <strong>serve scientific applications</strong>, and not, for example, image editing in the vein of Photoshop or GIMP. Having said this, we value being part of diverse scientific fields. (One of the first citations of the scikit-image paper was a remote sensing paper, to our delight: none of the core developers work in that field!)</li>
<li><strong>We are inclusive.</strong> From my first contributions to the project, I have received patient mentorship from Stéfan, Emmanuelle, Johannes Schönberger, Andy Mueller, and others. (Indeed, I am still learning from fellow contributors, as seen <a href="https://github.com/scikit-image/scikit-image/pull/3031#issuecomment-398961212">here</a>, to show just one example.) We will continue to welcome and mentor newcomers to the Scientific Python ecosystem who are making their first contribution.</li>
<li>Both of the above points have a corrolary: <strong>we require excellent documentation</strong>, in the form of usage examples, docstrings documenting the API of each function, and comments explaining tricky parts of the code. This requirement has stalled a few PRs in the past, but this is something that our monthly meetings will specifically address.</li>
<li><strong>We don't do magic.</strong> We use NumPy arrays instead of fancy façade objects that mask their complexity. We prefer to educate our users over making decisions on their behalf (through quality documentation, particularly in docstrings).</li>
<li><strong>We are community-driven</strong>, which means that decisions about the API and features will be driven by our users' requirements, and not the whims of the core team. (For example, I would happily <a href="http://toolz.readthedocs.io/en/latest/curry.html">curry</a> all of our functions, but that would be confusing to most users, so I suffer in silence. =P)</li>
</ul>

I hope that the above values are uncontroversial in the scikit-image core team. (I myself used to fall heavily on the pro-magic side, but hard experience with this library has shown me the error of my ways.) I also hope, but more hesitantly, that our much wider community of users will also see these values as, well, valuable.

As I mentioned above, I hope this blog post will spawn a discussion involving both the core team and the wider community, and that this discussion can be distilled into a public roadmap for scikit-image.

<h2>Part three: scikit-image 1.0</h2>

I have deliberately left out new features off the mission, except for metadata handling. The library will never be "feature complete". But we <em>can</em> develop a stable and consistent enough API that adding new features will almost never require breaking it.

For completeness, I'll compile my personal pet list of things I will attempt to work on or be particularly excited about other people working on. This is <em>not</em> part of the roadmap, it's part of my roadmap.

<ul>
<li>Near-complete support for n-dimensional data. I want 2D-only functions to become the exception in the library, maybe so much so that we are forced to add a <code>_2d</code> suffix to the function name.</li>
<li>Typing support. I never want to move from simple arrays as our base data type, but I want a way to systematically distinguish between plain images, label images, coordinate lists, and other types, in a way that is accessible to automatic tools.</li>
<li>Basic image registration functionality.</li>
<li>Evaluation algorithms for all parts of the library (such as segmentation, or keypoint matching).</li>
</ul>

<h2>The human side</h2>

Along with articulating the way we see the project, another key part of getting to 1.0 is supporting existing maintainers, and onboarding new ones. It is clear that the project is currently straining under the weight of its popularity. While we solve one issue, three more are opened, and two pull requests.

In the past, we have been too hesitant to invite new members to the core team, because it is difficult to tell whether a new contributor shares your vision. Our roadmap document is an important step towards rectifying this, because it clarifies where the library is going, and therefore the decision making process when it comes to accepting new contributions, for example.

In a followup to this post, I aim to propose a <em>maintainer onboarding document</em>, in a similar vein, to make sure that new maintainers all share the same process when evaluating new PRs and communicating with contributors. A governance model is also in the works, by which I mean that Stéfan has been wanting to establish one for years and now Emmanuelle and I are onboard with this plan, and I hope others will be too, and now we just need to decide on the damn thing.

I hope that all of these changes will allows us to reach the scikit-image 1.0 milestone sooner rather than later, and that everyone reading this is as excited about it as I was while we hashed this plan together.

As a reminder, <strong>this is not our final roadmap</strong>, nor our final <strong>vision/mission statement</strong>. Please comment on the corresponding <a href="https://github.com/scikit-image/scikit-image/issues/3263">GitHub issue</a> for this post if you have thoughts and suggestions! (You can also use the <a href="https://mail.python.org/mailman/listinfo/scikit-image">mailing list</a>, and we will soon provide a way to submit anonymous comments, too.) As a community, we will come together to create the library we all want to use and contribute to.

As a reminder, everything in this blog is <a href="https://dancohen.org/2013/11/26/cc0-by/">CC0+BY</a>, so feel free to reuse any or all of it in your own projects! And I want to thank BIDS, and specifically Nelle Varoquaux at BIDS, for making this discussion possible, among many other things that will be written up in upcoming posts.

<strong>Update:</strong> Anonymous comments are now open at https://pollev.com/juannunezigl611. To summarise, to comment on this proposal you can:

<ul>
<li>comment on the <a href="https://github.com/scikit-image/scikit-image/issues/3263">GitHub issue</a></li>
<li>submit a comment below</li>
<li>submit an anonymous comment at https://pollev.com/juannunezigl611</li>
</ul></body></html>
