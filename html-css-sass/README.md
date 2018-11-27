# Humans & Machines HTML/CSS/SASS Style Guide

inspired by Google, AirBnB and cssguidelin.es

## Separation of Concerns

- Strictly structure (markup) from presentation (styling) from behavior (scripting)
- Keep the interaction between the three to an absolute minimum
- Choose server-side rendering above client-side rendering unless your a building a real webapp // TODO: true?

Make sure documents and templates contain only HTML and HTML that is solely serving structural purposes. Move everything presentational into style sheets, and everything behavioral into scripts.

Whenever possible, we prefer to write HTML and CSS over JavaScript. In general, HTML and CSS are more prolific and accessible to more people of all different experience levels. HTML and CSS are also faster in your browser than JavaScript, and your browser generally provides a great deal of functionality for you.    

## Accessibility

- Use semantic headings and structure 
- Order DOM elements logically
- Use elements for what they have been created for. For example, use heading elements for headings, `p` elements for paragraphs, `a` elements for anchors, etc.
- Make content readable and provide predictable functionality
- Ensure links have :focus state and are recognizable (underlined)
- Use appropriate alt text for images
- To prevent redundancy and for images whose purpose is purely decorative, use no alt text, as in `alt=""`
- Use unobtrusive JavaScript and beware of in line scripting
- Provide No-JS alternatives, at least in a lo-fi manner
- Ensure that tab order for the site and especially forms follow a logical pattern
- Add labels for all form controls
- Make sure placeholder attributes are not being used in place of label tags
- Avoid Assumptions

Accessibility affects all users, not just those with stereotypical disabilities. Accepting this means realizing accessibility is about building for stress cases, such as:
 
- Old age
- Chronic medical conditions like arthritis
- Being outside with a heavy sun glare
- Cognitive impairment from medication or lack of sleep
- Needing to use a site with different devices
- Shaky WiFi that affects asset loading
- Running from the thing that escaped your floorboards

Think about it!


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

- Always use comments to explain non-obvious code: 
- What does it cover?
- What purpose does it serve?
- Why is respective solution used or preferred?
- Whether some CSS relies on other code elsewhere?
- What effect changing some code will have elsewhere?

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that color: red; will make something red, but if you’re using overflow: hidden; to clear floats—as opposed to clipping an element’s overflow—this is probably something worth documenting.

### Component and High-Level Comments

- Use a DocBlock multi-line comment to document components
- Describe states, permutations, conditions, and treatments

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
 
### Section Comments

- Group sections by a section comment

**Do**

    /* Header */
    .article__header {}

    /* Footer */
    .article__footer {}

    /* Gallery */
    .article__gallery {}
    

### Declaration Comments

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset.

To do this we can use a kind of reverse footnote like so:

- Always use comments to explain non-obvious declarations
- Either use the reverse footnote approach 
- or insert a simple end-of-line comment

**Do**
    
    /* Reverse footnote approach
     *
     * These types of comment allow us to keep all of our documentation in one place 
     * whilst referring to the parts of the ruleset to which they belong.
     */
    
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
    
    /* End-of-line approach
     *
     * The reverse footnote approach however seems like an overkill in some cases
     * It might eventually be even harder to read if the ruleset is very long
     * and there are lots of comments.
     * Therefore an evil but simple end-of-line comment will do the trick as well
     * even if they extend our set maximum line length.
     */

    /**
     * Helper-class to cut off a long line
     */
    .ellipsis {
        white-space: nowrap; // Prevents line-breaks
        text-overflow: ellipsis; // Adds an ellipses to the end of the line
        overflow: hidden;
    }

Whatever syntax you prefer, try to stay consistent at least within the file you are editing.

### HTML comments

// TODO
Twig or Smarty

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

### Formatting Rules

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
    

## CSS and SASS

### Formating Rules

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


### CSS Quotation Marks

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

### Declaration Order

- Group declarations by type
- Use empty line to group
- Don’t think about the order within a group so much
- Add `@includes` where they make sense within this order

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
    
Sticking to this proposal of declaration sorting is not mandatory. Have a look at the specific codebase. If nobody does it, feel free to order randomly as well. Or have a linter to the rescue.   
    
### Selector Order and Nesting

