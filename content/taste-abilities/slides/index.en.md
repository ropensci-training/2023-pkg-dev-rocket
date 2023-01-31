---
outputs:
- Reveal
title: Better packages
hidden: true
layout: list
weight: 11
output: hugodown::md_document
countdown: true
rmd_hash: cbf598d53cf13afa

---

# Better packages

<div class="highlight">

</div>

<div class="highlight">

{{< figure src="3697811.jpeg" alt="Red rocket ship" caption="Picture by [Matheus Bertelli on Pexels](https://www.pexels.com/photo/editorial-photo-of-red-rocket-ship-3697811/)." width="250" >}}

</div>

------------------------------------------------------------------------

## My R package development creds

I really :heart: R package development

-   Volunteer editor for [rOpenSci Software Peer Review](https://ropensci.org/software-review).

-   At work, maintenance of [rOpenSci dev guide](https://devguide.ropensci.org).

-   Created the [R-hub blog](https://blog.r-hub.io).

-   Worked on the [HTTP testing in R](https://books.ropensci.org/http-testing/) book.

------------------------------------------------------------------------

## My R package development creds

Contributed to

-   [pkgdown 2.0.0](https://www.tidyverse.org/blog/2021/12/pkgdown-2-0-0/) (to produce documentation websites for packages)

-   [fledge 0.1.0](https://cynkra.github.io/fledge/) (Smoother change tracking and versioning for R packages)

-   [glitter 0.1.0](https://lvaudor.github.io/glitter/) (a SPARQL domain-specific language)

------------------------------------------------------------------------

## What is good code

> The only way to write good code is to write tons of shitty code first. Feeling shame about bad code stops you from getting to good code

[Hadley Wickham's 2015 tweet](https://twitter.com/hadleywickham/status/589068687669243905)

------------------------------------------------------------------------

## What is good code?

> If you're an experienced programmer and you're tempted to code-shame someone, try thinking back to your own million lines of bad code. Imagine someone had mocked you then, the way I'd like to mock myself for my Perl/Java "solution". Would you have continued asking for help? Would you have ever gotten through your million lines?

David Robinson's blog post ["A Million Lines of Bad Code"](http://varianceexplained.org/programming/bad-code/)

------------------------------------------------------------------------

## Today's workshop

I'll present a collection of very useful things I've learnt over the past few years.

Then we can discuss or you can pick one aspect to explore in your one package.

------------------------------------------------------------------------

## Interface

------------------------------------------------------------------------

## Nice messages

Get to know the [cli package](https://cli.r-lib.org/reference/index.html)

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>variable</span> <span class='o'>&lt;-</span> <span class='m'>42</span></span>
<span><span class='nf'>cli</span><span class='nf'>::</span><span class='nf'><a href='https://cli.r-lib.org/reference/cli_alert.html'>cli_alert_info</a></span><span class='o'>(</span><span class='s'>"Set &#123;.field parameter&#125; to &#123;.val &#123;variable&#125;&#125;"</span><span class='o'>)</span></span><span><span class='c'>#&gt; <span style='color: #00BBBB;'>ℹ</span> Set <span style='color: #00BB00;'>parameter</span> to <span style='color: #0000BB;'>42</span></span></span></code></pre>

</div>

------------------------------------------------------------------------

## Nice messages

How to control verbosity? How to shup your package up?

-   argument in each function :weary:

-   global option à la `usethis.quiet`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>cli_alert_info</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='nv'>...</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>  <span class='kr'>if</span> <span class='o'>(</span><span class='o'>!</span><span class='nf'><a href='https://rdrr.io/r/base/options.html'>getOption</a></span><span class='o'>(</span><span class='s'>"usethis.quiet"</span>, default <span class='o'>=</span> <span class='kc'>FALSE</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>    <span class='nf'>cli</span><span class='nf'>::</span><span class='nf'><a href='https://cli.r-lib.org/reference/cli_alert.html'>cli_alert_info</a></span><span class='o'>(</span><span class='nv'>...</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span></span>
<span><span class='o'>&#125;</span></span></code></pre>

</div>

------------------------------------------------------------------------

## Nice messages

Further reading: <https://github.com/ropensci/dev_guide/issues/603>

*:toolbox: Are there messages in your package you could improve?*

------------------------------------------------------------------------

## Error messages

-   Tips on content in the [tidyverse style guide](https://style.tidyverse.org/error-messages.html) with examples.

-   Interface with [`cli::cli_abort()`](https://cli.r-lib.org/reference/cli_abort.html)

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nf'>cli</span><span class='nf'>::</span><span class='nf'><a href='https://cli.r-lib.org/reference/cli_abort.html'>cli_abort</a></span><span class='o'>(</span></span>
<span>  <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span></span>
<span>    <span class='s'>"Can't find good error message."</span>,</span>
<span>    i <span class='o'>=</span> <span class='s'>"Read the tidyverse style guide."</span></span>
<span>  <span class='o'>)</span></span>
<span><span class='o'>)</span></span><span><span class='c'>#&gt; <span style='color: #BBBB00; font-weight: bold;'>Error</span><span style='font-weight: bold;'>:</span></span></span>
<span><span class='c'>#&gt; <span style='color: #BBBB00;'>!</span> Can't find good error message.</span></span>
<span><span class='c'>#&gt; <span style='color: #00BBBB;'>ℹ</span> Read the tidyverse style guide.</span></span></code></pre>

</div>

------------------------------------------------------------------------

## Error messages

*:toolbox: Go through your package's error messages (look for [`stop()`](https://rdrr.io/r/base/stop.html) and equivalents). Could some of them be improved by applying the tidyverse guidance?*

------------------------------------------------------------------------

## Argument checks

-   Document argument type, default.

-   Check arguments. [`rlang::arg_match()`](https://rlang.r-lib.org/reference/arg_match.html) for instance.

Further reading: [Checking the inputs of your R functions](https://blog.r-hub.io/2022/03/10/input-checking/) by Hugo Gruson , Sam Abbott , Carl Pearson.

------------------------------------------------------------------------

## Arguments checks

*:toolbox: Does your package document and validate arguments? Improve this in one function.*

------------------------------------------------------------------------

## Less code or less headaches

------------------------------------------------------------------------

## Weigh your dependencies

Does this dependency spark joy? :wink:

-   A dependency is code that someone else carefully crafted and tested!
-   A dependency is a failure point.

Further reading: [Dependencies: Mindset and Background](https://r-pkgs.org/dependencies-mindset-background.html) in the R Packages book by Hadley Wickham and Jenny Bryan.

------------------------------------------------------------------------

## Weight your dependencies

*:toolbox: Are there dependencies you could add or remove from your package?*

------------------------------------------------------------------------

## Less code?

Feature creep: " excessive ongoing expansion or addition of new features in a product" <https://en.wikipedia.org/wiki/Feature_creep>

Okay to split the package.

Okay to say no to feature requests. [Example](https://github.com/r-lib/pkgdown/issues/1430#issuecomment-924268834)

> Thanks for filing this issue! Unfortunately, developing good software requires relentless focus, which means that we have to say no to many good ideas. Even though I'm closing this issue, I really appreciate the feedback, and hope you'll continue to contribute in the future smile

------------------------------------------------------------------------

## Less code

*:toolbox: Are there feature requests you'd like to say no to? Save answer as [GitHub reply](https://docs.github.com/en/get-started/writing-on-github/working-with-saved-replies/creating-a-saved-reply)?*

------------------------------------------------------------------------

## Code

------------------------------------------------------------------------

## Code as simple as possible: early returns

:eyes:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>do_this</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>  <span class='kr'>if</span> <span class='o'>(</span><span class='o'>!</span><span class='nf'>is_that_present</span><span class='o'>(</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>    <span class='kr'><a href='https://rdrr.io/r/base/function.html'>return</a></span><span class='o'>(</span><span class='kc'>NULL</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span> <span class='kr'>else</span> <span class='o'>&#123;</span></span>
<span>    <span class='c'># more code</span></span>
<span>    <span class='kr'><a href='https://rdrr.io/r/base/function.html'>return</a></span><span class='o'>(</span><span class='nv'>blip</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span></span>
<span><span class='o'>&#125;</span></span>
<span></span></code></pre>

</div>

------------------------------------------------------------------------

## Code as simple as possible: early returns

:sparkles:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>do_this</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>  <span class='kr'>if</span> <span class='o'>(</span><span class='o'>!</span><span class='nf'>is_that_present</span><span class='o'>(</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>    <span class='kr'><a href='https://rdrr.io/r/base/function.html'>return</a></span><span class='o'>(</span><span class='kc'>NULL</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span> </span>
<span>  <span class='c'># more code</span></span>
<span>  </span>
<span>  <span class='nv'>blip</span></span>
<span><span class='o'>&#125;</span></span>
<span></span></code></pre>

</div>

------------------------------------------------------------------------

## Code as simple as possible: early returns

Further reading: [Code Smells and Feels](https://github.com/jennybc/code-smells-and-feels) by Jenny Bryan

*:toolbox: Look at logic in one or a few functions. Could you simplify it with early returns, helper functions?*

------------------------------------------------------------------------

