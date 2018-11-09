# Humans & Machines HTML/CSS/SASS Style Guide

inspired by Google, AirBnB and cssguidelin.es

## Separation of Concerns

Separate structure from presentation from behavior.

Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum.

That is, make sure documents and templates contain only HTML and HTML that is solely serving structural purposes. Move everything presentational into style sheets, and everything behavioral into scripts.    

## Accessibility

// TODO

#### Multimedia Fallback

Provide alternative contents for multimedia.

For multimedia, such as images, videos, animated objects via `canvas`, make sure to offer alternative access. For images that means use of meaningful alternative text (`alt`) and for video and audio transcripts and captions, if available.

Providing alternative contents is important for accessibility reasons: A blind user has few cues to tell what an image is about without `@alt`, and other users may have no way of understanding what video or audio contents are about either.

For images whose `alt` attributes would introduce redundancy, and for images whose purpose is purely decorative, use no alternative text, as in `alt=""`.

**Don’t**
 
    <img src="spreadsheet.png">

**Do**

    <img src="spreadsheet.png" alt="Spreadsheet screenshot">

### Server-Side Rendering

// TODO: maybe part of seperation of concerns

## General Formatting Rules

- **Use only lowercase** - or mixed cases for copy
- Indent by 4 spaces at a time
- Don’t use tabs – unless your editor replaces them with whitespace – or mix tabs and spaces for indentation.

**Don’t**
    
    /* Never use uppercase, especially for copy text
     * If it needs to be uppercase use CSS, stupid!
     */
    <A HREF="/">HOME</A>

    color: #E5E5E5;

**Do**

    <a href="/">Home</a>
    
    color: #e5e5e5;

All code has to be lowercase: This applies to HTML element names, attributes, attribute values (unless `text/CDATA`), CSS selectors, properties, and property values (with the exception of strings).

## Comments

Explain code as needed, where possible. 

Use comments to explain code: 

- What does it cover?
- What purpose does it serve?
- Why is respective solution used or preferred?
- Whether some CSS relies on other code elsewhere?
- what effect changing some code will have elsewhere?

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that color: red; will make something red, but if you’re using overflow: hidden; to clear floats—as opposed to clipping an element’s overflow—this is probably something worth documenting.

### High-level

For large comments that document entire sections or components, we use a DocBlock-esque multi-line comment which adheres to our 80 column width.

**Do**

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
 
This level of detail should be the norm for all non-trivial code—descriptions of states, permutations, conditions, and treatments.

### Section Comments

- Group sections by a section comment (optional).


    /* Header */
    .article__header {}

    /* Footer */
    .article__footer {}

    /* Gallery */
    .article__gallery {}
    

### Low-level

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset.

To do this we can use a kind of reverse footnote like so:

**Do**

    /**
     * Helper-class to cut off a long line 
     * 1. Prevents line-breaks
     * 2. Adds an ellipses to the end of the line
     */
    .ellipsis {
      white-space: nowrap; /* 1 */
      text-overflow: ellipsis; /* 2 */
      overflow: hidden;
    }

These types of comment allow us to keep all of our documentation in one place whilst referring to the parts of the ruleset to which they belong.

However this might seem like an overkill in some cases. It might eventually be even harder to read if the ruleset is very long and there are lots of comments.

Therefore we might just want to use these evil end-of-line comments—even if they extend our set maximum line length.

**Also ok**
```
/**
 * Helper-class to cut off a long line
 */
.ellipsis {
  white-space: nowrap; // Prevents line-breaks
  text-overflow: ellipsis; // Adds an ellipses to the end of the line
  overflow: hidden;
}
```

Whatever syntax you prefer, try to stay consistent at least within the file you are editing.

## Action Items

Mark todos and action items with `TODO` and append action as in `TODO: action item`.

Append a contact (username or mailing list) in parentheses as with the format `TODO(contact)`.

**Do**
```
{# TODO(john.doe): revisit centering #}
<center>Test</center>

<!-- TODO: remove optional tags -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>

.box--flex {
    display: flex // TODO: implement fallback for older brwosers
}
```

## HTML

### Document Type and Validity

