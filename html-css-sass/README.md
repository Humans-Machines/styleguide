# Humans & Machines HTML/CSS/SASS Style Guide

inspired by Google, AirBnB and cssguidelin.es

### General Formatting Rules

#### Indentation

Indent by 4 spaces at a time.

Don’t use tabs – unless your editor replaces them with whitespace – or mix tabs and spaces for indentation.

**Do**

    <ul>
        <li>Fantastic
        <li>Great
    </ul>

    .example {
        color: blue;
    }

#### Capitalization

Use only lowercase. Or mixed cases for copy.

All code has to be lowercase: This applies to HTML element names, attributes, attribute values (unless `text/CDATA`), CSS selectors, properties, and property values (with the exception of strings).
Also **never** use uppercase for copy text. If it needs to be uppercase use CSS, stupid!

**Don’t**

    <A HREF="/">HOME</A>

    color: #E5E5E5;

**Do**

    <a href="/">Home</a>
    
    color: #e5e5e5;

### General Meta Rules

#### Encoding

Use UTF-8 (no BOM).

Make sure your editor uses UTF-8 as character encoding, without a byte order mark.

Specify the encoding in HTML templates and documents via `<meta charset="utf-8">`. Do not specify the encoding of style sheets as these assume UTF-8.

#### Comments

Explain code as needed, where possible. 

Use comments to explain code: 

- What does it cover?
- What purpose does it serve?
- Why is respective solution used or preferred?
- Whether some CSS relies on other code elsewhere?
- what effect changing some code will have elsewhere?

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that color: red; will make something red, but if you’re using overflow: hidden; to clear floats—as opposed to clipping an element’s overflow—this is probably something worth documenting.

##### High-level

For large comments that document entire sections or components, we use a DocBlock-esque multi-line comment which adheres to our 80 column width.

**Do**

```css
/**
 * The site’s main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
```
 
This level of detail should be the norm for all non-trivial code—descriptions of states, permutations, conditions, and treatments.

##### Object–Extension Pointers

When working across multiple partials, or in an OOCSS manner, you will often find that rulesets that can work in conjunction with each other are not always in the same file or location. For example, you may have a generic button object—which provides purely structural styles—which is to be extended in a component-level partial which will add cosmetics. We document this relationship across files with simple object–extension pointers. In the object file:

```css
/**
 * Extend `.btn {}` in _components.buttons.scss.
 */

.btn { }
```

And in your theme file:

```css
/**
 * These rules extend `.btn {}` in _objects.buttons.scss.
 */

.btn--positive { }

.btn--negative { }
```

This simple, low effort commenting can make a lot of difference to developers who are unaware of relationships across projects, or who are wanting to know how, why, and where other styles might be being inherited from.

##### Low-level

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset. To do this we use a kind of reverse footnote. Here is a more complex comment detailing the larger site headers mentioned above:

```
/**
 * Large site headers act more like mastheads. They have a faux-fluid-height
 * which is controlled by the wrapping element inside it.
 *
 * 1. Mastheads will typically have dark backgrounds, so we need to make sure
 *    the contrast is okay. This value is subject to change as the background
 *    image changes.
 * 2. We need to delegate a lot of the masthead’s layout to its wrapper element
 *    rather than the masthead itself: it is to this wrapper that most things
 *    are positioned.
 * 3. The wrapper needs positioning context for us to lay our nav and masthead
 *    text in.
 * 4. Faux-fluid-height technique: simply create the illusion of fluid height by
 *    creating space via a percentage padding, and then position everything over
 *    the top of that. This percentage gives us a 16:9 ratio.
 * 5. When the viewport is at 758px wide, our 16:9 ratio means that the masthead
 *    is currently rendered at 480px high. Let’s…
 * 6. …seamlessly snip off the fluid feature at this height, and…
 * 7. …fix the height at 480px. This means that we should see no jumps in height
 *    as the masthead moves from fluid to fixed. This actual value takes into
 *    account the padding and the top border on the header itself.
 */

.page-head--masthead {
  margin-bottom: 0;
  background: url(/img/css/masthead.jpg) center center #2e2620;
  @include vendor(background-size, cover);
  color: $color-masthead; /* [1] */
  border-top-color: $color-masthead;
  border-bottom-width: 0;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1) inset;

  @include media-query(lap-and-up) {
    background-image: url(/img/css/masthead-medium.jpg);
  }

  @include media-query(desk) {
    background-image: url(/img/css/masthead-large.jpg);
  }

  > .wrapper { /* [2] */
    position: relative; /* [3] */
    padding-top: 56.25%; /* [4] */

    @media screen and (min-width: 758px) { /* [5] */
      padding-top: 0; /* [6] */
      height: $header-max-height - double($spacing-unit) - $header-border-width; /* [7] */
    }

  }

}
```

