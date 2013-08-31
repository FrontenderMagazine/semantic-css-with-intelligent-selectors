# Semantic CSS With Intelligent Selectors

“Form ever follows function. This is the law.” So said the architect and “father 
of skyscrapers” [Louis Sullivan][1]. For architects not wishing to crush 
hundreds of innocent people under the weight of a colossal building, this rule 
of thumb is pretty good. In design, you should **always lead with function**, 
and allow form to emerge as a result. If you were to lead with form, making your 
skyscraper look pretty would be easier, but at the cost of producing something 
pretty dangerous.

So much for architects. What about front-end architects — or “not real
architects,” as we are sometimes known? Do we abide by this law or do we flout
it?

With the advent of [object-oriented CSS][2] (OOCSS), it has become increasingly
fashionable to “[decouple presentation semantics from document semantics][3].”
By leveraging the undesignated meanings of [classes][4], it is possible to
manage one’s document and the *appearance* of one’s document as curiously
separate concerns.

![Overthinking][Overthinking how a functional thing should look.]

In this article, **we will explore an alternative approach to styling Web
documents**, one that marries document semantics to visual design wherever
possible. With the use of “intelligent” selectors, we’ll cover how to query the
extant, functional nature of semantic HTML in such a way as to reward well-
formed markup. If you code it right, you’ll get the design you were hoping for.

If you are like me and have trouble doing or thinking about more than one thing
at a time, I hope that employing some of these ideas will make your workflow
simpler and more transferable between projects. In addition, the final section
will cover a more reactive strategy: We’ll make a CSS bookmarklet that contains
intelligent **attribute selectors** to test for bad HTML and report errors using
pseudo-content.

## Intelligent Selectors

With the invention of style sheets came the possibility of physically separating
document code from the code used to make the document presentable. This didn’t
help us to write better, more standards-aware HTML any more than the advent of
the remote control resulted in better television programming. It just made
things more convenient. By being able to style multiple elements with a single
selector (`p` for paragraphs, for instance), consistency and maintenance became
significantly less daunting prospects.

![DRIVEL][television remote reading DRIVEL]

The `p` selector is an example of an intelligent selector in its simplest form.
The `p` selector is intelligent because it has innate knowledge of semantic
classification. Without intervention by the author, it already knows how to
identify paragraphs and when to style them as such — simple yet effective,
especially when you think of all of the automatically generated paragraphs
produced by WYSIWYG editors.

So, if that’s an intelligent selector, what’s an unintelligent one? Any selector
that requires the author to **intervene and alter the document** simply to
elicit a stylistic nuance is an unintelligent selector. The class is a classic
unintelligent selector because it is not naturally occurring as part of semantic
convention. You can name and organize classes sensibly, but only with
deliberation; they aren’t smart enough to take care of themselves, and browsers
aren’t smart enough to take care of them for you.

Unintelligent selectors are time-intensive because they require styling hooks to
be duplicated case by case. If we didn’t have p tags, we’d have to use
unintelligent selectors to manufacture paragraphs, perhaps using `.paragraph` in
each case. One of the downsides of this is that the CSS isn’t portable — that
is, you can’t apply it to an HTML document without first going through the
document and adding the classes everywhere they are required.

![paragraph][class selector called paragraph]

Unintelligent selectors at times seem necessary, or at least easier, and few of
us are willing to rely entirely on intelligent selectors. However, some
unintelligent selectors can become “plain stupid” selectors by creating a
**mismatch** between document structure and presentation. I’ll be talking about
the alarming frequency with which the unintelligent `.button` selector quickly
becomes plain stupid.

### Vive la Différence

Intelligent selectors are not confined just to the basic elements offered to us
in HTML’s specification. To build complex intelligent selectors, you can defer
to combinations of **context and functional attribution** to differentiate basic
elements. Some elements, such as `<a>`, have a multitude of functional
differences to consider and exploit. Other elements, such as `<p>`, rarely
differ in explicit function but assume slightly different roles according to
context.

    header p {
       /* styles for prologic paragraphs */
    }

    footer p {
       /* styles for epilogic paragraphs */
    }

Simple descendent selectors like these are extremely powerful because they
enable us to visually disclose different types of the same element without
having to physically alter the underlying document. This is the whole reason why
style sheets were invented: to facilitate physical separation without breaking
the conceptual reciprocity that should exist between document and design.

