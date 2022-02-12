# Guidelines for writing HTML and CSS

These guidelines should help you and us to work together on sweet projects.

> Every line of code should appear to be written by a single person, no matter the number of contributors.

Most parts of these guidelines are heavily inspired by the work of [Harry Roberts](https://csswizardry.com/) 
([cssguidelin.es](https://cssguidelin.es/), 
[ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/)), 
the [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html), as well as the mindset of
Andy Bell ([CUBE CSS](https://piccalil.li/blog/cube-css)) and [Tailwind’s](https://tailwindcss.com/) creator 
[Adam Watham](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/).

## Table of contents

- [Separation of Concerns](#separation-of-concerns)
- [Accessibility](#accessibility)
- [General Formatting Rules](#general-formatting-rules)
- [Comments](#comments)
  * [Component and High-Level Comments](#component-and-high-Level-comments)
  * [Section Comments](#section-comments)
  * [Declaration Comments](#declaration-comments)
  * [HTML comments](#html-comments)
- [Action Items](#action-items)
- [HTML](#html)
  * [Document Type and Validity](#document-type-and-validity)
  * [HTML Formatting Rules](#html-formatting-rules)
  * [Reducing Markup](#reducing-markup)
  * [JavaScript Generated Markup](#javascript-generated-markup)
- [CSS Methodology](#css-methodology)
  * [Key Goals](#key-goals)
  * [Guidelines](#guidelines)
  * [CSS Structure](#css-structure)
  * [Utility Classes](#utility-classes)
  * [Objects](#objects)
  * [Components and BEM](#components-and-bem)
  * [Scoped Components](#scoped-components)
  * [Responsiveness / Mobile first](#responsiveness--mobile-first)
  * [Design Tokens](#design-tokens)
  * [Global Values](#global-values)
  * [Class Grouping](#class-grouping)
  * [Componentless Modules](#componentless-modules)
- [CSS Codestyle](#css-codestyle)
  * [Formating Rules](#formating-rules)
  * [CSS Quotation Marks](#css-quotation-marks)
  * [Declaration Order](#declaration-order)
  * [Selector Order and Nesting](#selector-order-and-nesting)
  * [Meaningful Whitespace](#meaningful-whitespace)
  * [Selectors](#selectors)
  * [ID and Class Naming](#id-and-class-naming)
  * [JavaScript Hooks](#javascript-hooks)
  * [Breakpoints](#breakpoints)
  * [Hacks](#hacks)
- [Best Practises](#best-practises)
  * [Relative Units](#relative-units)
  * [Custom Properties](#custom-properties)
  * [Spacing](#spacing)
  * [Modular Scale](#modular-scale)
  * [Type Styles](#type-styles)
  * [Fluid Typography](#fluid-typography)
  * [Preferes Reduced Motion](#preferes-reduced-motion)
- [Preprocessors/SASS](#preprocessors-sass)
  * [SASS Syntax and Formatting](#sass-syntax-and-formatting)
  * [File Naming](#file-naming)
  * [Variables](#variables)
  * [Extend](#extend)
- [PurgeCSS](#purgecss)
- [Sources and further reading](#sources-and-further-reading)



## Separation of Concerns

- Strictly structure (markup) from presentation (styling) from behavior (scripting)
- Keep the interaction between the three to an absolute minimum
- Choose server-side rendering above client-side rendering unless your a building a real webapp

Make sure documents and templates contain only HTML and HTML that is solely serving structural purposes. Move everything 
presentational into style sheets, and everything behavioral into scripts. There might be exceptions from this rule 
(eg. think Atomic CSS).

Whenever possible, we prefer to write HTML and CSS over JavaScript. In general, HTML and CSS are more prolific and 
accessible to more people of all different experience levels. HTML and CSS are also faster in your browser than 
JavaScript, and your browser generally provides a great deal of functionality for you.    

## Accessibility

Accessibility affects all users, not just those with stereotypical disabilities. Accepting this means realizing
accessibility is about building for stress cases, such as:

- Old age
- Chronic medical conditions like arthritis
- Being outside with a heavy sun glare
- Cognitive impairment from medication or lack of sleep
- Needing to use a site with different devices
- Shaky WiFi that affects asset loading
- Running from the thing that escaped your floorboards

> 💡 Think about it!
 
Therefore we advocate the creation of highly accessible products:

- Use semantic headings and structure 
- Order DOM elements logically
- Use elements for what they have been created for. For example, use heading elements for headings, `p` elements for 
paragraphs, `a` elements for anchors, etc.
- Make content readable and provide predictable functionality
- Ensure links have `:focus` or `:focus-visible` states and are recognizable (eg. underlined)
- Use appropriate alt text for images
- Let screen reader users and keyboard users skip to main content 
- To prevent redundancy and for images whose purpose is purely decorative, use no alt text, as in `alt=""`
- Use unobtrusive JavaScript and beware of inline scripting
- Provide No-JS alternatives, at least in a lo-fi manner
- Ensure that content will be displayed even if JavaScript fails or takes a while to be parsed
- Ensure that tab order for the site and especially forms follow a logical pattern
- Add labels for all form controls
- Make sure placeholder attributes are not being used in place of label tags
- Prevent transitions and animations for users which prefer reduced motion
- Make sure to hide soley decorative elements from screen-reader
- Hide duplicate responsive content from screen-readers (eg. hide a desktop menu if there is a mobile menu as well)
- Make heavy and wise use of aria-attributes
- Avoid Assumptions


## General Formatting Rules

- **Use only lowercase** - or mixed cases for copy
- Indent by 4 spaces at a time
- Don’t use tabs – unless your editor replaces them with whitespace – or mix tabs and spaces for indentation.

**Don’t**

```
/* Never use uppercase, especially for copy text
* If it needs to be uppercase use CSS, stupid!
*/
<A HREF="/">HOME</A>

color: #E5E5E5;
```

**Do**

```
<a href="/">Home</a>

color: #e5e5e5;
```

All code has to be lowercase: This applies to HTML element names, attributes, attribute values (unless `text/CDATA`), 
CSS selectors, properties, and property values (with the exception of strings).

## Comments

You are not alone in this. At least it is you and your future you. So please help yourself and other, and 
always use comments to explain non-obvious code:

- What does it cover?
- What purpose does it serve?
- Why is respective solution used or preferred?
- Whether some CSS relies on other code elsewhere?
- What effect changing some code will have elsewhere?

As a rule, you should comment anything that isn’t immediately obvious from the code alone. 
That is to say, there is no need to tell someone that `color: red;` will make something red, 
but if you’re using `overflow: hidden;` to clear floats – as opposed to clipping an element’s 
overflow – this is probably something worth documenting.

### Component and High-Level Comments

- Use a DocBlock multi-line comment to document components
- Describe states, permutations, conditions, and treatments

**Do**

```
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
### Section Comments

- Group sections by a section comment (optional if the class names speak for themselves)

**Do**

```scss
/* Header */
.article__header {}

/* Footer */
.article__footer {}

/* Gallery */
.article__gallery {}
```

### Declaration Comments

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset. 
To do this we can:

- use a kind of reverse footnote (streber style)
- insert a simple end-of-line comment

**Do**

```scss
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
```

Whatever syntax you prefer, try to stay consistent at least within the file you are editing.

### HTML comments

Ìf you are using template languages such as Twig or Smarty use comments to explain non-obvious code as well. 
The commenting style might differ with specific language and project. Again, whatever syntax you prefer, 
try to stay consistent within a codebase.

## Action Items

- Mark todos and action items with `TODO` and append action as in `TODO: action item`.
- Append a contact (username or mailing list) in parentheses as with the format `TODO @contact`.

**Do**

```html
{# TODO @jonny: revisit centering #}
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

### HTML Formatting Rules

- Use a new line for every block, list, or table element
- Indent every child element
- Seperate sections with empty lines
- Use double (`""`) rather than single quotation marks (`''`) around attribute values.
- Break long lines if it significantly improves readability (optional)

**Don’t**

```html 
<blockquote><p><em>Space</em>, the final frontier.</p></blockquote>
<ul class='list'>
<li>Moe</li>
<li>Larry</li>
<li>Curly</li></ul>
```

**Do**

```html 
<blockquote>
  <p><em>Space</em>, the final frontier.</p>
</blockquote>

<ul class="list">
  <li>Moe</li>
  <li>Larry</li>
  <li>Curly</li>
</ul>

<md-progress-circular md-mode="indeterminate"
                      class="md-accent"
                      ng-show="ctrl.loading"
                      md-diameter="35">
</md-progress-circular>
```

### Reducing Markup

- Avoid superfluous parent elements when writing HTML whenever possible

**Don’t**

```html 
<span class="avatar">
  <img src="...">
</span>
```

**Do**

```html 
<img class="avatar" src="...">
```

### JavaScript Generated Markup

Writing markup in a JavaScript file makes the content harder to find, harder to edit, and less performant. 
Avoid it whenever possible (Nevermind if you are going down the JS framework road).

## CSS Methodology

### Key Goals

- Keep things maintainable and predictable
- Think ahead of time and ensure scalability
- Keep specificity low at all times
- Utilise the power of CSS and the cascade

### Approach

Our approach of writing CSS is heavily inspired by [ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) (Inversed Triangle CSS by Harry Roberts/csswizardry)
and [CUBE CSS](https://piccalil.li/blog/cube-css) by Andy Bell and we spice it up with a heavy dose of atomic CSS 
(eg. with [TailwindCSS](https://tailwindcss.com/)).

> The inversed triangle from ITCSS shows how styles represented by selectors are ordered in the resulting CSS:
from generic styles to explicit ones, from low-specificity selectors to more specific ones
and from far reaching to localized ones.

    Inversed Triangle
    
      *************     Settings
       ***********      Tools
        *********       Generic
         *******        Elements
          *****         Objects
           ***          Components
            *           Utilities

The idea of the `Inversed Triangle` gives us a good underlying structure of how to set up our CSS layers. 
It is a great guideline of where new code needs to live – and it is the best weapon against specificity wars.

We also see a lot of potential in an utility-first approach using atomic CSS classes:

- Fewer declaration duplicates
- No specificity issues
- No need for class and component naming
- Very predictable
- Reduced risk of regressions

The benefits with an extensive use of utility classes are obvious. However there are times – especially in 
heavily art directed projects – where a utility-only approach seems to fall short. So for our way of writing code 
Andy Bell’s mindset with [CUBE CSS](https://piccalil.li/blog/cube-css) comes into play:

> CUBE CSS (Composition, Utility, Block, Exception) is a methodology oriented 
> towards simplicity and consistency. Instead of going utility-first like atomic CSS libraries such as Tailwind, 
> CUBE CSS tries to unleash the power of the cascade by the use of global style axioms and layout primitives.
 
> The Cascade is itself *magnificent*, because it enables us to write very little CSS and really isn’t
as scary as people often make it out to be.

So for setting up the groundworks, implementing the overall composition and laying out recurring design
patterns we would rather go for a smarter, cascade-driven way than adding lots of classes to every HTML element.

We also think that for more complex scenarios (eg. modals) or recurring building blocks (think cards or buttons) 
good old `CSS Components` *(CUBE CSS terminology: Block)* should still be the way to go.

### Guidelines

- Think progressive enhancement
- Go [mobile first](#responsiveness--mobile-first)
- DYI: Don’t repeat yourself (and keep the codebase tight)
- Unleash the power of [Custom Properties](#custom-properties) at all times

In the spirit of the `Inversed Triangle` we group all selectors into layers:
from generic styles to explicit ones, from low-specificity selectors to more specific ones.

    Main Selector layers  

     *******        HTML Elements
      *****         Objects
       ***          Components
        *           Utilities

- Use styled `HTML Elements`, and `Objects` to layout
  the design system and composition
- Use `Objects` for common behavioural patterns
- Use `Utilities` heavily for everything which is not tackled
  by `HTML Elements` and `Objects`
- Use `Utilities` to modify styles declared
  by `HTML Elements`, `Objects`, and `Components`
- Use `Components` as a last resort for things
  which can’t be *(easily)* done with `Objects` and `Utilities`
- Use `Components` for more complex and contextual styles
  that deviate from the common, global system

### CSS Structure

```scss
.
├── SETTINGS
│   │ Define the ground: Design tokens, sizes, other vars
│   │ No CSS output here
│   │  
│   ├── Config...............Configuration and environment settings
│   ├── Tokens...............Global design tokens such as colors, size scale settings and text styles
│   ├── Breakpoints..........Breakpoint definitions
│   ├── UI...................UI Specific settings for custom properties
│   ├── Z-Index..............Handle z-index throughout relevant ui-elements
│   └── Vars.................Static SASS vars for use in components, objects and utilities
│ 
│ 
├── TOOLS
│   │ Globally used mixins and functions
│   │ No CSS output here
│   │
│   ├── Functions............Some simple helper functions
│   ├── Mixins...............Globally available mixins
│   └── Animations...........Define animations
│ 
│ 
├── GENERIC
│   │ Global resets and normalize styles, box-sizing definition, etc
│   │ First layer of the triangle that generates CSS
│   │
│   ├── Box-sizing...........Better default `box-sizing`
│   ├── Normalize.css........Default normalize styles
│   ├── Reset................Modern reset
│   └── Measure..............Prevent extensive line-length with a style axiom
│
│ 
├── VENDOR
│   Includes of vendor styles for third party components
│   Try to include only low specificity styles
│ 
│ 
├── ELEMENTS
│   │ Styling for bare HTML elements (like H1, A, header, footer, …)
│   │ Redefine browser presets to the projects needs and pursued design system
│   │
│   ├── Fontface.............@font-face declarations
│   ├── Root.................Custom Properties
│   ├── Page.................Page-level styles (HTML element).
│   ├── Headings.............Heading styles.
│   ├── Links................Hyperlink styles.
│   └── Quotes...............Styling for blockquotes, etc.
│ 
│ 
├── OBJECTS
│   Class-based selectors which define composition and undecorated design patterns.
│   An object (CUBE CSS Methodology: Utility) does one job and does that job well.
│ 
│ 
├── COMPONENTS
│   Specific UI components which are to complex to be defined by atomic CSS utilities
│   Uses BEM for naming
│ 
│ 
├── SCOPES
│   Styling for bare HTML elements in a scoped context
│   Eg. for Markdown output from CMS
│ 
│ 
├── UTILITIES
│   Atomic CSS utilities
│   Included last to also be able to serve as component modifiers
│ 
│ 
├── HELPERS
│   Helper classes for JavaScript calculation
│   Eg. for determining the scrollbar width
│ 
│ 
└── DEVELOPMENT
    Debugging and Development components and styles
    Eg. for showing a layout grid
```

### Utility Classes

Utility classes are a powerful ally in combatting CSS bloat and poor page performance. A utility class is typically a
single, immutable property-value pairing expressed as a class. Their primary appeal is speed of use while writing HTML
and limiting the amount of custom CSS you have to write.

Depending on the project utility classes might be created by hand, with SASS mixins or by the use of frameworks
such as TailwindCSS. As you create your own classes make sure to stick to the naming convention of TailwindCSS.

*Utility classes are especially nice for:*
- font and typographic styles
- text-alignment
- font and background colors
- box-shadows
- element spacing
- screen-reader visibility
- responsive display (eg .lg:hidden)

*Utility classes could also be used for:*
- paddings and margins
- display and position
- width and height
- simple flex-box and grid declarations
- simple transitions and animations
- and much more …

*Utility classes should be avoided for:*
- focus/hover effects
- advanced animations/effects
- styling variants of a component

Utility classes speed-up the implementation process and make it easier to customize things. But there are some downsides 
to a utility first approach, which are the reasons, why we are not going all in on them:

- utilities mostly ignore the power of the cascade
- extensive utility class groups are hard to read and maintain
- design systems can often better be handled with components and CSS Custom Properties
- pseudo-elements are a powerful tool, which are hard to implement with utilities only 
- unique motion effects are hard to archive
- utility class groups might be fragile if multiple styles work together and rely on each other
- semantic and meaningful class names might sometimes be a better choice (eg. in a BEM modifier)

### Objects
  
An `Object` does one job and does that job well (Single Responsibility Principle). An `Object`, more often than not, 
will only have a few CSS properties defined, while a `Component` deals with more complex setups. 
However the borders between a `Component` and an `Object` are blurred. So stop overthinking, whether the piece of code 
you are about to create is a `Component` or an `Object`.

```scss
// _objects.cluster.scss

/**
* Provide a flex container in order to display items side by side
*
* Can be used for elements with varying width
* or adjusted widths via the .width utility class
*
* 1. Use --column-gap as default for cluster
* 2. Settles the outer gap margin of inner elements
* 3. Lets the cluster be multiline
* 4. Centers each row. Change alignment with flex utility class
* 5. Defines a gap for children
*/

.cluster {
  --cluster-gap: var(--column-gap, 1rem); // [1]
  margin-left: calc(var(--cluster-gap) * 0.5 * -1); // [2]
  margin-right: calc(var(--cluster-gap) * 0.5 * -1); // [2]
  display: flex;
  flex-wrap: wrap; // [3]
  align-items: center; // [4]
}

.cluster > * {
  margin-left: calc(var(--cluster-gap) * 0.5); // [5]
  margin-right: calc(var(--cluster-gap) * 0.5); // [5]
}
```

### Components and BEM

- Create reusable components for more complex common visual or behavioural patterns
- Don’t style objects based on their context. An object should look the same no matter where you put it
- Use `BEM` to style `blocks` containing `elements` to be altered with `modifiers`  
- Separate words within names by hyphens `-`
- Delimit elements by double underscores `__`
- Delimit modifiers by double hyphens `--`
- Use the same naming convention for grandchildren. So no `.block-name__child__grand-child`-craziness
- Use `has-` or `is-` prefix for the state
- Do not use Tailwind’s `@apply` to create components
- Create a single SASS file for every component

**Don’t**

```html 
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
```

**Do**

```html 
<!--    
 * Going for a object-oriented BEM approach makes the following code self-documenting 
 *
 * We can see which classes are and are not related to each other, and how
 * We know what classes we can’t use outside of the scope of this component.
 * Also, we know which classes we are free to reuse elsewhere.
 -->     
<div class="box profile profile--pro-user">
    <img class="avatar profile__image" />
    <p class="profile__bio">...</p>
</div>

/* Do: Use utility classes for common micro patterns */
.lg:hidden {
    @media screen and (min-width: 599px) {
        display: none;
    }
}

/* Do: Prefix state classes to tell what is going on */
.is-activated {
    display: block;
}
```

`Components` respectively `Blocks` are logically and functionally independent components of a web page. 
They are nestable and should be capable of being contained inside another block without breaking anything. 
`Elements` are the constituent parts of a block that can’t be used outside of it. 
`Modifiers` define the appearance and behavior of a block.

### Scoped Components

If you are working with a JS framework you might be styling your components directly inside the main component file. This
is a valid way of defining scoped styles for a specific component. In this case the components layer should be removed
from the global SASS setup and all component styles should be handled like this.

Nevertheless always take good care while deciding which styles should be defined inside the component itself and when
to reach out for global objects or utility classes.

### Responsiveness / Mobile first

- Make sure everything is entirely responsive
- Default your CSS for less capable devices
- Style components for the smallest viewport first (mobile first)
- Overwrite styles to adapt to larger viewports via media queries
- Don’t use media queries for the default view (smallest viewport) unless these styles are just an enhancement and
  overwriting for larger viewports will lead to extensive effort

Styling the most likely simpler component structure on mobile devices first will lead to less and simplified code and
fewer overwrites. Leaving out everything within media queries the code can be parsed much faster by mobile devices—and
devices with few capabilities such as e-book readers will see a simple default view.

### Design Tokens

> Design Tokens are the visual atoms of the design system – specifically, 
> they are named entities that store visual design attributes. 
> We use them in place of hard–coded values in order to maintain a scalable and consistent visual system.

*[Jina Anne](https://twitter.com/jina)*

In order to build up a solid and scalable design system we implement a single source of truth for abstracted
values such as colors, spacing values and typographic text styles. These values could either be defined outside 
of our context (in a database or even a JSON file) or with simple CSS maps in our CSS settings layer.

```scss
// _settings.tokens.scss
// Global design tokens

// SIZE SCALE
//
// We use a modular scale that powers all the utilities that
// it is relevant for (text styles/font-size, margin, padding).
// All items are calculated off these tokens.
//
// OUR APPROACH
// Instead of going into complicated SASS Mixins or Calc operations
// we use this tool to get the values for a chosen type of scale:
// https://type-scale.com/
//
// NAMING CONVENTION
// Naming is taken from font weight where a weight of 400 is considered normal
//
// SPECIFIC PROJECT SETTINGS
// Every project has a specific size scale

$size-scale: (
  '25': 0.1rem,
  '50': 0.25rem,
  '100': 0.5rem,
  '200': 0.64rem,
  '300': 0.8rem,
  '400': 1rem,
  '500': 1.333rem,
  '600': 1.777rem,
  '700': 2.4rem,
  '800': 3.9rem,
  '900': 5.6rem,
  '1000': 7.8rem,
  '1100': 11.2rem,
);

// COLORS
// Set up a colour palette which allows us
// to theme the entire project from one location.

// Text colors
$text-colors: (
  'default': #000,
  'highlight': #ff854d,
);

// Background colors
$bg-colors: (
  'default': #f8f7f2,
  'grey': #edece7,
);

// Border colors
$border-colors: (
  'default': rgba(0, 0, 0, 0.15),
);

// Border colors
$border-radius: (
  's': 0.1rem,
  'm': 0.2rem,
  'l': 0.5rem,
);

// Box shadow
$shadows: (
  'modal': 0 0 6rem rgba(0, 0, 0, 0.15),
  'box': 0 0 4rem rgba(0, 0, 0, 0.1),
);

// TEXT STYLES
//
// Text styles will be created as utility classes with the 
// text-style CSS mixin
//
// .usage {
//   @include text-style(100);
// }
//
// If key in $text-styles map matches size from $size-scale
// font-size is automatically set to this value

$text-styles: (
  // Default Text Style
  'default': (
      font-weight: 400,
      line-height: 1.2;
      font-family: "'PXR Repro', Arial, 'Helvetica Neue', sans-serif;",
      letter-spacing: 0;
  ),

  // Small Text
  '300': (
    line-height: 1.235,
    letter-spacing: 0.015em,
  ),

  // Body Text Size
  '400': (
    line-height: 1.2,
  ),

  // Text M
  '500': (
    font-weight: 300,
    line-height: '1.147',
  ),

  // Text L
  '600': (
    font-weight: 300,
    line-height: '1.104',
  ),

  // Medium
  'medium': (
    font-weight: 500,
  ),
);
```

One dimensional token values such as colors and sizes should be made available to the frontend 
by adding them as `Custom Properties` to the `root`. These values can then be used within `Components`, 
`Objects` and `Utilities`. They have a very low specificity and therefore can easily be 
overwritten for (responsive) modifications.  

```scss
// _settings.tokens.scss
$size-scale: (
  '25': 0.1rem,
  '50': 0.25rem,
  '100': 0.5rem,
  '200': 0.64rem,
  '300': 0.8rem,
  '400': 1rem,
  '500': 1.333rem,
  '600': 1.777rem,
  '700': 2.4rem,
  '800': 3.9rem,
  '900': 5.6rem,
  '1000': 7.8rem,
  '1100': 11.2rem,
);

// _tool.mixins.scss
@mixin create-custom-properties($map, $prefix: '') {
  @each $prop, $value in $map {
    --#{$prefix}#{$prop}: #{$value};
  }
}

// _elements.root.scss
:root {
  // Add custom properties for colors
  @include create-custom-properties($size-scale, 'size-');

  // Creates these properties from design tokens setting
  --size-25: 0.1rem;
  --size-50: 0.25rem;
  --size-100: 0.5rem;
  ...
}
```

If our project uses TailwindCSS we can use these global values for our Tailwind classes as well. 
Simply specify them in the Tailwind configuration file:

```scss
// tailwind.config.js
module.exports = {
  theme: {
    spacing: {
      '25': 'var(--size-25)',
      '50': 'var(--size-50)',
      '100': 'var(--size-100)',
      ...
    }
  }
}
```

### Global Values

There also is a need to define project-level values other than `Design Tokens`. Values that need to be open for 
(responsive) modifications (eg. layout settings) should be available as `Custom Properties`. They are stored inside
the settings layer as `CSS maps` and added to the `:root` via a `SASS mixin`:

```scss
// _settings.ui.scss

// Project-level settings for ui
$ui-props: (
  // default values
  'default': (
      // Default definition for .page-grid
      'grid-rows': 16,
      'column-gap': 0.5rem,

      ... 
  ),

  // breakpoint md values
  'md': (
      // Default definition for .page-grid
      'column-gap': 0.707rem,

      ...
  ),
);

// _elements.root.scss      
:root {
  // Add custom properties for ui definitions
  @include create-custom-properties($ui-props, true);
}
```

Some values such as transitions timings and easings or default box ratios only need to be available 
within the preprocess. They may be stored as static [`SASS variables`](#variables):

```scss
// _settings.vars.sass

// TRANSITIONS
// Transition timing
$trans-time--xs: 0.125s;
$trans-time--s: 0.25s;
$trans-time--m: 0.5s;
$trans-time--l: 1s;

// Transition easing
$trans-func--default: cubic-bezier(.1,.6,.4,1);
$trans-func--ease-in-out: cubic-bezier(.55,.08,0,1);
$trans-func--ease-out: cubic-bezier(0,.23,.07,1)

// ASPECT RATIO    
$landscape-ratio: (3 / 4 * 100%); // 4:3
$wide-ratio: (1 / 2 * 100%); // 2:1
$portrait-ratio: (3 / 2 * 100%); // 2:3
$cube-ratio: 100%;
```

### Class Grouping

By the use of `Utilitìes`, `Objects` and `Components` side by side there might be a lot of classes 
defined for a single element. Therefore we recomment grouping things with pipes, like so:

```html 
<article class="card | flow | bg-base color-secondary">
  <h2 class="text-500 | sticky top-0 | js-is-sticky">
    Card Headline
  <h2>
  ...
</article>
```

- Do not add pipes for empty groups
- Use the following order:
  - `Components`
  - `Objects`
  - `Utilities`
  - `JS-Hooks`

### Componentless Modules

There will be simple modules which can be layed out with `Utilities` and `Objects` alone. Finding these modules
within the codebase by inspecting the frontend inside the browser is painful – but adding a component class with 
no styles attached to overcome this issue seems like bad practise. We recommend adding a `data` attribute, like so:

```html 
<div class="flow | flex flex-col" data-ui-name="text-box">
  ...
</div>
```

In order to stay consistent we add these `data` attributes to `Components` with dedicated classes as well. 
The naming attributes may be stripped for production.

## CSS Codestyle

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

```html 
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
.video
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
```

**Do**

```scss
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

.video {
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
```

### CSS Quotation Marks

- Use single (`''`) quotation marks for attribute selectors and property values.
- Do not use quotation marks in URI values (`url()`).
- Exception: If you do need to use the `@charset` rule, use double quotation 
  marks—[single quotation marks are not permitted](https://www.w3.org/TR/CSS21/syndata.html#charset).

**Don’t**

```scss
@import url("https://www.google.com/css/maia.css");

html {
  font-family: "open sans", arial, sans-serif;
}
```

**Do**

```scss
@import url(https://www.google.com/css/maia.css);

html {
  font-family: 'open sans', arial, sans-serif;
}
```

### Selector Order and Nesting

- **Do not nest selectors unnecessarily**
- **Do not nest selectors unnecessarily**
- **Do not nest selectors unnecessarily**
- Add self-declarations first
- Add pseudo-classes next
- Add modifier and state declarations next
- Add responsive styles for every selector
- Avoid generating new selectors from the current selector reference (`&`)

**Don’t**

```scss
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
@include respond-to(lg) {
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
```
    
**Do**

```scss
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
    
    /* Do: Add responsive styles for every selector */
    @include respond-to(lg)  {
        // styles for desktop header navi
    }
}
    
/* Do: Prevent modifier nesting for low specificity */
&.header__nav--small {
    // styles for modified header
}  
    
.header__nav-link {
    // styles for links inside header navi
    
    @include respond-to(lg)  {
        // styles for desktop links inside header navi
    }
}
```
    
As it comes to responsiveness we usually are dealing with a main `mobile` and a `decent` breakpoint. The `decent` breakpoint covers every viewport larger than 599 pixel. While the default styles will define the mobile view, the decent styles will be applied with the first media query block. To distinguish between different `decent` viewports such as hudge phones, tablets and very big screen or portrait viewports as well, more precise sets of media queries will be added afterwards.

### Meaningful Whitespace

- Make use of whitespace between rulesets to structure code (optional)
- Put four blank lines before new sections (or three? Just be consistent)

**Do**

```scss
/* .foo */    
.foo { }

.foo__bar { }

.foo--baz { }
 


/* .bar */    
.bar { }

.bar__baz { }

.bar__foo { }
```

### Selectors

- Never style IDs
- Style type selectors for reset purposes only
- Avoid qualifying class names with type selectors. Put the class name at the lowest possible level
- Use combined selectors wisely
- Select what you want explicitly, rather than relying on circumstance or coincidence
- Write selectors for re-usability
- Do not qualify selectors unnecessarily, as this will impact the number of different elements you can apply styles to
- Keep selectors as short as possible, in order to keep specificity down and performance up

**Don’t**

```scss
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

/* Don’t: Overcomplicate your code 
 * 
 * If your code looks like this you might want to think about adding a class to your HTML 
 */
div:nth-of-type(3) ul:last-child li:nth-of-type(odd) * { 
    // style some magic list-item
}   
```

**Do**

```scss
/* Do: Use classes instead of IDs or type selectors 
 * Do: Be specific about what you are styling */
.header__nav {
    // styles for header navi
}

/* Do: Use specific, reusable classes. Put the class name at the lowest possible level */
.btn--promo { 
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
```

### ID and Class Naming

- Prefer dashes over camelCasing in class names 
- Use meaningful or generic ID and class names
- Use ID and class names that are as short as possible but as long as necessary
- Make use of BEM

**Don’t**

```scss
/* Don’t: meaningless */
#yee-1901 {}

/* Don’t: presentational */
.button-green {}
.clear {}

/* Don’t: too long, not readable */    
#navigation {}
.atr {}
```

**Do**

```scss
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
```

### Breakpoints

- Think about clever responsive solutions before reaching out for media queries
  (look into [math functions](https://caniuse.com/css-math-functions), grid and flex-box)
- Style components for the smallest viewport first ([mobile first](#responsiveness-mobile-first))
- Use min width media queries to progressive enhance your layout for bigger screens
- Use [`em` values](https://zellwk.com/blog/media-query-units/) to define media queries. `em` based media queriess
  favor in default font size set by user

**Do**

```scss
// _settings.breakpoints.scss

// breakpoint definitions
$breakpoints: (
  // default breakpoints are treated as min width screen media queries
  default: (
    'md': 37.5em, // 16px * 37.5em: 600px
    'lg': 64em, // 16px * 64em: 1024px
  ),
  
  // custom breakpoints output the raw definition
  custom: (
    'portrait': 'screen and (orientation: portrait)',
    'landscape': 'screen and (orientation: landscape)',
    'wide': 'screen and (min-aspect-ratio: 16/9)',
    'tower': 'screen and (max-aspect-ratio: 2/3)',
    'print': 'print',
  ),
);

// respond-to mixin
@mixin respond-to($breakpoint) {
  @each $type in map-keys($breakpoints) {
    $breakpoint-group: map-get($breakpoints, $type);
  
    @if map-has-key($breakpoint-group, $breakpoint) {
      $value: map-get($breakpoint-group, $breakpoint);

      @if $type == default {
        // default breakpoints are treated as min width screen media queries
        @media screen and (min-width: $value) {
            @content;
        }
      } @else {
        // custom breakpoints output the raw definition
        @media #{$value} {
            @content;
        }
      }
    }
  }
}

// usage
.box {
  width: 100%;
  
  @include respond-to(md) {
    width: 50%;
  }
  
  @include respond-to(lg) {
    width: 33%;
  }
}
```

Breakpoint specific styles should be nested in the root of the specific class. Every breakpoint has its own single
block of code. They are coming after the default mobile declarations and are ordered from small to large.
Custom breakpoints (eg. orientation queries) come last.

**Don’t**

```scss
/* Don’t: State media queries outside a parent class */
.box {
  width: 100%;
}

@include respond-to(md) {
  .box {
    width: 50%;
  }
}

/* Don’t: Create more than one block per breakpoint / Nest media queries too deep */
.box {
  width: 100%;
  
  .modal & {
    @include respond-to(md) {
        width: 80%;
    }
  }

  .form & {
    @include respond-to(md) {
      width: 50%;
    }
  }
}
```

**Do**

```scss
/* Don’t: Create more than one block per breakpoint */
.box {
  width: 100%;

  @include respond-to(md) {
    .modal & {
        width: 80%;
    }
    
    .form & {
      width: 50%;
    }
  }

  @include respond-to(lg) {
    width: 25%;
    
    .form & {
      width: 33%;
    }
  }
  
  @include respond-to(port) {
     width: 80%;
   }
}
```

### JavaScript Hooks

Avoid binding to the same class in both your CSS and JavaScript. This is because doing so means you can’t have 
(or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable 
to bind your JS onto specific classes.

- Create JavaScript-specific classes to bind to, prefixed with `.js-`
- Don’t use data-Attributes for this purpose

**Do**

```html 
<button class="btn btn-primary | js-request-to-book">Request to Book</button>
```
    
A common practice is to use data-* attributes as JS hooks, but this is incorrect. data-* attributes, as per the spec, 
are used to store custom data private to the page or application (emphasis mine). data-* attributes are designed to 
store data, not be bound to.
 
### Hacks

- Avoid user agent detection as well as CSS “hacks”—try a different approach first
- If you need to use hacks anyways, make sure to [comment](#comments) extensively

## Best Practises

### Relative Units

- Avoid pixels, they are ignorant
- Use relative units like `rem` and `em` instead
- Use unitless values for `line-height`
- Use `rem` for font and layout sizes to be resized via the `Html` root font-size
- Do not forget about less common units such as 'ch' or 'ex'

**Don’t**

```scss
/* Don’t: Use pixel values */
.headline {
  font-size: 18px;
  line-height: 27px;
  width: 300px;
}   
```

**Do**

```scss
/* Do: Use relative units */
.headline {
  font-size: 1.8rem;
  line-height: 1.5;    
  max-width: 48ch;
  width: 80%;
  margin-top: 1rem;
}
```

There are some few exceptions for the use of pixel values such as 1px borders.

### Custom Properties

Custom properties (often referred to as CSS Variables) are exceptionally useful and 
much more powerful that SASS variables. Here are some great benefits:

- can be contextually overridden (eg. by style scoping or media queries)
- can be set dynamically with JavaScript
- have a very low specificity
- leverage the cascade
- create a bridge between HTML and CSS (making it easy to import settings from a CMS into the CSS layer)
- can have a fallback
- are the No. 1 choice for design tokens and design system implementations
- makes it super easy to implement custom style and color themes
- are widely supported (excluding IE)

With Custom Properties we can eliminate redundant styling and the need to write the same code over and over again.

**Before**

```scss
.button {
  display: inline-block;
  padding: 0.5rem 1rem;
  color: black;
}

.button--primary {
  background: red;
  box-shadow: inset 0 0 2px red;
}

.button--secondary {
  background: blue;
  box-shadow: inset 0 0 2px blue;
}
```

**After**

```scss
:root {
  // Define global default colors as a design system
  // to be used anywhere in the codebase
  --text-default: black;
  --text-primary: red;
  --text-secondary: blue;
  --bg-default: white;
  
  // Create a darkmode version of the design system
  @media (prefers-color-scheme: dark) {
    --text-default: white;
    --bg-default: black;
  }
}

body {
  color: var(--text-default); // Will be black by default - and white on devices with darkmode switched on
  background: var(--text-default); // Will be white by default - and black on devices with darkmode switched on
}

.button {
  display: inline-block;
  padding: .5em 1em;
  color: var(--text-btn, inherit); // Using the inherited default color as fallback 
}

.button--primary {
  --text-btn: var(--text-primary); // Conditionally override default with global primary color
}

.button--secondary {
  --text-btn: var(--text-secondary); // Conditionally override default with global secondary color
}
```

There are much more advanced solutions and possibilities than this simple button component and darkmode implementation.
For a deeper understanding, lots of compelling examples and further readings make sure to visit the 
[Complete Guide to Custom Properties](https://css-tricks.com/a-complete-guide-to-custom-properties/).

### Spacing

In order to visually and conceptually separate flow elements we make use of the margin property. Since there currently
is no way in CSS to style elements based on their proceeding elements its is good practise to only use top margin for 
this purpose. This approach gives us the possibility to make the size of the space depenend on the prior element by
using adjacent sibling selector.

```SCSS
<main>
  <article></article>
  <article></article>
  <div class="ad"></div>
  <article></article>
  <article></article>
</main>
  
article {
  margin-top: 1rem;
}
  
.ad {
  margin-top: 2rem;
} 

.ad + article {
  margin-top: 2em;
}
```

Instead of creating specific classes to handle the spacing we would rather use a utility class  for individual top 
spacing rules.

```html
<main>
    <article></article>
    <article class="mt-100"></article>
</main>
```

Most of the times the spacing of an element should not be handled by element itself but by the context it is living in.
For one it is a lot of work to define individual spacing for every element – be it with utility classes or within a
BEM component. But also does not make a lot of sense from a conceptual point of view either. Think of a component, which
might be displayed in various parts of a side or an app. The needed space around it might differ greatly and it will
be quite hard to make and define these decisions on a component level. So we should always avoid margin on component 
wrapper.

Therefore we highly recommend the use of a flow object (also referred to as [The Stack](https://every-layout.dev/layouts/stack/)).
With this layout primitive margins are injected via their common parent. You can easily create per-element exceptions 
within a single flow context without running into specificity issues.

```scss
// Basic Flow Object
.flow > * + * {
  margin-block-start: var(--flow-space, 1.5em);
}

.flow-exception,
.flow-exception + * {
  --flow-space: 3rem;
}

// Advanced Flow Object
// with predefined spacing sizes via CSS Map $size-scale
//
// 1. Class for parents with spaced children
// 2. Flex declaration lets us group elements to the top and bottom
//    of the vertical space with a margin-bottom: auto on a child.
// 3. Define specific margins via custom properties
// 4. Spaced children: All but the first get a top margin
.flow-50,
.flow-100,
.flow-200,
.flow-300,
.flow-400,
.flow-500,
.flow-600 {
  display: flex; // [2]
  flex-direction: column; // [2]
  justify-content: flex-start;
}

.flow-50 > * + *, // [4]
.flow-100 > * + *,
.flow-200 > * + *,
.flow-300 > * + *,
.flow-400 > * + *,
.flow-500 > * + *,
.flow-600 > * + * {
  margin-top: var(--flow-space);
}

.flow-50 > * + *  { --flow-space: #{map-get($size-scale, "50")};} // [3]
.flow-100 > * + *  { --flow-space: #{map-get($size-scale, "100")};} // [3]
.flow-200 > * + *  { --flow-space: #{map-get($size-scale, "200")};} // [3]
.flow-300 > * + *  { --flow-space: #{map-get($size-scale, "300")};} // [3]
.flow-400 > * + *  { --flow-space: #{map-get($size-scale, "400")};} // [3]
.flow-500 > * + *  { --flow-space: #{map-get($size-scale, "500")};} // [3]
.flow-600 > * + *  { --flow-space: #{map-get($size-scale, "600")};} // [3]

```

### Modular Scale

We are aiming for harmony in our visual layouts. Therefore we tend to create design systems where each text size and 
spacing value is based on a modal scale. 

A modular scale is a sequence of numbers related to each other through a specific ratio. It can be created by multiplying 
each subsequent number by the same predefined ratio. The base value is usually our default text size of 1rem. 
This base size paired with a chosen ratio (such as the golden ratio or any musical proportion) will create a scale of 
values which all share this proportion.

In order to create a size scale based on a specific ratio you can use tool such as [Modular Scale](https://www.modularscale.com/)
or [Type Scale](https://type-scale.com/).

Our naming convention is taken from font weights where a weight of 400 is considered normal. This is easy to relate to 
and offers great flexibility for later on created bigger, smaller or in-between values.

```SCSS
// This project uses a Perfect Fourth scale above 1rem
// meaning the ratio between values is 1.333
// Values below are handpicked
$size-scale: (
    '25': 0.1rem,
    '50': 0.25rem,
    '100': 0.5rem,
    '200': 0.64rem,
    '300': 0.8rem,
    '400': 1rem,
    '500': 1.333rem,
    '600': 1.777rem,
    '700': 2.4rem,
    '800': 3.9rem,
    '900': 5.6rem,
    '1000': 7.8rem,
);
```

### Type Styles

In a design system every type size should be accompanied with a matching line-height. While line-heights should be
determined in a relative manner anyways (eg. `line-height: 1.2`) they still need some treatment to feel visually aligned.
Smaller text are in need of bigger line-heights while bigger text should be set more tight to create the same feeling
of light. This is also true for letter-spacing: Smaller text need more room to breath, letters from bigger texts should
come closer together.

Therefore a typographic treatment is always a predefined combination of size, line-height and letter-spacing. To
reflect this part of our design system we create text style utility classes. These text styles then work just
like pharagraph styles known from text editors or design software. The naming convention and the predefined type sizes
make use of the modular scale, which is also defined via our Design Tokens library.

```scss
// SCSS Map combining custom settings 
// with predefined sizes from modular scale
$text-styles: (
    '300': (
        line-height: 1.235,
        letter-spacing: 0.015em,
    ),
    '400': (
        line-height: 1.2,
        letter-spacing: 0.015em,
    ),
    '500': (
        line-height: '1.147',
        letter-spacing: 0.013em,
    ),
    '600': (
        line-height: '1.104',
        letter-spacing: 0.01em,
    ),
    '700': (
        line-height: '1.045',
        letter-spacing: 0,
    ),
    '800': (
        line-height: '1.042',
        letter-spacing: -0.015em,
    ),
);

// Mixins to generate text styles
@mixin text-style($name) {
  @if map-has-key($text-styles, $name) {

    // Get size from $size-scale if $name matches a size here
    @if map-has-key($size-scale, $name) {
      font-size: map-get($size-scale, $name);
    }

    // Get defined properties
    $style: map-get($text-styles, $name);

    // Add defined properties
    @each $property, $value in $style {
      #{$property}: #{$value};
    }
  }
}

// Create utilities for text layout formats
@each $name in map-keys($text-styles) {
  .text-style-#{$name} {
    @include text-style($name);
  }
}
```

### Fluid Typography

For implementing responsive typography it used to be common practise to define explicit type sizes and change these 
values on specific predefined breakpoints. But – as we know – there are a lot of different devices and screen sizes 
around and it seems like a bad idea to only optimize for a very limited and random selection of assumed common devices.

Viewport units give us the power to adapt our type scaling to the size of the client’s browser window. The idea
of changing font sizes based on the screen size is often referred to as *Fluid Typography*. This technique not only
enables us to optimize our typographic system for all screen sizes with a single line of code (at best) and without the 
use of hard breakpoints (if any at all) – it also helps us to very gradually fine-tune the type scaling in real 
device scenarios later on.

Fluid Typography scales smoothly between a minimum and (optionally) a maximum value. This dynamic value is calculated 
by combining a specific fraction of the view width (`vw`) with a static `rem` value. We do this because viewport units 
are much too responsive to the screen’s width to be used as the only relation and we would also face some significant 
accessibility issues with the sole use of viewport units: Since these values exclusively depend on the screen size, user 
preferences would not be taken into account and zooming of the page would be preventet.

```scss
// Simple and (mostly) accessible fluid typography
html {
  // 1. em or rem does not matter here
  // 2. rem-size should not go much below 1rem to keep browser default in sync
  // 3. Increase or decrease zoom factor to define zoom distinction
  // 4. The min() function gives the smallest value from two passed parameters
  // 5. clamp() isn't necessary, since the first part already makes --baseFontSize the min
  // 6. calc() isn’t necessary in min()
  --baseFontSize: 0.9rem; // [1][2]
  --zoomFactor: 0.7vw; // [3]
  --maxFontSize: 3rem; // [1]
  
  // font-size: min(0.9rem + 0.7vw, 3rem);
  font-size: min(var(--baseFontSize) + var(--zoomFactor), var(--maxFontSize)); // [4][5][6]

  // Fine-tune scaling for other breakpoints
  // by redefining custom properties
  @include respond-to(md) {
    --baseFontSize: 0.7rem;
    --zoomFactor: 0.5vw;
  }
}
```

Since we implement this dynamic value as the font size for the html element, all other font-size settings can relate to 
the base value via relative `rem` values and the concepts of [Type Styles](#type-styles) and 
[Modular Scale](#modular-scale). Although initially invented to gain more typographic excellence, this fluid sizing 
approach also works for margin, padding, gaps and everything else we define via `rem`.

There are more complex solutions around, which will not only align the base font size with the viewport but also make
the modular scale fluid as well ([eg. Utopia.fyi](https://utopia.fyi/)). By adding fluidity to the modular scale we can 
adjust the contrast in hirarchic type size setups for different screen sizes: Smaller distinctions for mobile 
devices, greater accentuations for desktop screens. 

How complex you would like to go while implementing fluid typography always depends on the project and the 
proposed design system.

### Preferes Reduced Motion

Respecting user preferences is key for modern websites – especially if they are set for accessibility reasons. 
Therefore we prevent animations and animated scroll behaviour right in our default `reset.scss` if the user has 
expressed their preference for reduced motion.

```scss

/* Remove all animations and transitions for people that prefer not to see them */
@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }
  
  // [1] We do not prevent the transitions or animations entirely
  //     but make them hyper short for animationend events to still
  //     work in JavaScript
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important; // [1]
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important; // [1]
    scroll-behavior: auto !important;
  }
}
```

Please also keep these things in mind as you are coding up some nice transitions in Vanilla JS or GSAP.

## Preprocessors/SASS

### SASS Syntax and Formatting

- Never write pure CSS, always use well structured SASS
- Use the `.scss` syntax, never the original `.sass` syntax
- Wrap strings with single quotes `'`
- Don’t wrap CSS values, which require not to be quoted
- Add leading zeros before a decimal value
- Don’t add units to 0 values when dealing with lengths
- Wrap top-level numeric calculations in parentheses
- Avoid the use of magic numbers

**Don’t**

```scss
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
```

**Do**

```scss
/* Do: Wrap strings with single quotes */
$direction: 'left';

/* Do: Skip quoting on some specific values */
$font-type: sans-serif;

/* Do: Use 0 values without units */
$length: 0;

/* Do: Wrap numeric calculations in parentheses */
.foo {
    width: (100% / 3);
}
```

### File Naming

SASS files should be prefixed by its layer, eg. `_settings.z-index.scss`. This will lead to more clarity
while handling files (there also might be a `_utilities.z-index.scss`).

### Variables

- Initiate variables the value is repeated at least twice and the value is likely to be updated
- Use dash-cased variable names (e.g. `$my-variable`)
- Store all variables in the `_settings.vars.scss`
- Prefix variables with `_` (e.g. `$_my-variable`), if you want to put it inside a component 
  file and only use it there

### Extend

- Avoid `@extend`

Extending is invisible. Extending doesn’t necessarily help file weight, contrary to the saying. 
Extending does not work across media queries. Extending is not flexible. Mixins have absolutely no drawback.

## PurgeCSS

If you are using a CSS framework such as TailwindCSS make sure to use [PurgeCSS](https://purgecss.com/) 
for production builds (the `purge`-option is already included in Tailwind). PurgeCSS removes unused selectors 
from your CSS, resulting in much smaller CSS files.

If you are using `purge` take good care to write purgeable HTML. That means that it is important to avoid 
dynamically creating class strings in your templates with string concatenation, otherwise PurgeCSS won't know to 
preserve those classes.


## Sources and further reading

### Styleguides

- [cssguidelin.es](https://cssguidelin.es/)
- [codeguide.co](http://codeguide.co/))
- [Google](https://google.github.io/styleguide/htmlcssguide.html)
- [Idiomatic CSS](https://github.com/necolas/idiomatic-css)
- [Airbnb](https://github.com/airbnb/css)

### Methodologies

- [ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/)
- [CUBE CSS](https://piccalil.li/blog/cube-css)
- [Modular CSS](https://spaceninja.com/2018/09/17/what-is-modular-css/)
- [Building a Scalable CSS Architecture With BEM and Utility Classes](https://css-tricks.com/building-a-scalable-css-architecture-with-bem-and-utility-classes/)

### Accessibility

- [The Web Accessibility Introduction I Wish I Had](https://dev.to/maxwell_dev/the-web-accessibility-introduction-i-wish-i-had-4ope)
- [The A11Y Project Checklist](https://a11yproject.com/checklist)
- [Mobile, Small, Portrait, Slow, Interlace, Monochrome, Coarse, Non-Hover, First](https://css-tricks.com/mobile-small-portrait-slow-interlace-monochrome-coarse-non-hover-first/)
- [prefers-reduced-motion: Sometimes less movement is more](https://web.dev/prefers-reduced-motion/)

### Utility first vs. BEM

- [Building a Scalable CSS Architecture](https://www.algolia.com/blog/engineering/redesigning-our-docs-part-4-building-a-scalable-css-architecture/)
- [CSS Utility Classes and "Separation of Concerns"](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/)
- [Why Tailwind Isn't for Me](https://dev.to/jaredcwhite/why-tailwind-isn-t-for-me-5c90)
- [Let There Be Peace on CSS](https://didoo.medium.com/let-there-be-peace-on-css-8b26829f1be0)
- [In Defense of Utility-First CSS](https://frontstuff.io/in-defense-of-utility-first-css)

### Slow Web

- [The Cost of Javascript Frameworks](https://timkadlec.com/remembers/2020-04-21-the-cost-of-javascript-frameworks/)

### Best Practices

- [It’s time we say goodbye to pixel units](https://uxdesign.cc/say-goodbye-to-pixels-cb720fbaf250)
- [PX, EM or REM Media Queries?](https://zellwk.com/blog/media-query-units/)
- [A Complete Guide to Custom Properties](https://css-tricks.com/a-complete-guide-to-custom-properties/)
- [The Power (and Fun) of Scope with CSS Custom Properties](https://css-tricks.com/the-power-and-fun-of-scope-with-css-custom-properties7)
- [The Stack](https://every-layout.dev/layouts/stack/)
- [Managing Flow and Rhythm with CSS Custom Properties](https://24ways.org/2018/managing-flow-and-rhythm-with-css-custom-properties/)

### Modular Scale and Fluid Typography

- [Type Scale](https://type-scale.com/)
- [Every Layout: Modular Scale](https://every-layout.dev/rudiments/modular-scale/)
- [Everything I know about Responsive Web Typography](https://zellwk.com/blog/responsive-typography/)
- [Viewport Unit Based Typography](https://zellwk.com/blog/viewport-based-typography/)