- **Do not nest selectors unnecessarily**
- Add self-declarations first
- Add pseudo-classes next
- Add modifier and state declarations next
- Add responsive styles for every selector
- Avoid generating new selectors from the current selector reference (`&`)

**Don’t**

    /* Don’t: Nest selectors unnecessarily 
     *
     * If a selector will work without being nested then do not nest it.
     * The only exception is styling elements based on the state of a block or its modifier
     */
    .header__nav {        
        .header__nav-link {
            // styles for links inside header navi
            
            .header__nav-link--last {
                // styles for last link inside header navi
            }
        }
        
        /* Don’t: Ignore order */
        width: 100%
        
        &:hover {
            // hover styles
        }
    }
    
    /* Don’t: Group responsive styles into seperate blocks */
    @include respond-to(desktop) {
        .header__nav {
            // styles for desktop header navi
            
            .header__nav-link {
                // styles for desktop links inside header navi
            }
        }
    }
    
    /* Don’t: Generate new selectors from the current selector reference 
     *
     * This approach makes those selectors unsearchable in the codebase 
     * and it ultimately makes code more difficult to read
     */
    .header__nav {        
        &-link {
            // styles for links inside header navi 
            // -> .header__nav-link
        }
    }
    
**Do**

    .header__nav {
        width: 100%
        
        &:hover {
            // hover styles 
        }
        
        &:before {
            // pseudo styles
        }      
        
        /* Do: Nest states for better readability */
        .has-nav-shown &,
        &.is-active {
            // state related styles
        }
        
        /* Do: Nest modifier for better readability */
        &.header__nav--small {
            // styles for modified header
        }  
        
        /* Do: Add responsive styles for every selector */
        @include respond-to(desktop)  {
            // styles for desktop header navi
        }
    }
        
    .header__nav-link {
        // styles for links inside header navi
        
        @include respond-to(desktop)  {
            // styles for desktop links inside header navi
        }
    }
    
As it comes to responsiveness we usually are dealing with a main `mobile` and a `decent` breakpoint. The `decent` breakpoint covers every viewport larger than 599 pixel. While the default styles will define the mobile view, the decent styles will be applied with the first media query block. To distinguish between different `decent` viewports such as hudge phones, tablets and very big screen or portrait viewports as well, more precise sets of media queries will be added afterwards.

### Meaningful Whitespace

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

### Selectors

- Never style IDs
- Style type selectors for reset purposes only
- Avoid qualifying class names with type selectors. Put the class name at the lowest possible level
- Use combined selectors wisely
- Select what you want explicitly, rather than relying on circumstance or coincidence
- Write selectors for reusability
- Do not qualify selectors unnecessarily, as this will impact the number of different elements you can apply styles to
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
     */
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
    
    /* Don’t: Forget about selector performance */
     *
     * What this does is looks at every single element on the page (that’s every single one) 
     * and then looks to see if any of those live in the .header__nav-link parent.
     * This is a very un-performant selector as the key selector is a very expensive one.
     * The effect on smaller sides is pretty minimal
     * but if we are about to build the next ebay you might want to think about this
     */
    .header__nav-link > * {
        // styles for <button> and <a> inside parent
    }
    
    /* Don’t: Overcomplicate your code 
     * 
     * If your code looks like this you might want to think about adding a class to your HTML 
     */
    div:nth-of-type(3) ul:last-child li:nth-of-type(odd) * { 
        // style some magic list-item
    }   

**Do**
    
    /* Do: Use classes instead of IDs or type selectors 
     * Do: Be specific about what you are styling */
    .header__nav {
        // styles for header navi
    }
    
    /* Do: Use specific, reusable classes. Put the class name at the lowest possible level */
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

### ID and Class Naming

- Prefer dashes over camelCasing in class names 
- Use meaningful or generic ID and class names
- Use ID and class names that are as short as possible but as long as necessary
- Make use of BEM

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
    
### Modular CSS and BEM