These types of comment allow us to keep all of our documentation in one place whilst referring to the parts of the ruleset to which they belong.

##### Preprocessor Comments

With most—if not all—preprocessors, we have the option to write comments that will not get compiled out into our resulting CSS file. As a rule, use these comments to document code that would not get written out to that CSS file either. If you are documenting code which will get compiled, use comments that will compile also. For example, this is correct:

    // Dimensions of the @2x image sprite:
    $sprite-width:  920px;
    $sprite-height: 212px;
    
    /**
     * 1. Default icon size is 16px.
     * 2. Squash down the retina sprite to display at the correct size.
     */
    .sprite {
      width:  16px; /* [1] */
      height: 16px; /* [1] */
      background-image: url(/img/sprites/main.png);
      background-size: ($sprite-width / 2 ) ($sprite-height / 2); /* [2] */
    }

We have documented variables—code which will not get compiled into our CSS file—with preprocessor comments, whereas our CSS—code which will get compiled into our CSS file—is documented using CSS comments. This means that we have only the correct and relevant information available to us when debugging our compiled stylesheets.


### Comments by AirBnB

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

##### Removing Comments
It should go without saying that no comments should make their way into production environments—all CSS should be minified, resulting in loss of comments, before being deployed.

#### Action Items

Mark todos and action items with `TODO`.

Highlight todos by using the keyword `TODO` only, not other common formats like `@@`.

Append a contact (username or mailing list) in parentheses as with the format `TODO(contact)`.