- Use HTML5 for all HTML documents: `<!DOCTYPE html>`.
- Although fine with HTML, do not close void elements, i.e. write `<br>`, not `<br />`.
- Use valid HTML code unless that is not possible
- Use tools such as the [W3C HTML validator](https://validator.w3.org/nu/) to test
    
### Semantics

- Use HTML according to its purpose
- Use elements for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc.

Using HTML according to its purpose is important for accessibility, reuse, and code efficiency reasons.

### HTML Formatting Rules

- Use a new line for every block, list, or table element
- Indent every child element
- Seperate sections with empty lines
- Use double (`""`) rather than single quotation marks (`''`) around attribute values.

**Don’t**
    
    <blockquote><p><em>Space</em>, the final frontier.</p></blockquote>
    <ul class='list'>
    <li>Moe</li>
    <li>Larry</li>
    <li>Curly</li></ul>

**Do**

    <blockquote>
      <p><em>Space</em>, the final frontier.</p>
    </blockquote>

    <ul class="list">
      <li>Moe</li>
      <li>Larry</li>
      <li>Curly</li>
    </ul>
    

## CSS

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

- Never style IDs, style type selectors only for reset purposes.
- Avoid qualifying class names with type selectors
- Use combined selectors wisely
- Select what you want explicitly, rather than relying on circumstance or coincidence
- Write selectors for reusability
- Do not nest selectors unnecessarily
- Do not qualify selectors unnecessarily, as this will impact the number of different elements you can apply styles to.
- Keep selectors as short as possible, in order to keep specificity down and performance up
- Think about selector performance

**Don’t**


    /* Don’t: Use of IDs for styling */
    #header-nav {}
    
    /* Don’t: qualifying ID and class names with type selectors */
    div.error {}
    
    /* Don’t: Use type selectors for specific styles: poor Selector Intent
     *
     * This selector’s intent is to style any ul inside any header element,
     * whereas our intent was to style the site’s main navigation.
     * This is poor Selector Intent: you can have any number of header elements on a page,
     * and they in turn can house any number of uls, so a selector like this runs the risk of 
     * applying very specific styling to a very wide number of elements. 
     * This will result in having to write more CSS to undo the greedy nature of such a selector.
    header ul { 
        // styles for header navi
    }
    
    /* Don’t: Use type selectors for specific styles: poor Selector Intent 
     *
     * Not only does this have poor Selector Intent
     * it will greedily style any and every link inside of a .promo to look like a button
     * it is also pretty wasteful as a result of being so locationally dependent: 
     * we can’t reuse that button with its correct styling outside of .promo 
     * because it is explicitly tied to that location.   
    */
    .promo a { 
        // styles for promo button
    } 
    
    /* Don’t: Nest selectors unnecessarily */
    .header__nav {
        // styles for header navi
        
        .header__nav-link {
            // styles for links inside header navi
        }
    }
    
    /* Don’t: Forget about selector performance */
     *
     * What this does is looks at every single element on the page (that’s every single one) 
     * and then looks to see if any of those live in the .header__nav-link parent.
     * This is a very un-performant selector as the key selector is a very expensive one.
     * The effect on smaller sides is pretty minimal
     * but if we are about to build the next ebay you might want to think about this
    .header__nav-link > * {
        // styles for <button> and <a> inside parent
    }
    
    /* Don’t: Overcomplicate your code 
     * 
     * If your code looks like this you might want to think about adding a class to your HTML */
    div:nth-of-type(3) ul:last-child li:nth-of-type(odd) * { 
        // style some magic list-item
    }
    

**Do**
    
    /* Do: Use classes instead of IDs or type selectors 
     * Do: Be specific about what you are styling */
    .header__nav {
        // styles for header navi
    }

    /* Do: Avoid nesting if possible */
    /* .header__nav .header__nav-link -> */ .header__nav-link {
        // styles for header navi
    }
    
    /* Do: Use specific, reusable classes */
    /* .promo a -> */ .btn--promo { 
        // styles for promo button
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

#### Relative Units

- Avoid pixels, they are ignorant
- Use relative units like `rem` and `em` instead
- Use `rem` for font-sizes to be resized via the `Html` root font-size 
- Use `em` for layout element size declaration to be resized via the `Body` font-size
- Avoid font-size declarations on parents
- Avoid throwing layout sizes on elements with font-size declarations 

**Don’t**
    
    /* Don’t: Use pixel values */
    .headline {
        font-size: 18px;
        width: 300px;
    }
    
    /* Don’t: Add unnessary font-sizes on parents
     * 
     * This declaration destoys the layout sizing approach for all children
     .main-wrapper {
        font-size: 1.6rem;
     }

    /* Don’t: Mix rem and em declarations within the same element
     * 
     * Here the width is relative to the elements own font-size 
     * instead of the font-size of the body.
     * The layout sizing via the body-font-size is thrown off.
    .headline {
        font-size: 1.8rem;
        width: 20em;
    }

**Do**
    
    /* We usually set a base font-size to both the html and the body element
     * in order to split layout-scaling from font-size-scaling.
     * These root font-sizes are scaled based on the viewport width (vw)
     * via a crazy sass-mixin and streamlined across all breakpoints.
     * This approach makes the streamlining of font on layout sizes 
     + for all viewport sizes very easily
     *
     * What you see here is just a much simplified example
     * without viepwort-based resizing
    html {
        font-size: 16px;
    }
    
    body {
        font-size: 12px;
    }
    
    /* Layout elements should be sized based on the body-font-size via the em unit */
    .box {
        width: 20em;
    }
    
    /* Font-sizes are responding to the html-font-size via the rem unit */
    .box__headline {
        font-size: 1.8rem;
    }
    
    /* The use of viewport units also a way to set up layout elements */ 
    .main-wrapper {
        width: 80vw;
        margin: 0 auto;
    }
    
    .hero {
        height: 18em;
        min-height: 80vh;
    }

    /* Due to render-issues on half pixels the use of pixels might be ok */
    .btn--underline {
        border-width: 1px;
    } 
 
   
    

#### Hacks

- Avoid user agent detection as well as CSS “hacks”—try a different approach first
- If you need to use hacks anyways, make sure to comment extensively

### CSS Formatting Rules

#### Declaration Order

- Group declarations by type
- Use empty line to group
- Don’t think about the order within a group so much

**Do**

    .selector {
        /* Layout/Positioning
         * The position of the element in space
         */
        position: absolute;
        z-index: 10;
        top: 0;
        right: 0;
        transform: translate3d(2rem, 0, 0);
        
        /* Display & Box Model
         * The element itself
         */
        display: inline-block;
        overflow: hidden;
        box-sizing: border-box;
        width: 10em;
        height: 10em;
        padding: 1em;
        border: 1px solid #333;
        margin: 1em;
        
        /* Visual
         * Design of the element
         */
        background: #000;
        color: #fff;
        opacity: 0.9;
        
        /* Type
         * Typesetting of the element
         */
        font-family: sans-serif;
        font-size: 16px;
        line-height: 1.4;
        text-align: right;
        
        /* Behaviour 
         * Transitions and activity of the element */
        cursor: pointer;
        transition: opacity 0.3s linear;
    }  

#### Declaration Style

- Use a semicolon after every declaration
- Use a space after a property name’s colon
- Always use a single space between property and value
- Use a space between the last selector and the declaration block
- The opening brace should be on the same line as the last selector in a given rule
- Always start a new line for each selector and declaration
- Use lowercase and shorthand hex values, e.g., `#aaa`
- Where allowed, avoid specifying units for zero-values, e.g. `margin: 0`
- Include a space after each comma in comma-separated property or function values
- Always put a blank line between rules

**Don’t**
    
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
    
    /* Don’t: no blank line between rules
     * Don’t: no use of lowercase and shorthand hex values */
    html {
          background: #FFFFFF;
    }
    body {
          margin: auto;
          width: 50%;
    }   

**Do**
    
    html {
        background: #fff;
    }
    
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
    
    
    /* Large blocks of single declarations 
     * can use a slightly different, single-line format. 
     * In this case, a space should be included after the opening brace 
     * and before the closing brace.
     */
    .selector-1 { width: 10%; }
    .selector-2 { width: 20%; }
    .selector-3 { width: 30%; }
    
    /* Long, comma-separated property values
     * such as collections of gradients, shadows or transitions
     * can be arranged across multiple lines to improve readability
     */
    .selector {
        box-shadow: 1px 1px 1px #000,
                    2px 2px 1px 1px #ccc inset;
        transition: opacity 0.3s linear,
                    transform 0.9s linear;
    }


#### Meaningful Whitespace

- Make use of whitespace between rulesets to structure code (optional)
- But four blank lines before new sections

**Do**

    /* .foo */    
    .foo { }
    
    .foo__bar { }
    
    .foo--baz { }
     
    
    
    /* .bar */    
    .bar { }
    
    .bar__baz { }
    
    .bar__foo { }


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



## Sources and further reading

https://google.github.io/styleguide/htmlcssguide.html
https://cssguidelin.es/
https://spaceninja.com/2018/09/17/what-is-modular-css/
https://github.com/airbnb/css
http://codeguide.co/
https://dev.to/maxwell_dev/the-web-accessibility-introduction-i-wish-i-had-4ope
https://csswizardry.com/2011/09/writing-efficient-css-selectors/
https://github.com/necolas/idiomatic-css