![heirarchy][a semantic heirarchy of needs: what it is, how it functions, where it is]

Inevitably, some adherents of OOCSS [treat the descendent selector with some
suspicion][5], with the more zealous insisting on markup such as the example
below, found in BEM’s “[Definitions][6]” documentation.

    <ul class="menu">
      <li class="menu__item">…</li>
      <li class="menu__item">…</li>
    </ul>

I won’t cover contextual selectors any further because, unless you have a
predilection for the kind of overprescription outlined above, I’m sure you
already use them every day. Instead, we’ll concentrate on differentiation by
function, as described in attributes and by **attribute selectors**.

## Hyperlink Attributes

Even those who advocate for conceptual separation between CSS and HTML are happy
to concede that some attributes — most attributes besides classes and custom
data attributes, in fact — have an important bearing on the internal functioning
of the document. Without `href`, your link won’t link to anything. Without
`type`, the browser won’t know what sort of `input` to render. Without `title`,
your `abbr` could be referring to either the British National Party or *Banco
Nacional de Panama*.

Some of these attributes may improve the semantic detail of your document, while
others are needed to ensure the correct rendering and functioning of their
subject elements. If they’re not there, they should be, and if they are there,
why not make use of them? You can’t write CSS without writing HTML.

### The `rel` Attribute

The `rel` attribute emerged as a standard for [link relations][7], a method of
describing some specific purpose of a link. Not all links, you see, are
functionally alike. Thanks to WordPress’ championing, `rel="prev"` and
`rel="next"` are two of the most widely adopted values, helping to describe the
relationship between individual pages of paginated blog content. Semantically,
an `a` tag with a `rel` attribute is still an `a` tag, but we are able to be
more specific. Unlike with classes, this specificity is semantically
consequential.

The rel attribute should be used where appropriate because it is vindicated by
HTML’s [functional specification][8] and can therefore be adopted by various
user agents to enhance the experience of users and the accuracy of search
engines. How, then, do you go about styling such links? With simple attribute
selectors, of course:

    [rel="prev"] {
      /* styling for "previous links" */
    }

    [rel="next"] {
      /* styling for "next" links */
    }

Attribute selectors like these are supported by all but the most archaic,
clockwork browsers, so that’s no reason not to use them anywhere the attributes
exist. In terms of specificity, they have the same weight as classes. No woe
there either, then. However, I recall it being suggested that we should decouple
document and presentation semantics. I don’t want to lose the rel attributes
(Google has [implemented them][9], for one thing), so I’d better put **an
attribute that means nothing** on there as well and style the element via that.

    <a href="/previous-article-snippet/" rel="prev" class="prev">previous page</a>

The first thing to note here is that the only part of the element above that
does not contribute to the document’s semantics is the class. The class, in
other words, is the only thing in the document that has nothing functionally to
do with it. In practice, this means that the class is the only thing that
**breaks with the very law of separation that it was employed to honor**: It has
a physical presence in the document without contributing to the document’s
structure.

OK, so much for abstraction, but what about maintenance? Accepting that we’ve
used the `class` as our styling hook, let’s now examine what happens when some
editing or refactoring has led us to remove some attributes. Suppose we’ve used
some pseudo-content to place a left-pointing arrow before the `[rel="prev"]`
link’s text:

    .prev:before {
      content: '\2190'; /* encoding for a left-pointing arrow ("←") */
    }

![arrow][previous link with arrow]

Removing the class will remove the pseudo-content, which in turn will remove the
arrow (obviously). But without the arrow, nothing remains to elucidate the
link’s extant prev relationship. By the same token, removing the `rel` attribute
will leave the arrow intact: The class will continue to manage presentation, all
the time disguising the **nonexistence of a stated relationship** in the
document. Only by applying the style directly, via the semantic attribute that
elicits it, can you keep your code and yourself honest and accurate. Only if
it’s really there, as a function of the document, should you see it.

## Attribute Substrings

I can imagine what you’re thinking: “That’s cute, but how many instances are
there really for semantic styling hooks like these on hyperlinks? I’m going to
have to rely on classes at some point.” I dispute that. Consider this incomplete
list of functionally disimilar hyperlinks, all using the `a` element as their
base:

* links to external resources,
* links to secure pages,
* links to author pages,
* links to help pages,
* links to previous pages (see example above),
* links to next pages (see example above again),
* links to PDF resources,
* links to documents,
* links to ZIP folders,
* links to executables,
* links to internal page fragments,
* links that are really buttons (more on these later),
* links that are really buttons and are toggle-able,
* links that open mail clients,
* links that cue up telephone numbers on smartphones,
* links to the source view of pages,
* links that open new tabs and windows,
* links to JavaScript and JSON files,
* links to RSS feeds and XML files.

That’s a lot of functional diversity, all of which is understood by user agents
of all sorts. Now consider that in order for all of these specific link types to
function differently, they must have mutually differential attribution. That is,
in order to function differently, they must be written differently; and if
they’re written differently, **they can be styled differently**.

In preparing this article, I created a proof of concept, named [Auticons][10].
Auticons is an [icon font][11] and CSS set that styles links automatically. All
of the selectors in the CSS file are attribute selectors that invoke styles on
well-formed hyperlinks, **without the intervention of classes**.

![auticons][art_auticons]

In many cases, Auticons queries a subset of the `href` value in order to
determine the function of the hyperlink. Styling elements according to the way
their attribute values begin or end or according to what substring they contain
throughout the value is possible. Below are some common examples.

### The Secure Protocol

Every well-formed (i.e. absolute) URL begins with a [URI scheme][12] followed by
a colon. The most common on the Web is `http:`, but `mailto:` (for SMTP) and
`tel:` (which refers to telephone numbers) are also prevalent. If we know how
the `href` value of the hyperlink is expected to begin, we can exploit this
semantic convention as a styling hook. In the following example for secure
pages, we use the `^=` comparator, which means “begins with.”

    a[href^="https:"] {
       /* style properties exclusive to secure pages */
    }

![lock][a link to a secure page with a lock icon]

In Auticons, links to secure pages become adorned with a padlock icon according
to a specific **semantic pattern**, identifiable within the `href` attribute.
The advantages of this are as follows:

* Links to secure pages — and only secure pages — are able to resemble links to 
secure pages by way of the padlock icon.
* Links to secure pages that cease to be true links to secure pages will lose 
the https protocol and, with it, the resemblance.
* New secure pages will adopt the padlock icon and resemble links to secure 
pages automatically.