- Create reusable components for common visual or behavioural patterns
- Abstract the structure of an object from the skin that is being applied
- Don’t style objects based on their context. An object should look the same no matter where you put it
- Use BEM to style `blocks` containing `elements` to be altered with `modifiers`
- Write names in lower case
- Separate words within names by hyphens `-`
- Delimit elements by double underscores `__`
- Delimit modifiers by double hyphens `--`
- Use the same naming convention for grandchildren. So no `.block-name__child__grand-child`-craziness
- Use utility classes for common visual or behavioural micro patterns
- Use `has-` or `is-` prefix for the state

**Don’t**
         
    <!--    
     * No one but the person who wrote the following code knows what it does
     *
     * How are the classes box and profile related to each other?
     * How are the classes profile and avatar related to each other?
     * Are they related at all? Should you be using pro-user alongside bio?
     * Will the classes image and profile live in the same part of the CSS?
     * Can you use avatar anywhere else? 
     -->     
    <div class="box profile pro-user">
        <img class="avatar image" />
        <p class="bio">...</p>
    </div>
    
    /* Don’t: Use not-reusable class names */
    .header--desktop {        
        @media screen and (max-width: 599px) {
            display: none;
        }
    }
    
    /* Don’t: Be unspecific with state classes */
    .activated {
        display: block;
    }

**Do**
        
    <!--    
     * Going for a object-oriented BEM approach makes the following code self-documenting 
     *
     * We can see which classes are and are not related to each other, and how
     * We know what classes we can’t use outside of the scope of this component.
     * Also, we know which classes we are free to reuse elsewhere.
     -->     
    <div class="l-box m-profile m-profile--is-pro-user">
        <img class="m-avatar m-profile__image" />
        <p class="m-profile__bio">...</p>
    </div>
    
    /* Do: Use utility classes for common micro patterns */
    .hidden-on-mobile {
        @media screen and (max-width: 599px) {
            display: none;
        }
    }
    
    /* Do: Prefix state classes to tell what is going on */
    .is-activated {
        display: block;
    }

Blocks are logically and functionally independent components of a web page. They are nestable and should be capable of being contained inside another block without breaking anything. Elements are the constituent parts of a block that can’t be used outside of it. Modifiers define the appearance and behavior of a block. 

Utility classes are a powerful ally in combatting CSS bloat and poor page performance. A utility class is typically a single, immutable property-value pairing expressed as a class. Their primary appeal is speed of use while writing HTML and limiting the amount of custom CSS you have to write.

Modular CSS simplifies code and facilitates refactoring. It produces self-documenting and reusable code that doesn’t influence outside its scope. It is predictable, maintainable, and performant. 

### Responsiveness / Mobile first

- Make sure every component is entirely responsive 
- Default your CSS for less capable devices
- Style components for the smallest viewport first (Mobile first)
- Overwrite styles to adapt to larger viewports via media queries
- Don’t use media queries for the default view (smallest viewport) unless these styles are just an enhancement and overwriting for larger viewports will lead to extensive effort

Styling the most likely simpler component structure on mobile devices first will lead to less and simplified code and fewer overwrites. Leaving out everything within media queries the code can be parsed much faster by mobile devices—and devices with few capabilities such as e-book readers will see a simple default view.

### Relative Units

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
     */
    .main-wrapper {
        font-size: 1.6rem;
    }

    /* Don’t: Mix rem and em declarations within the same element
     * 
     * Here the width is relative to the elements own font-size 
     * instead of the font-size of the body.
     * The layout sizing via the body-font-size is thrown off.
     */
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
     */
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
    
### JavaScript hooks

- Avoid binding to the same class in both your CSS and JavaScript. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.
- Create JavaScript-specific classes to bind to, prefixed with `.js-`
- Don’t use data-Attributes for this purpose

**Do**

    <button class="btn btn-primary js-request-to-book">Request to Book</button>
    
A common practice is to use data-* attributes as JS hooks, but this is incorrect. data-* attributes, as per the spec, are used to store custom data private to the page or application (emphasis mine). data-* attributes are designed to store data, not be bound to.
 
### Hacks

- Avoid user agent detection as well as CSS “hacks”—try a different approach first
- If you need to use hacks anyways, make sure to comment extensively

### SASS Syntax and Formatting

