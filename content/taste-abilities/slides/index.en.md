---
outputs:
- Reveal
title: Better packages
hidden: true
layout: list
weight: 11
output: hugodown::md_document
countdown: true
rmd_hash: 7ea0983666082ee6

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

> If you're an experienced programmer and you're tempted to code-shame someone, try thinking back to your own million lines of bad code. Imagine someone had mocked you then (...). Would you have continued asking for help? Would you have ever gotten through your million lines?

David Robinson's blog post ["A Million Lines of Bad Code"](http://varianceexplained.org/programming/bad-code/)

------------------------------------------------------------------------

## Today's workshop

I'll present a collection of very useful things I've learnt over the past few years.

After each section I'll summarize and ask you to comment.

Then you pick up one thing to improve in your package, and open a PR.

Then another short slidedeck.

Then you get to review someone else's PR!

------------------------------------------------------------------------

## Interface

------------------------------------------------------------------------

### Nice messages

Get to know the [cli package](https://cli.r-lib.org/reference/index.html)

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>variable</span> <span class='o'>&lt;-</span> <span class='m'>42</span></span>
<span><span class='nf'>cli</span><span class='nf'>::</span><span class='nf'><a href='https://cli.r-lib.org/reference/cli_alert.html'>cli_alert_info</a></span><span class='o'>(</span><span class='s'>"Set &#123;.field parameter&#125; to &#123;.val &#123;variable&#125;&#125;"</span><span class='o'>)</span></span><span><span class='c'>#&gt; <span style='color: #00BBBB;'>ℹ</span> Set <span style='color: #00BB00;'>parameter</span> to <span style='color: #0000BB;'>42</span></span></span></code></pre>

</div>

[Vignette to migrate from usethis::ui functions to cli](https://cli.r-lib.org/articles/usethis-ui.html)

------------------------------------------------------------------------

### Nice messages

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

### Nice messages

Further reading: <https://github.com/ropensci/dev_guide/issues/603>

*:toolbox: Are there messages in your package you could improve?*

------------------------------------------------------------------------

### Error messages

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

### Error messages

*:toolbox: Go through your package's error messages (look for [`stop()`](https://rdrr.io/r/base/stop.html) and equivalents). Could some of them be improved by applying the tidyverse guidance?*

------------------------------------------------------------------------

### Argument checks

-   Document argument type, default.

-   Check arguments. [`rlang::arg_match()`](https://rlang.r-lib.org/reference/arg_match.html) for instance.

Further reading: [Checking the inputs of your R functions](https://blog.r-hub.io/2022/03/10/input-checking/) by Hugo Gruson , Sam Abbott , Carl Pearson.

------------------------------------------------------------------------

### Arguments checks

*:toolbox: Does your package document and validate arguments? Improve this in one function.*

------------------------------------------------------------------------

## Interface :microphone: `stop()` :microphone:

-   Nice messages with {cli}.
-   Error messages with {cli}, tidyverse style guide.
-   Argument checks with rlang, R-hub blog post.

Please post in the chat

-   Something you found interesting!
-   Something you disagreed with!
-   A recent good/bad experience with these tools?

------------------------------------------------------------------------

## Less code or less headaches

------------------------------------------------------------------------

### Weigh your dependencies

Does this dependency spark joy? :wink:

-   A dependency is code that someone else carefully crafted and tested!
-   A dependency is a failure point.

Further reading: [Dependencies: Mindset and Background](https://r-pkgs.org/dependencies-mindset-background.html) in the R Packages book by Hadley Wickham and Jenny Bryan.

------------------------------------------------------------------------

### Weigh your dependencies

In [rOpenSci dev guide](https://devguide.ropensci.org/building.html#recommended-scaffolding)

-   curl, httr2, httr. Not RCurl.

-   jsonlite. Not rjson nor RJSONIO.

-   xml2. Not XML

-   sf, spatial suites developed by the r-spatial and rspatial communities. Not sp, rgdal, maptools, rgeos.

------------------------------------------------------------------------

### Weigh your dependencies

*:toolbox: Are there dependencies you could add, replace or remove in your package?*

------------------------------------------------------------------------

### Less code? Beyond using dependencies

Feature creep: "excessive ongoing expansion or addition of new features in a product" <https://en.wikipedia.org/wiki/Feature_creep>

Okay to split the package.

Okay to say no to feature requests. [Example](https://github.com/r-lib/pkgdown/issues/1430#issuecomment-924268834)

------------------------------------------------------------------------

### Less code

*:toolbox: Are there feature requests you'd like to say no to? Save answer as [GitHub reply](https://docs.github.com/en/get-started/writing-on-github/working-with-saved-replies/creating-a-saved-reply)?*

------------------------------------------------------------------------

## Less code :microphone: `stop()` :microphone:

-   Choosing dependencies.
-   Dependencies to avoid.
-   Defining package scope.

Please post in the chat

-   Something you found interesting!
-   Something you disagreed with!
-   A recent good/bad experience with these tools?

------------------------------------------------------------------------

## Code

------------------------------------------------------------------------

### Code as simple as possible: early returns

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

### Code as simple as possible: early returns

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

### Code as simple as possible: early returns

Further reading: [Code Smells and Feels](https://github.com/jennybc/code-smells-and-feels) by Jenny Bryan

*:toolbox: Look at logic in one or a few functions. Could you simplify it with early returns, helper functions?*

------------------------------------------------------------------------

### Code aesthetics

Some of it only relevant if you see code.

-   Use alignment?
-   Use paragraphs
-   Use "header" comments for navigation.

------------------------------------------------------------------------

#### Code alignment

-   Align argument in function definitions.

-   More vertical alignment? I am not sensitive to it. :innocent:

------------------------------------------------------------------------

#### Paragraphs

One paragraph = one idea (works for writing prose too!).

Vertical space is costly (what fits on the screen?)

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>head</span> <span class='o'>&lt;-</span> <span class='nf'>collect_metadata</span><span class='o'>(</span><span class='nv'>website</span><span class='o'>)</span></span>
<span><span class='nv'>head_string</span> <span class='o'>&lt;-</span> <span class='nf'>stringify</span><span class='o'>(</span><span class='nv'>head</span><span class='o'>)</span></span>
<span></span>
<span><span class='nv'>body</span> <span class='o'>&lt;-</span> <span class='nf'>create_content</span><span class='o'>(</span><span class='nv'>website</span><span class='o'>)</span></span>
<span><span class='nv'>body_string</span> <span class='o'>&lt;-</span> <span class='nf'>stringify</span><span class='o'>(</span><span class='nv'>body</span><span class='o'>)</span></span>
<span></span></code></pre>

</div>

------------------------------------------------------------------------

#### Header comments

At least in RStudio IDE, outline on the right. In any case good to indicate high-level structure within a script.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'># Header level 1 ----
more code

## Header level 2 ----
more code</code></pre>

</div>

------------------------------------------------------------------------

### Code aesthetics

*:toolbox: Open one or a few scripts, can you improve the aesthetics?*

------------------------------------------------------------------------

### Less comments / self explaining code

Comments are like little alerts. Don't create fatigue!

Comments that repeat the code get out of date.

------------------------------------------------------------------------

### Less comments / self explaining code

``` r
# use only non empty strings
if (!is.na(x) && nzchar(x)) {
  use_string(x)
}
```

------------------------------------------------------------------------

### Less comments / self explaining code

``` r
is_non_empty_string <- function(x) {
  !is.na(x) && nzchar(x)
}

if (is_non_empty_string(x)) {
  use_string(x)
}
```

------------------------------------------------------------------------

### Less comments / self-explaining code

Further reading: <https://blog.r-hub.io/2023/01/26/code-comments-self-explaining-code/>

*:toolbox: Are there opportunities for less comments (or more comments!) in some of your scripts?*

------------------------------------------------------------------------

## Code :microphone: `stop()` :microphone:

-   Early returns.
-   Code aesthetics.
-   Less comments/self-explaining code.

Please post in the chat

-   Something you found interesting!
-   Something you disagreed with!
-   A recent good/bad experience with these tools?

------------------------------------------------------------------------

## Test code

------------------------------------------------------------------------

### DAMP / DRY

DAMP: descriptive and meaningful.

DRY: don't repeat yourself.

A trade-off!

------------------------------------------------------------------------

### Test code vs code

Code is covered by test code so we can take more risks!

------------------------------------------------------------------------

### Ideal tests

-   Self-contained.

-   Can be run interactively. [`testthat::test_path()`](https://testthat.r-lib.org/reference/test_path.html).

-   No leak. {withr}. [`withr::local_options()`](https://withr.r-lib.org/reference/with_options.html), [`withr::local_tempdir()`](https://withr.r-lib.org/reference/with_tempfile.html)...

------------------------------------------------------------------------

### Example: {swamp}

Let's explore <https://github.com/maelle/swamp>

*:toolbox: Do some of your tests have top-level code? Can you create helper files and helper functions, and repeat object creation in each test?*

------------------------------------------------------------------------

### Escape hatches

My code

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span></span>
<span></span>
<span><span class='nv'>is_internet_down</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>  <span class='o'>!</span><span class='nf'>curl</span><span class='nf'>::</span><span class='nf'><a href='https://rdrr.io/pkg/curl/man/nslookup.html'>has_internet</a></span><span class='o'>(</span><span class='o'>)</span></span>
<span><span class='o'>&#125;</span></span>
<span></span>
<span><span class='nv'>my_complicated_code</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>  <span class='kr'>if</span> <span class='o'>(</span><span class='nf'>is_internet_down</span><span class='o'>(</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>    <span class='nf'><a href='https://rdrr.io/r/base/message.html'>message</a></span><span class='o'>(</span><span class='s'>"No internet! Le sigh"</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span></span>
<span>  <span class='c'># blablablabla</span></span>
<span><span class='o'>&#125;</span></span></code></pre>

</div>

How to test for the message?

------------------------------------------------------------------------

### Escape hatch

Let's add a switch/escape hatch

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span><span class='nv'>is_internet_down</span> <span class='o'>&lt;-</span> <span class='kr'>function</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span></span>
<span>  <span class='kr'>if</span> <span class='o'>(</span><span class='nf'><a href='https://rdrr.io/r/base/nchar.html'>nzchar</a></span><span class='o'>(</span><span class='nf'><a href='https://rdrr.io/r/base/Sys.getenv.html'>Sys.getenv</a></span><span class='o'>(</span><span class='s'>"TESTPKG.NOINTERNET"</span><span class='o'>)</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>&#123;</span></span>
<span>    <span class='kr'><a href='https://rdrr.io/r/base/function.html'>return</a></span><span class='o'>(</span><span class='kc'>TRUE</span><span class='o'>)</span></span>
<span>  <span class='o'>&#125;</span></span>
<span></span>
<span>  <span class='o'>!</span><span class='nf'>curl</span><span class='nf'>::</span><span class='nf'><a href='https://rdrr.io/pkg/curl/man/nslookup.html'>has_internet</a></span><span class='o'>(</span><span class='o'>)</span></span>
<span><span class='o'>&#125;</span></span></code></pre>

</div>

------------------------------------------------------------------------

### Escape hatches

In the test,

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span></span>
<span></span>
<span><span class='nf'>test_that</span><span class='o'>(</span><span class='s'>"my_complicated_code() notes the absence of internet"</span>, <span class='o'>&#123;</span></span>
<span>  <span class='nf'>withr</span><span class='nf'>::</span><span class='nf'><a href='https://withr.r-lib.org/reference/with_envvar.html'>local_envvar</a></span><span class='o'>(</span><span class='s'>"TESTPKG.NOINTERNET"</span> <span class='o'>=</span> <span class='s'>"blop"</span><span class='o'>)</span></span>
<span>  <span class='nf'>expect_message</span><span class='o'>(</span><span class='nf'>my_complicated_code</span><span class='o'>(</span><span class='o'>)</span>, <span class='s'>"No internet"</span><span class='o'>)</span></span>
<span><span class='o'>&#125;</span><span class='o'>)</span></span>
<span></span>
<span></span></code></pre>

</div>

------------------------------------------------------------------------

### Escape hatches

Further reading: <https://blog.r-hub.io/2023/01/23/code-switch-escape-hatch-test/>

------------------------------------------------------------------------

### Escape hatches

*:toolbox: do you have such a situation to test?*

------------------------------------------------------------------------

## Test code :microphone: `stop()` :microphone:

-   DAMP & DRY
-   Test code vs code
-   Ideal tests (self contained, can be run interactively, no leak)
-   Escape hatches

Please post in the chat

-   Something you found interesting!
-   Something you disagreed with!
-   A recent good/bad experience with these tools?

------------------------------------------------------------------------

## Choose your own adventure

...with your own package! In breakout rooms.

:warning: Make Pull Requests and don't merge them yet! :warning:

We'll gather in XX minutes as a group to discuss.

