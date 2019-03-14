<!--
.. title: Some things I learned while building a site on GitHub Pages
.. slug: some-things-i-learned-while-building-a-site-on-github-pages
.. date: 2015-01-04 05:09:10
.. tags: github pages,Jekyll,markdown,Planet SciPy,web development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>Understatement: I'm not much of a web developer. However, we all have to become a little bit versed in web-dev if we want to publish things these days. GitHub Pages makes it really easy to publish a site (check out the <a href="https://pages.github.com">official guide</a>, and Thinkful's truly excellent <a href="http://www.thinkful.com/learn/a-guide-to-using-github-pages/">interactive getting started guide</a>).

If you just want to publish a static set of html files using absolute paths, you'll be fine. However, Pages uses <a href="http://jekyllrb.com">Jekyll</a>, a server that can transform collections of Markdown, HTML, and other files into full-fledged websites. The process is definitely full of gotchas, though, and you'll run into issues for anything other than single pages. I'm making this list for my own future reference, and so that I can finally close the umpteen tabs I have open on the topic! But I hope someone else will find it useful.

</p><h2>1. Jekyll lets you write Markdown, but it's not GitHub-Flavored Markdown</h2>

<strong>Update:</strong> This is no longer true. You can use GFM (or something very similar to it) using <code>kramdown</code> as the renderer.

You might assume, since you are writing markdown hosted on GitHub, that <a href="https://github.github.com/github-flavored-markdown/">GitHub-Flavored Markdown (GFM)</a> would work. But you would be wrong. You can configure Jekyll on GH-Pages to use either <a href="https://github.com/vmg/redcarpet">Redcarpet</a> or <a href="http://kramdown.gettalong.org/syntax.html">Kramdown</a>, both of which have significant syntactic differences with GFM. For example, fenced code blocks <a href="http://kramdown.gettalong.org/syntax.html#fenced-code-blocks">use <code>~~~</code> in Kramdown</a>, instead of GFM's <code>```</code>.

Anyway, to set the Markdown engine used to render your site, set the <code>markdown</code> attribute in the Jekyll configuration file, <code>_config.yml</code>, placed at the root of your repo:

<pre><code>
# Dependencies
markdown:    kramdown
highlighter: pygments
</code></pre>

<h2>2. ... And syntax highlighting doesn't work properly in Jekyll on GH-Pages</h2>

<strong>Update:</strong> This has also been fixed.

Even using the correct delimiters for Kramdown is not good enough to get your code snippets to render properly, because of <a href="https://github.com/jekyll/jekyll/issues/2709">this</a> and <a href="https://github.com/jekyll/jekyll/issues/2715">this</a>. So, instead of using backtick- or tilde-fenced blocks, you have to use slightly clunkier Jekyll "liquid tags", which have the following fairly ugly syntax:

<pre>
{% highlight python %}
    from scipy import ndimage as nd
{% endhighlight %}
</pre>

<h2>3. Linking is a bit weird</h2>

My expectation when I started working with this was that I could write in markdown, and link directly to other markdown files using relative URLs, and Jekyll would just translate these to HTML links when serving the site. This is how <a href="http://www.mkdocs.org">MkDocs</a> and local viewers such as <a href="http://marked2app.com">Marked 2</a> work, for example.

That's not at all how Jekyll does things. <strong>Every markdown page that you want served needs to have a header called the <a href="http://jekyllrb.com/docs/frontmatter/">Front Matter</a>.</strong> Other markdown files will be publicly accessible but they will be treated as plaintext files.

<em>How</em> the markdown files get served also varies depending on your <code>_config.yml</code> and the <code>permalink</code> variable. The annoying thing is that the <a href="https://github.com/jekyll/jekyll/issues/1293">documentation</a> on this is entirely centred on blog posts, with little indication about what happens to top-level pages. In the case of the <code>pretty</code> setting, <code>filename.markdown</code>, for example, gets served at <code>base.url/filename/</code>, <em>unless</em> you set the page's <code>permalink</code> attribute manually in the Front Matter, for example to <code>title.html</code> or <code>title/</code>.

<h2>4. Relative paths don't work on GH-Pages</h2>

This is a biggy, and super-annoying, because even if you follow GitHub's <a href="https://help.github.com/articles/using-jekyll-with-pages/">guide for local development</a>, <em>all</em> your style and layout files will be missing once you deploy to Pages.

The problem is that Jekyll assumes that your site lives at the root URL (i.e. <code>username.github.io</code>), when it is in fact in <code>username.github.io/my-site</code>.

The solution was developed by Matt Swensen <a href="https://github.com/jekyll/jekyll/issues/332#issuecomment-18952908">here</a>: You need to set a <code>baseurl</code> variable in your <code>_config.yml</code>. This should be your repository name. For example, if you post your site to the <code>gh-pages</code> branch of the repository <code>https://github.com/username/my-jekyll-site</code>, which gets served at <code>username.github.io/my-jekyll-site/</code>, you should set <code>baseurl</code> to <code>my-jekyll-site</code>.

Once you've set the <code>baseurl</code>, you <em>must</em> use it when linking to CSS and posts, using liquid tags: <code>{{ site.baseurl }}/path/to/style.css</code> and <code>{{ site.baseurl }}{{ post.url }}</code>.

When you're testing the site locally, you can either manually configure the <code>baseurl</code> tag to the empty string (<code>bundle exec jekyll serve --baseurl ''</code>), so you can navigate to 0.0.0.0:4000 as you would normally with Jekyll; or you can <a href="http://blog.parkermoore.de/2014/04/27/clearing-up-confusion-around-baseurl">leave it untouched</a> and navigate to 0.0.0.0:4000/my-jekyll-site, mirroring the structure of GitHub Pages.

<strong>This is all documented in a specific Jekyll docs page</strong> that is unfortunately buried after <em>a lot</em> of other, less-relevant Jekyll docs. As a reference, look <a href="http://jekyllrb.com/docs/github-pages/">here</a>, and search for "Project Page URL Structure".

<h2>5. You <em>must</em> provide an index.html or index.md file</h2>

Otherwise you get a rather unhelpful <a href="https://github.com/jekyll/jekyll/issues/1293">Forbidden Error</a>.

<h2>And 6. There is a "console" syntax highlighter for bash sessions</h2>

I'd always used "bash" as my highlighter whenever I wanted to show a shell session, but it turns out that "console" does a slightly better job, by highlighting <code>$</code> prompts and commands, while graying output slightly. Christian Jann, who has written his own more-specific highlighter, ShellSession, has a <a href="http://www.jann.cc/pygments-shell-session-lexer-demo/shell_code_comparison.html">nice comparison</a> of the three. Although ShellSession is cool, I couldn't get it to work out-of-the-box on Jekyll.

<hr>

That should be enough to get you started... (Don't forget to check out <a href="http://www.thinkful.com/learn/a-guide-to-using-github-pages/">Thinkful's guide</a>!) Happy serving!</body></html>