- Never write pure CSS, always use well structured SASS
- Use the `.scss` syntax, never the original `.sass` syntax
- Wrap strings with single quotes `'`
- Don’t wrap CSS values, which require not to be quoted
- Add leading zeros before a decimal value
— Don’t add units to 0 values when dealing with lengths
- Wrap top-level numeric calculations in parentheses
- Avoid the use of magic numbers

**Don’t**

    /* Don’t: Fail to use quotes
     *
     * Color names are treated as colors when unquoted,
     * which can lead to serious issues
     * Most syntax highlighters will choke on unquoted strings 
     */
    $direction: left;
    
    /* Don’t: Use quotes on some specific values
     *
     * Specific CSS values (identifiers) such as initial or sans-serif
     * require not to be quoted. Indeed, the declaration font-family: 'sans-serif' 
     * will silently fail because CSS is expecting an identifier, not a quoted string. 
     * Because of this, we do not quote those values. 
     */
    $font-type: 'sans-serif';
    
    /* Don’t: Display leading zeros before a decimal value */
    .foo {
        padding: 2.0em;
        opacity: .5;
    }
    
    /* Don’t: Add unit to 0 value */
    $length: 0em;
    
    /* Don’t: Fail to wrap numeric calculations in parentheses
     * Not only does this requirement dramatically improve readability, 
     * it also prevents some edge cases by forcing Sass to evaluate 
     * the contents of the parentheses.
     */
    .foo {
        width: 100% / 3;
    }
    
    /* Don’t: Use magic numbers
     *
     * “Magic number” is an old school programming term for unnamed numerical constant.
     * Basically, it’s just a random number that happens to just work 
     * yet is not tied to any logical explanation.
     * Needless to say magic numbers are a plague and should be avoided at all costs.
     * When you cannot manage to find a reasonable explanation for why a number works, 
     * add an extensive comment explaining how you got there and why you think it works. 
     */
    .foo {
        top: 0.327em; // This value is the lowest to align the top of `.foo` with its parent
    }

**Do**
    
    /* Do: Wrap strings with single quotes
    $direction: 'left';
    
    /* Do: Skip quoting on some specific values
    $font-type: sans-serif;
    
    /* Do: Never display trailing zeros */
    .foo {
        padding: 2em;
        opacity: 0.5;
    }
    
    /* Do: Use 0 values without units */
    $length: 0;
    
    /* Do: Wrap numeric calculations in parentheses */
    .foo {
        width: (100% / 3);
    }


### Variables

- Initiate variables the value is repeated at least twice and the value is likely to be updated
- Use dash-cased variable names (e.g. `$my-variable`)
- Store all variables in the `_var.scss`
- Prefix variables with `_` (e.g. `$_my-variable`), if you want to put it inside a component file and only use it there

### Extend

- Avoid `@extend`

Extending is invisible. Extending doesn’t necessarily help file weight, contrary to the saying. Extending doesn’t work across media queries. Extending is not flexible. Mixins have absolutely no drawback.



# airBNB



### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.


# Architecture
Break Into As Many Small Files As Makes Sense
There is no penalty to splitting into many small files. Do it as much as feels good to the project. I know I find it easier to jump to small specific files and navigate through them than fewer/larger ones.

I'd probably do this right in the global.scss, rather than have global @import a _header.scss file which has its own sub-imports. All that sub-importing could get out of hand.

Globbing might help if there starts to be too many to list.

#Partials are named _partial.scss
This is a common naming convention that indicates this file isn't meant to be compiled by itself. It likely has dependencies that would make it impossible to compile by itself. Personally I like dashes in the "actual" filename though, like _dropdown-menu.scss.


## Sources and further reading

- https://google.github.io/styleguide/htmlcssguide.html
- https://cssguidelin.es/
- https://spaceninja.com/2018/09/17/what-is-modular-css/
- https://github.com/airbnb/css
- http://codeguide.co/
- https://dev.to/maxwell_dev/the-web-accessibility-introduction-i-wish-i-had-4ope
- https://a11yproject.com/checklist
- https://csswizardry.com/2011/09/writing-efficient-css-selectors/
- https://github.com/necolas/idiomatic-css
- https://getbootstrap.com/docs/
- https://css-tricks.com/mobile-small-portrait-slow-interlace-monochrome-coarse-non-hover-first/
- https://www.sitepoint.com/avoid-sass-extend/