This selector becomes truly intelligent when applied to **dynamic content**.
Because secure links exist as secure links even in the abstract, the attribute
selector can anticipate their invocation: As soon as an editor publishes some
content that contains a secure link, the link resembles a secure one to the
user. No knowledge of class names or complex HTML editing is required, so even
simple [Markdown][13] will create the style:

    [Link to secure page](https://payment.example.com/)

Note that using the `[href^="https:"]` prefix is not infallible because not all
HTTPS pages are truly secure. Nonetheless, it is only as fallible as the browser
itself. Major browsers all render a padlock icon natively in the address bar
when displaying HTTPS pages.

![PayPal][PayPal secure page]

### File Types

As promised, you can also style hyperlinks according to how their `href` value
ends. In practice, this means you can use CSS to indicate what **type of file**
the link refers to. Auticons supports `.txt`, `.pdf`, `.doc`, `.exe` and many
others. Here is the `.zip` example, which determines what the `href` ends with,
using `$=`:

    [href$=".zip"]:before, 
    [href$=".gz"]:before {
       content: '\E004'; /* unicode for the zip folder icon */
    }

### Combinations

You know how you can get all **object-oriented** and use a selection of multiple
classes on elements to build up styles? Well, you can do that automatically with
attribute selectors, too. Let’s compare:

    /* The CSS for the class approach */

    .new-window-icon:after {
       content: '[new window icon]';
    }

    .twitter-icon:before {
      content: '[twitter icon]';
    }

    /* The CSS for the attribute selector approach */

    [target="_blank"]:after {
       content: '[new window icon]';
    }

    [href*="twitter.com/"]:before {
      content: '[twitter icon]';
    }

(Note the `*=` comparator, which means “contains.” If the value string contains 
the substring `twitter.com/`, then the style will be honored.)

    <!-- The HTML for the class approach -->

    <a href="http://twitter.com/heydonworks" target="_blank" class="new-window-icon twitter-icon">@heydonworks</a>

    <!-- The HTML for the attribute selector approach -->

    <a href="http://twitter.com/heydonworks" target="_blank">@heydonworks</a>

![twitter][A twitter link with icons to show that it goes to twitter and is external]

Any content editor charged with adding a link to a Twitter page now needs to
know only two things: the URL (they probably know the Twitter account already)
and how to open links in new tabs (obtainable from a quick Google search).

### Inheritance

Some unfinished business: What if we have a link that does not match any of our
special attribute selectors? What if a hyperlink is just a plain old hyperlink?
The selector is an easy one to remember, and performance fanatics will be
pleased to hear that it couldn’t be any terser without existing at all.

![a][A basic anchor selector (a)]

Flippancy aside, let me assure you that inheritance within the cascade works
with attribute selectors just as it does with classes. First, style your basic
`a` — perhaps with a `text-decoration: underline` rule to keep things
accessible; then, progressively enhance further down the style sheet, using the
attribute selectors at your disposal. Browsers such as Internet Explorer (IE) 7
do not support pseudo-content at all. Thanks to inheritance, at least the links
will still look like links.

    a {
      color: blue;
      text-decoration: underline;
    }

    a[rel="external"]:after {
       content: '[icon for external links]';
    }

## Actual Buttons Are Actual

In the following section, we’ll detail the construction of our CSS bookmarklet
for reporting code errors. Before doing this, let’s look at how plain stupid
selectors can creep into our workflow in the first place.

Adherents of OOCSS are keen on classes because they can be reused, as
components. Hence, `.button` is preferable to `#button`. I can think of one
better component selector for button styles, though. Its name is easy to
remember, too.

![button][button element selector]

> The `<button>` element represents a button.

> – [W3C Wiki][14]

[Topcoat][15] is an OOCSS BEM-based UI framework from Adobe. The CSS for 
Topcoat’s various button styles is **more than 450 lines** if you include the 
comment blocks. Each of these comment blocks suggests applying your button style 
in a manner similar to this introductory example:

    <a class="topcoat-button">Button</a>

This example is not a button. No, sir. If it were a button, it would be marked
up using `<button>`. In fact, in every single browser known to man, if it were
marked up as a button and no author CSS was supplied, you could count on it
looking like a button by default. It’s not, though; it’s marked up using `<a>`,
which makes it a hyperlink — a hyperlink, in fact, that lacks an `href`, meaning
it [isn’t even a hyperlink][16]. Technically, it’s just a [placeholder][17] for
a hyperlink that you haven’t finished writing yet.

![shark][Dog in a shark costume]

A dog in a shark costume does not a shark make. (Image: [reader of the pack][18])

The examples in Topcoat’s CSS are only examples, but the premise that the class
defines the element and not the HTML is deceptive. No amount of class name
modification via “meaningful hyphenation” can make up for this invitation to
turn your unintelligent selector into a plain stupid one and to just **code
stuff wrong**.

**Update:** Since writing this article, Topcoat.io has replaced these examples 
with `<button>` examples. This is great! However, I still have my reservations 
about the way the examples expound the use of the `.is-disabled` class while 
omitting the proper `disabled` attribute. To hear both sides, find my 
conversation with the Topcoat representative in the comments. For further 
examples of OOCSS-facilitated web standards mishaps, look no further than 
[semantic-ui.com][19]. The “standard button” in their examples is a `<div>` 
containing an empty `<i>`.

### See No Evil, Hear No Evil

> “If it looks like a duck, swims like a duck, and quacks like a duck, then it 
probably is a duck.”

A link that resembles a button and triggers button-like JavaScript events is, to
many, a button. However, this only means that it has passed the first two stages
of the “duck test.” For all users to be able to apply inductive reasoning and
discern the button as such, it must also quack like one. Because it remains a
link, it will be announced by screen readers as “link,” meaning that your
allegorical skyscraper is not wheelchair-accessible. Avoiding this kind of
unnecessary confusion for assistive technology users is not a pursuit of
semantic perfection but a responsibility we should undertake for real benefit.

Nonetheless, some will insist on using `a` as the basis of their buttons.
Hyperlinks are (slightly) easier to restyle consistently, after all. If `a` is
the element of choice, then there is only one way to make it a close-to-true
button in the accessibility layer. You guessed it: You must apply another
meaningful attribute in the form of a [WAI ARIA][20] role. To ensure that a
hyperlink element looks like a button for good reason, apply only the following
attribute selector.

    [role="button"] {
       /* semantic CSS for modified elements that are announced as “button” in assistive technologies */
    }

## Quality Assurance With Attribute Selectors

> “CSS gives so much power to the class attribute, that authors could 
conceivably design their own “document language” based on elements with almost 
no associated presentation (such as DIV and SPAN in HTML) and assigning style 
information through the “class” attribute. Authors should avoid this practice 
since the structural elements of a document language often have recognized and 
accepted meanings.”

> – “[Selectors][21],” CSS Level 2, W3C

The reason we have two elements — `a` and `button` — is to semantically
demarcate two entirely different types of functional interaction. While the
hyperlink denotes a means to go somewhere, the button is intended as the
instigator of an event or action. One is about traversal, the other about
transformation. One facilitates disengagement, the other engagement.

To make sure we don’t do anything too daft and get our links and buttons muddled
up, we will now build a CSS bookmarklet that uses intelligent attribute
selectors to test the validity and quality of the two respective elements.

Inspired partly by [Eric Meyer’s post][22] and taking a few cues from
[DiagnostiCSS][23], this style sheet will combine attribute selectors and the
:not selector (or [negation pseudo-class][24]) to highlight problems in the
HTML. Unlike these other two implementations, it will write an error to the
screen using pseudo-content. Each error will be written in **Comic Sans**
against a pink background.

By connecting function directly to form, we see that ugly markup results in ugly
CSS. Consider this the revenge of an abused document, exacted on its designer.
To try it out, drag `revenge.css` to your bookmarks, and click the bookmark to
trigger it on any page that you fancy. **Note:** It won’t currently work for
pages that are served over https.

<a style="font-size: 2em; padding: 1em; background: pink; font-family: 'comic sans ms', cursive; display: inline-block; color: #000;" href="javascript:(function(){revenge=document.createElement('LINK');revenge.href='//www.heydonworks.com/css/revenge_buttons.css';revenge.rel='stylesheet';revenge.media='all';document.body.appendChild(revenge);})();" title="REVENGE.CSS">REVENGE.CSS</a>

*Drag to your bookmarks bar.*

### Rule 1

> “If it’s a hyperlink, it should have an `href` attribute.”

    a:not([href]):after {
       content: 'Do you mean for this to be a link or a button, because it does not link to anything!';
       display: block !important;
       background: pink !important;
       padding: 0.5em !important;
       font-family: 'comic sans ms', cursive !important;
       color: #000 !important;
       font-size: 16px !important;
    }

**Notes:** In this example, we are testing not the attribute’s value, but 
whether the attribute exists in the first place — that is, whether `[href]` 
matches any element with an `href` attribute. This test is only appropriate on 
hyperlinks, hence the `a` prefix. The rule reads like, “For every `a` element 
that is not also an `[href]` element, append some pseudo-content with an error 
notice.”

### Rule 2

> “If it’s a hyperlink and has an href attribute, it should have a valid value.”

    a[href=""]:after, a[href$="#"]:after, a[href^="javascript"]:after {
       content: 'Do you mean for this link to be a button, because it does not go anywhere!';
       /*... ugly styles ...*/
    }

**Notes:** If the `href` is empty, ends in a `#` or is using JavaScript, it’s 
probably being used as a button without the correct `button` element. Note that 
I am using “starts with `javascript`.” Standard practice for voiding `href`s is 
to use `javascript:void(0)`, but we can’t depend on that always being written in 
the same way (with or without a space after the colon, for example).

### Rule 3

> “If it uses a button class, it should be a button — at least in the 
accessibility layer.”

    .button:not(button):not([role="button"]):not([type="button"]):not([type="submit"]):not([type="reset"]):after,
    .btn:not(button):not([role="button"]):not([type="button"]):not([type="submit"]):not([type="reset"]):after, 
    a[class*="button"]:not([role="button"]):after {
       content: 'If you are going to make it look like a button, make it a button, damn it!';
       /*... ugly styles ...*/
    }

**Notes:** In this example, we’re demonstrating how you can chain negation when 
testing for attributes. Each selector reads like this: “If the element has a 
class that says it’s a button but it’s not a `button` element and it doesn’t 
have the correct role to make it a button in the accessibility layer and it’s 
not an input being used as a button, then… well, you’re lying.” I’ve had to use 
`[class*="button"]` to catch the many Topcoat class variations (62 in total!) 
that fail to enforce actual buttons on hyperlinks. I’ve noticed that some 
authors use `button-container` and the like on parent elements, which is why the 
`a` qualifier is included to avoid false positives. You may recognize the `.btn` 
class from Twitter Bootstrap, which (if you’ve read the [component’s 
documentation][25] carefully) you’ll know is also unsure about whether links or 
buttons are buttons.

### Rule 4

> “If it is an a element with role="button", then it should link to somewhere 
when JavaScript is off.”

    a[role="button"]:not([href*="/"]):not([href*="."]):not([href*="?"]):after {
       content: 'Either use a link fallback, or just use a button element.';
       /*... ugly styles ...*/
    }

**Notes:** We can be fairly sure that `href`s that do not include one of `/`, 
`.` (usually to precede a file extension) or `?` (to start a query string) are 
probably bogus. Getting links to act as buttons and `return: false` when 
JavaScript is on is fine — fine, that is, if they have a page to go to when 
JavaScript is off. In fact, it’s the only legitimate reason I can think of not 
to use `<button>` instead.

### Rule 5

> “You can’t disable a hyperlink.”

    a.button[class*="disabled"]:after, 
    a.btn.disabled:after,
    a[class*="button"][class*="disabled"]:after {
       content: 'You cannot disable a hyperlink. Use a button element with disabled="disabled".';
       /*... ugly styles ...*/
    }

**Notes:** Even ancient user agents understand the `disabled` attribute, so use 
it with an appropriate element in a compliant way. You can concatenate attribute 
selectors just as you can concatenate classes: In the last of the three 
selectors, we’re saying, “If it’s a link that contains the substring `button` 
and the substring `disabled`, then print an error message.” Twitter Bootstrap 
uses the second form, `.btn.disabled`, in its style sheet, but not with the `a` 
prefix. We’ll only consider it an error if used on hyperlinks.

### Rule 6

> “Buttons in forms should have explicit types.”

    form button:not([type]):after {
    	content: 'Is this a submit button, a reset button or what? Use type="submit", type="reset" or type="button"';
    }

**Notes:** We need to determine whether `buttons` within forms have explicit 
types, because [some browsers][26] will treat any `button` in this context 
without a specified type as `type="submit"`. We want to be absolutely sure that 
the form won’t submit if our button has a different purpose.

### Rule 7

> “Both hyperlinks and buttons should have some sort of content or an ARIA 
label.”

    a:empty:not([aria-label]):not([aria-labelledby]):after, 
    button:empty:not([aria-label]):not([aria-labelledby]):after, 
    button:not([aria-label]):not([aria-labelledby]) img:only-child:not([alt]):after, 
    a:not([aria-label]):not([aria-labelledby]) img:only-child:not([alt]):after {
       content: 'All buttons and links should have text content, an image with alt text or an ARIA label';
       /*... ugly styles ...*/
    }

**Notes:** Buttons and links that don’t include any kind of direction for their 
usage — in either textual or graphical form — are pretty bogus. These final two 
selectors are perhaps the most complex I’ve ever written. For the hyperlink 
version, the selector reads something like this: “If it is a hyperlink that does 
not have either an `aria-label` attribute or an `aria-labelledby` attribute and 
it contains only an image as content but this image does not have an `alt` 
attribute, then write the ugly error message.” Also, note the use of the 
[`:empty` selector][27]. Arguably, no element that is not self-closing should 
ever be left empty.

Ten points to the first person using **revenge.css** who can tell me where I’ve 
broken my own rule in this very article. Trust me, the error is definitely there.

## Conclusion

The reason I use the kinds of selectors and patterns described above is not to
try something different or to have something new to write about. Attribute
selectors aren’t, by themselves, anything new anyway. IE 6 is the [only
browser][28] that doesn’t support them. The reason I use them is because I
simply do not have the time or mental capacity to “do” HTML and CSS in parallel.
My brain just isn’t good enough for that. The reason I style my page headers
with `[role="banner"]` and not `.page-header` is because that’s the only way
I’ll know — upon seeing the intended visual effect — that I’ve put the
[navigable landmark][29] in place. How else does one keep track? You can’t just
leave it to testing, because then it’s usually too late.

**There’s no such thing as semantic CSS**. There’s only semantic HTML and its 
visible form. In this article I have tried to demonstrate that, by coupling the 
function and form of Web pages directly, you can create mechanisms for reward 
and punishment. On the one hand, you can set up selectors that invoke visual 
motifs only when the suitable markup is used. On the other hand, you can query 
the markup for bad patterns and erode the visual design as a commitment to the 
underlying ugly truth.

It’s true that not all the styling hooks in your arsenal will likely ever be
uniformly semantic or intelligent. Classes are often desirable as polyfills for
much needed elements or attributes that have yet to be standardized. That’s how
`.footer` became `<footer>` and `type="text"` (with a bunch of JavaScript)
became `type="url"`. Other times, they are helpful for doing non-semantic layout
scaffolding with [grid frameworks][30] and the like.

However, if you are committed to giving CSS its own completely separate logic,
then you are bound to create unnecessary arguments between form and function. In
this eventuality, only constant vigilance can protect against inaccessibility
and invalidity. To make matters worse, trying to manufacture pseudo-semantics
purely with classes makes it easy to fall into one of those interminable
discussions over [what makes a semantic class name][31]. You’ll start spending
less time using the remote to control the TV and more time just sitting there,
contemplating the remote control held in your hand.

Life is too short.

[1]: http://en.wikipedia.org/wiki/Louis_Sullivan
[2]: http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/
[3]: http://nicolasgallagher.com/about-html-semantics-front-end-architecture/
[4]: http://www.w3.org/TR/CSS2/selector.html#class-html
[5]: http://www.impressivewebs.com/when-to-avoid-descendant-selector/
[6]: http://bem.info/method/definitions/
[7]: http://blog.whatwg.org/the-road-to-html-5-link-relations#what
[8]: http://en.wikipedia.org/wiki/Functional_specification
[9]: http://googlewebmastercentral.blogspot.co.uk/2011/09/pagination-with-relnext-and-relprev.html
[10]: http://heydonworks.com/auticons-icon-font/
[11]: http://sixrevisions.com/resources/free-icon-fonts/
[12]: http://en.wikipedia.org/wiki/URI_scheme
[13]: http://daringfireball.net/projects/markdown/syntax#link
[14]: http://www.w3.org/wiki/HTML/Elements/button
[15]: http://topcoat.io/
[16]: http://www.whatwg.org/specs/web-apps/current-work/multipage/links.html#attr-hyperlink-href
[17]: http://www.w3.org/wiki/HTML/Elements/a
[18]: http://www.flickr.com/photos/youngandwithit/
[19]: http://semantic-ui.com/elements/button.html
[20]: http://www.w3.org/WAI/intro/aria
[21]: http://www.w3.org/TR/CSS2/selector.html#class-html
[22]: http://meyerweb.com/eric/tools/css/diagnostics/
[23]: https://github.com/diagnosticss/diagnosticss
[24]: http://dev.w3.org/csswg/selectors3/#negation
[25]: http://getbootstrap.com/2.3.2/#buttonDropdowns
[26]: http://stackoverflow.com/questions/932653/how-to-prevent-buttons-from-submitting-forms
[27]: https://developer.mozilla.org/en-US/docs/Web/CSS/:empty
[28]: http://www.quirksmode.org/css/selectors/
[29]: http://blog.paciellogroup.com/2013/07/enabling-landmark-based-keyboard-navigation-in-firefox/
[30]: http://unsemantic.com/
[31]: http://css-tricks.com/semantic-class-names/

[Overthinking how a functional thing should look.]: img/art_overthink1_mini.png
[television remote reading DRIVEL]: img/art_remote_mini.png
[class selector called paragraph]: img/art_paragraph_500_mini.png
[a semantic heirarchy of needs: what it is, how it functions, where it is]: img/art_heirarchy_500_mini.png
[previous link with arrow]: img/art_previous_500_mini.png
[art_auticons]: img/art_auticons_mini.png
[a link to a secure page with a lock icon]: img/art_secure_500_mini.png
[PayPal secure page]: img/paypal_500_mini.png
[A twitter link with icons to show that it goes to twitter and is external]: img/art_twitter_500_mini.png
[A basic anchor selector (a)]: img/art_a_mini.png
[button element selector]: img/art_button_mini.png
[Dog in a shark costume]: img/shark_dog_mini.jpg