Append action items after a colon as in `TODO: action item`.

    {# TODO(john.doe): revisit centering #}
    <center>Test</center>

    <!-- TODO: remove optional tags -->
    <ul>
      <li>Apples</li>
      <li>Oranges</li>
    </ul>

## HTML

### HTML Style Rules

#### Document Type

Use HTML5.

HTML5 (HTML syntax) is preferred for all HTML documents: `<!DOCTYPE html>`.

(It’s recommended to use HTML, as `text/html`. Do not use XHTML. XHTML, as [`application/xhtml+xml`](https://hixie.ch/advocacy/xhtml), lacks both browser and infrastructure support and offers less room for optimization than HTML.)

Although fine with HTML, do not close void elements, i.e. write `<br>`, not `<br />`.

#### HTML Validity

Use valid HTML where possible.

Use valid HTML code unless that is not possible due to otherwise unattainable performance goals regarding file size.

Use tools such as the [W3C HTML validator](https://validator.w3.org/nu/) to test.

Using valid HTML is a measurable baseline quality attribute that contributes to learning about technical requirements and constraints, and that ensures proper HTML usage.

**Don’t**

    <title>Test</title>
    <article>This is only a test.

**Do**

    <!DOCTYPE html>
    <meta charset="utf-8">
    <title>Test</title>
    <article>This is only a test.</article>
    
#### Separation of Concerns

Separate structure from presentation from behavior.

Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum.

That is, make sure documents and templates contain only HTML and HTML that is solely serving structural purposes. Move everything presentational into style sheets, and everything behavioral into scripts.    

#### Semantics

Use HTML according to its purpose.

Use elements (sometimes incorrectly called “tags”) for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc.

Using HTML according to its purpose is important for accessibility, reuse, and code efficiency reasons.

**Don’t**

    <div onclick="goToRecommendations();">All recommendations</div>

**Do**

    <a href="recommendations/">All recommendations</a>

#### Multimedia Fallback

Provide alternative contents for multimedia.

For multimedia, such as images, videos, animated objects via `canvas`, make sure to offer alternative access. For images that means use of meaningful alternative text (`alt`) and for video and audio transcripts and captions, if available.

Providing alternative contents is important for accessibility reasons: A blind user has few cues to tell what an image is about without `@alt`, and other users may have no way of understanding what video or audio contents are about either.

For images whose `alt` attributes would introduce redundancy, and for images whose purpose is purely decorative, use no alternative text, as in `alt=""`.

**Don’t**
 
    <img src="spreadsheet.png">

**Do**

    <img src="spreadsheet.png" alt="Spreadsheet screenshot">


#### `type` Attributes

Omit `type` attributes for style sheets and scripts.

Do not use `type` attributes for style sheets (unless not using CSS) and scripts (unless not using JavaScript).

Specifying `type` attributes in these contexts is not necessary as HTML5 implies [`text/css`](https://html.spec.whatwg.org/multipage/obsolete.html#attr-style-type) and [`text/javascript`](https://html.spec.whatwg.org/multipage/scripting.html#attr-script-type) as defaults. This can be safely done even for older browsers.

**Don’t**

    <link rel="stylesheet" href="https://www.google.com/css/maia.css"
        type="text/css">
      
    <script src="https://www.google.com/js/gweb/analytics/autotrack.js"
        type="text/javascript"></script>

**Do**

    <link rel="stylesheet" href="https://www.google.com/css/maia.css">
    
    <script src="https://www.google.com/js/gweb/analytics/autotrack.js"></script>

### HTML Formatting Rules

#### General Formatting

Use a new line for every block, list, or table element, and indent every such child element.

Independent of the styling of an element (as CSS allows elements to assume a different role per `display` property), put every block, list, or table element on a new line.

Also, indent them if they are child elements of a block, list, or table element.

    <blockquote>
      <p><em>Space</em>, the final frontier.</p>
    </blockquote>

    <ul>
      <li>Moe</li>
      <li>Larry</li>
      <li>Curly</li>
    </ul>

#### HTML Line-Wrapping

Break long lines (optional).

While there is no column limit recommendation for HTML, you may consider wrapping long lines if it significantly improves readability.

When line-wrapping, each continuation line should be indented at least 4 additional spaces from the original line.

    <md-progress-circular md-mode="indeterminate" class="md-accent"
        ng-show="ctrl.loading" md-diameter="35">
    </md-progress-circular>

    <md-progress-circular
        md-mode="indeterminate"
        class="md-accent"
        ng-show="ctrl.loading"
        md-diameter="35">
    </md-progress-circular>

    <md-progress-circular md-mode="indeterminate"
                          class="md-accent"
                          ng-show="ctrl.loading"
                          md-diameter="35">
    </md-progress-circular>

#### HTML Quotation Marks

When quoting attributes values, use double quotation marks.

Use double (`""`) rather than single quotation marks (`''`) around attribute values.

**Don’t**

    <a class='maia-button maia-button-secondary'>Sign in</a>

**Do**

    <a class="maia-button maia-button-secondary">Sign in</a>

## CSS

### Terminology

#### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

#### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

Perhaps somewhat surprisingly, one of the most fundamental, critical aspects of writing maintainable and scalable CSS is selectors. Their specificity, their portability, and their reusability all have a direct impact on the mileage we will get out of our CSS, and the headaches it might bring us.

#### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

### CSS Style Rules

#### ID and Class Naming

- Prefer dashes over camelCasing in class names and make use of [BEM](#bem)
- Use meaningful or generic ID and class names
- Use ID and class names that are as short as possible but as long as necessary


**Don’t**

    /* Don’t: meaningless */
    #yee-1901 {}

    /* Don’t: presentational */
    .button-green {}
    .clear {}
    
    /* Don’t: too long, not readable */    
    #navigation {}
    .atr {}


**Do**

    /* Do: specific */
    #gallery {}
    #login {}
    .video {}

    /* Do: generic */
    .aux {}
    .alt {}
    
    /* Do: short, readable */
    #nav {}
    .author {}

#### Selectors

- Never style IDs, style type selectors only for reset purposes
- Avoid qualifying class names with type selectors
- Use combined selectors wisely
- Avoiding unnecessary ancestor selectors is useful for performance reasons

**Don’t**


    /* Don’t: Use of IDs for styling */
    #header-nav {}
    
    /* Don’t: qualifying ID and class names with type selectors */
    div.error {}
    
    /* Don’t: Use type selectors for specific styles: poor Selector Intent */
    header ul { 
        // styles for header navi
    }


**Do**
    
    /* Do: Use classes instead of IDs or type selectors */
    .header__nav {
        // styles for header navi
    }
    
    /* Do: Use type selectors for resets */
    h1,
    h2,
    h3,
    h4 {
        font-weight: 100;
    }
    
    a {
        color: inherit;
    }

    


The difference in speed between an ID and a class is almost totally irrelevant. But IDs are much too specific to be used in styling CSS.

https://csswizardry.com/2011/09/writing-efficient-css-selectors/

# info on selectors

Perhaps somewhat surprisingly, one of the most fundamental, critical aspects of writing maintainable and scalable CSS is selectors. Their specificity, their portability, and their reusability all have a direct impact on the mileage we will get out of our CSS, and the headaches it might bring us.

It is important when writing CSS that we scope our selectors correctly, and that we’re selecting the right things for the right reasons. Selector Intent is the process of deciding and defining what you want to style and how you will go about selecting it. For example, if you are wanting to style your website’s main navigation menu, a selector like this would be incredibly unwise:

header ul { }
This selector’s intent is to style any ul inside any header element, whereas our intent was to style the site’s main navigation. This is poor Selector Intent: you can have any number of header elements on a page, and they in turn can house any number of uls, so a selector like this runs the risk of applying very specific styling to a very wide number of elements. This will result in having to write more CSS to undo the greedy nature of such a selector.

A better approach would be a selector like:

.site-nav { }
An unambiguous, explicit selector with good Selector Intent. We are explicitly selecting the right thing for exactly the right reason.

Poor Selector Intent is one of the biggest reasons for headaches on CSS projects. Writing rules that are far too greedy—and that apply very specific treatments via very far reaching selectors—causes unexpected side effects and leads to very tangled stylesheets, with selectors overstepping their intentions and impacting and interfering with otherwise unrelated rulesets.

CSS cannot be encapsulated, it is inherently leaky, but we can mitigate some of these effects by not writing such globally-operating selectors: your selectors should be as explicit and well reasoned as your reason for wanting to select something.

Reusability
With a move toward a more component-based approach to constructing UIs, the idea of reusability is paramount. We want the option to be able to move, recycle, duplicate, and syndicate components across our projects.

To this end, we make heavy use of classes. IDs, as well as being hugely over-specific, cannot be used more than once on any given page, whereas classes can be reused an infinite amount of times. Everything you choose, from the type of selector to its name, should lend itself toward being reused.

Location Independence
Given the ever-changing nature of most UI projects, and the move to more component-based architectures, it is in our interests not to style things based on where they are, but on what they are. That is to say, our components’ styling should not be reliant upon where we place them—they should remain entirely location independent.

Let’s take an example of a call-to-action button that we have chosen to style via the following selector:

.promo a { }
Not only does this have poor Selector Intent—it will greedily style any and every link inside of a .promo to look like a button—it is also pretty wasteful as a result of being so locationally dependent: we can’t reuse that button with its correct styling outside of .promo because it is explicitly tied to that location. A far better selector would have been:

.btn { }
This single class can be reused anywhere outside of .promo and will always carry its correct styling. As a result of a better selector, this piece of UI is more portable, more recyclable, doesn’t have any dependencies, and has much better Selector Intent. A component shouldn’t have to live in a certain place to look a certain way.

Portability
Reducing, or, ideally, removing, location dependence means that we can move components around our markup more freely, but how about improving our ability to move classes around components? On a much lower level, there are changes we can make to our selectors that make the selectors themselves—as opposed to the components they create—more portable. Take the following example:

input.btn { }
This is a qualified selector; the leading input ties this ruleset to only being able to work on input elements. By omitting this qualification, we allow ourselves to reuse the .btn class on any element we choose, like an a, for example, or a button.

Qualified selectors do not lend themselves well to being reused, and every selector we write should be authored with reuse in mind.

Of course, there are times when you may want to legitimately qualify a selector—you might need to apply some very specific styling to a particular element when it carries a certain class, for example:

/**
 * Embolden and colour any element with a class of `.error`.
 */
.error {
  color: red;
  font-weight: bold;
}

/**
 * If the element is a `div`, also give it some box-like styling.
 */
div.error {
  padding: 10px;
  border: 1px solid;
}
This is one example where a qualified selector might be justifiable, but I would still recommend an approach more like:

/**
 * Text-level errors.
 */
.error-text {
  color: red;
  font-weight: bold;
}

/**
 * Elements that contain errors.
 */
.error-box {
  padding: 10px;
  border: 1px solid;
}
This means that we can apply .error-box to any element, and not just a div—it is more reusable than a qualified selector.

Quasi-Qualified Selectors
One thing that qualified selectors can be useful for is signalling where a class might be expected or intended to be used, for example:

ul.nav { }
Here we can see that the .nav class is meant to be used on a ul element, and not on a nav. By using quasi-qualified selectors we can still provide that information without actually qualifying the selector:

/*ul*/.nav { }
By commenting out the leading element, we can still leave it to be read, but avoid qualifying and increasing the specificity of the selector.

Naming
As Phil Karlton once said, There are only two hard things in Computer Science: cache invalidation and naming things.

I won’t comment on the former claim here, but the latter has plagued me for years. My advice with regard to naming things in CSS is to pick a name that is sensible, but somewhat ambiguous: aim for high reusability. For example, instead of a class like .site-nav, choose something like .primary-nav; rather than .footer-links, favour a class like .sub-links.

The differences in these names is that the first of each two examples is tied to a very specific use case: they can only be used as the site’s navigation or the footer’s links respectively. By using slightly more ambiguous names, we can increase our ability to reuse these components in different circumstances.

To quote Nicolas Gallagher:

Tying your class name semantics tightly to the nature of the content has already reduced the ability of your architecture to scale or be easily put to use by other developers.

That is to say, we should use sensible names—classes like .border or .red are never advisable—but we should avoid using classes which describe the exact nature of the content and/or its use cases. Using a class name to describe content is redundant because content describes itself.

The debate surrounding semantics has raged for years, but it is important that we adopt a more pragmatic, sensible approach to naming things in order to work more efficiently and effectively. Instead of focussing on ‘semantics’, look more closely at sensibility and longevity—choose names based on ease of maintenance, not for their perceived meaning.

Name things for people; they’re the only things that actually read your classes (everything else merely matches them). Once again, it is better to strive for reusable, recyclable classes rather than writing for specific use cases. Let’s take an example:

/**
 * Runs the risk of becoming out of date; not very maintainable.
 */
.blue {
  color: blue;
}

/**
 * Depends on location in order to be rendered properly.
 */
.header span {
  color: blue;
}

/**
 * Too specific; limits our ability to reuse.
 */
.header-color {
  color: blue;
}

/**
 * Nicely abstracted, very portable, doesn’t risk becoming out of date.
 */
.highlight-color {
  color: blue;
}
It is important to strike a balance between names that do not literally describe the style that the class brings, but also ones that do not explicitly describe specific use cases. Instead of .home-page-panel, choose .masthead; instead of .site-nav, favour .primary-nav; instead of .btn-login, opt for .btn-primary.

# info end

#### 0 and Units

- Do not use units after `0` values unless they are required.

**Do**

    flex: 0px; /* This flex-basis component requires a unit. */
    flex: 1 1 0px; /* Not ambiguous without the unit, but needed in IE11\. */
    margin: 0;
    padding: 0;
    
    
#### Border

- Use `0` instead of `none` to specify that a style has no border.

**Don’t**

```css
.foo {
  border: none;
}
```

**Do**

```css
.foo {
  border: 0;
}
```

#### Hacks

- Avoid user agent detection as well as CSS “hacks”—try a different approach first.

It’s tempting to address styling differences over user agent detection or special CSS filters, workarounds, and hacks. Both approaches should be considered last resort in order to achieve and maintain an efficient and manageable code base. Put another way, giving detection and hacks a free pass will hurt projects in the long run as projects tend to take the way of least resistance. That is, allowing and making it easy to use detection and hacks means using detection and hacks more frequently—and more frequently is too frequently.

### CSS Formatting Rules

#### Declaration Order

As off right now we don’t have a solid suggestion on how to order declarations. Alphabetizing declarations does not do the trick of writing clearly arranged code; it’s just time consuming. Try to group declarations by what they do. Otherwise feel free to propose a nice solution.  

#### Declaration Style

- **Do not use ID selectors!**
- Use a semicolon after every declaration
- Use a space after a property name’s colon
- Always use a single space between property and value
- Use a space between the last selector and the declaration block
- The opening brace should be on the same line as the last selector in a given rule
- Always start a new line for each selector and declaration
- Always put a blank line between rules

**Don’t**
 
    /* Don’t: Style IDs, much too specific */
    #menu {
        background: green;
    }
    
    /* Don’t: missing ; */
    .test {
        display: block;
        height: 100px
    }
    
    /* Don’t: missing spaces */    
    h3{
        font-weight:bold;
    }
    
    /* Don’t: unnecessary line break */
    #video
    {
        margin-top: 1em;
    }
    
    /* Don’t: selectors and declarations on same line */
    h1, h2, h3 {
        font-weight: normal; line-height: 1.2;
    }
    
    /* Don’t: no blank line between rules */
    html {
          background: #fff;
    }
    body {
          margin: auto;
          width: 50%;
    }   

**Do**

    .menu {
        background: green;
    }
    
    .test {
        display: block;
        height: 100px;
    }
    
    h3 {
        font-weight: bold;
    }
    
    #video {
        margin-top: 1em;
    }
    
    h1,
    h2,
    h3 {
        font-weight: normal;
        line-height: 1.2;
    }

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

### JavaScript hooks

- Avoid binding to the same class in both your CSS and JavaScript. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.
- Create JavaScript-specific classes to bind to, prefixed with `.js-`
- Don’t use data-Attributes for this purpuse

**Do**

    <button class="btn btn-primary js-request-to-book">Request to Book</button>
    
A common practice is to use data-* attributes as JS hooks, but this is incorrect. data-* attributes, as per the spec, are used to store custom data private to the page or application (emphasis mine). data-* attributes are designed to store data, not be bound to.

#### CSS Quotation Marks

- Use single (`''`) quotation marks for attribute selectors and property values.
- Do not use quotation marks in URI values (`url()`).
- Exception: If you do need to use the `@charset` rule, use double quotation marks—[single quotation marks are not permitted](https://www.w3.org/TR/CSS21/syndata.html#charset).

**Don’t**

    @import url("https://www.google.com/css/maia.css");

    html {
      font-family: "open sans", arial, sans-serif;
    }

**Do**

    @import url(https://www.google.com/css/maia.css);

    html {
      font-family: 'open sans', arial, sans-serif;
    }
   

### CSS Meta Rules

#### Section Comments

Group sections by a section comment (optional).

If possible, group style sheet sections together by using comments. Separate sections with new lines.

    /* Header */

    #adw-header {}

    /* Footer */

    #adw-footer {}

    /* Gallery */

    .adw-gallery {}






# Airbnb CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
1. [Translation](#translation)
1. [License](#license)



## CSS










## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[⬆ back to top](#table-of-contents)**

## Sources and further reading

https://google.github.io/styleguide/htmlcssguide.html
https://cssguidelin.es/
https://spaceninja.com/2018/09/17/what-is-modular-css/
https://github.com/airbnb/css
http://codeguide.co/