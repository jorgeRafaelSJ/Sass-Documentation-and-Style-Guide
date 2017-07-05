# Sass-Documentation-and-Style-Guide

##### Sass === Syntactically Awesome StyleSheets

##### What is a CSS preprocessor?

A scripting language that extends CSS by allowing developers to write code in one language and then compile it into CSS. Some examples of CSS preprocessor include: Sass and LESS. [Why Sass?](https://www.keycdn.com/blog/sass-vs-less/)

### Setup

Download and Install: https://marketplace.visualstudio.com/items?itemName=MadsKristensen.WebCompiler

The configuration file is already set as compilerconfig.json. It will take the input in the Sass folder and compile into the output file located in the css folder. If you modify the output css file all your changes will be deleted when Sass is recompiled.

### Naming Guide

* Dashes instead of camelCase!
  * [READ THIS!](https://csswizardry.com/2010/12/css-camel-case-seriously-sucks/)
  
  ```sass
  //yuck
  .camelCaseCssIsCluttered {}
  
  //good
  .dashes-are-readable {}
  ```

* Dashes instead of underscores!

  ```sass
  //ugh
  .this_requires_more_keystrokes {}
  
  //good
  .this-is-faster {}
  
  /* Start to override Kendo UI's autogenerated ID's. Let's control the dependency, not let it control us. */
  ```

* Semantic & feature related naming
  * [Here's why...](https://maintainablecss.com/chapters/semantics/)
  ```scss
  //gahhhhhh
  .form {} 
  #logo {}
  .errorBody {}
  
  /* What form/logo? What does this selector affect? 
  Unless this is the ONLY form/logo/errorBody in the entire application, be specific. */

  //better
  #registration-form {}
  #main-navbar-logo {}
  .order-validation-error {}
  
  /* If an element is going to be reused THEN find a more flexible name and reorganize accordingly. */
  ```
  ```sass
  //boo
  .navbar-default .navbar-nav > li > a {}

  /* It's unnecessarily long and should be easier to follow. 
     Keep children selectors limited to 2.
    If you're feeling fancy 3 at the most BUT make sure 
    that the parent and the root of the selector is reliably named. */

  //yay!
  .main-navbar-link {}
  
  /* Also this is better for searchability. Too many children makes it really difficult to know
  whats affecting which elements. Also, it makes html much more descriptive in combination with dashes. */ 
    
  ```
  ```sass
  
  //meh
  #welcomeBody, .efmErrorBody, .errorBody, .paragraphBody {}
  
  /* If identical styles are being used with 3 or more different names it means 
  they are a component. Components are good. Take care of the dependency. */
  
  //hooray
  .message-page-paragraph {}
  
  /* All those selectors belong to pages where that <p> tag is the main content
  (i.e. successful registration page, password reset page, error page)
  a good way to handle would be to unify and categorize all those pages as message pages */ 
 
  /* Also notice how non descriptive ".paragraphBody" is. */
  ```
  * [Google's HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
  * [Google's HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
  * [Google's HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
  
### File Structure 

* We will be working from the /sass folder. 
* /sass has the "main.scss" file in which other partials are imported using ```@import './file/path';```
* main.scss is the input file that is configured in compilerconfig.json
* The order of those imports is important so do not modify it. 
* There are two main folders inside /sass. 
  * /sass/modules - Reusable Sass specific functionalities that imported to main.scss before everything else. 
  * /sass/partials - Where our custom styles live. 
* /modules contains files -> colors.scss & mixins.scss. 
  * colors.scss - contains all the Sass color variables that should be used in other partials.
  * mixins.scss - contains all the Sass functions that get executed and filled in at compile time. 
* /partials - contains 2 folders & component files. 
  * Note: When one of the styles in these folders is going to be used more often we need to make it into a component file.
  * /shared - Styles belonging exclusively to a specific html file under /Views/Shared. Named the same. 
  * /views - Styles belonging exclusively to an html file under /Views. Named the same.  
  * Component Files - buttons.scss, links.scss, content-panel.scss. (This is our goal.) 
  
## Sass - What we can do now! 
### [Standard Sass Style Guide](https://css-tricks.com/sass-style-guide/)

* [LEARN MORE...](http://sass-lang.com/guide)
* [Playground](https://www.sassmeister.com/)
* [This is just a cool idea.](https://blog.prototypr.io/sass-maps-to-ui-components-f14e1f34412e)



### Nesting Elements

```sass
//OLD WAY
.very-specific-name {
  width: 100px;
}

.very-specific-name > li {
  padding: 10px 7px;
}

.very-specific-name > li a {
  text-decoration: none;
}

//NEW WAY
.very-specific-name {
  width: 100px;
 
  > li {
   padding: 10px 7px;
  
    a {
      text-decoration: none;
    }
  }
}

/* MAX nesting depth of 3 selectors! Make root selector VERY specific. */ 
```

### &:hover, &:active, &:focus 
```sass
//OLD WAY
.we-have-this-long-selector, .oh-and-this-one-as-well {
  background-color: transparent;
  color: $primary;
}

.we-have-this-long-selector:hover, 
.oh-and-this-one-as-well:hover, {
  background-color: $primary;
  color: $white75;
}

//NEW WAY
.we-have-this-long-selector, .oh-and-this-one-as-well {
  background-color: $white75;
  color: $primary;
  
  &:hover { 
    background-color:$primary;
    color: $white75;
  }
}

/* It will be taken care of when it compiles. Thanks Sass! More maintainable! */ 
```
### Media tag mixins
```sass

//THE GOODS

//max-width media tag mixin
@mixin max($size) {
  @if $size == xs {
    @media screen and (max-width: 330px) {
      @content;
    }
  }
  @else if $size == sm {
    @media screen and (max-width: 576px) {
      @content;
    }
  }
  @else if $size == md {
    @media screen and (max-width: 768px) {
      @content;
    }
  }
  @else if $size == lg {
    @media screen and (max-width: 1200px) {
      @content;
    }
  }
}

//min-width media tag mixin
@mixin min($size) {
  @if $size == xs {
    @media screen and (min-width: 330px) {
      @content;
    }
  }
  @else if $size == sm {
    @media screen and (min-width: 576px) {
      @content;
    }
  }
  @else if $size == md {
    @media screen and (min-width: 768px) {
      @content;
    }
  }
  @else if $size == lg {
    @media screen and (min-width: 992px) {
      @content;
    }
  }
  @else if $size == xl {
    @media screen and (min-width: 1200px) {
      @content;
    }
  }
}
```

#### From now on media tags will always go inside a selector.

```scss

//How to use the media tags!

.some-class {
  width: 50%; 
  
  @include max(md) {
    width: 75%;
  }
  
  @include max(xs) {
    width: 100%; 
  }
}

COMPILES TO...

.some-class {
  width: 50%; 
}
@media screen and (max-width: 768px) {
  .some-class {
    width: 75%; 
  }
}
@media screen and (max-width: 330px) {
  .some-class {
    width: 100%; 
  }
}


/* Would like to move in a direction where we don't combine min and max for the same element. 
  Being that our focus is not mobile first using max makes more sense. */ 
```

### Cross browser compatibility mixin
```sass 

/* $exclude is an optional parameter, used for properties like "transition" in which -ms- is not valid */ 

@mixin browsers($property, $value, $exclude:"") {

  @if index($exclude, webkit) == null {
    -webkit-#{$property}: $value;
  }

  @if index($exclude, moz) == null {
    -moz-#{$property}: $value;
  }

  @if index($exclude, ms) == null {
    -ms-#{$property}: $value;
  }

  @if index($exclude, o) == null {
    -o-#{$property}: $value;
  }

  #{$property}: $value;
}
```
```sass
.spinner {
   @include browsers(animation-delay, -1.1s, ms);
}

.beep-boop {
   @include browsers(example, not-real, (webkit, moz));
}

COMPILES TO...

.spinner {
  -webkit-animation-delay: -1.1s;
  -moz-animation-delay: -1.1s;
  -o-animation-delay: -1.1s;
  animation-delay: -1.1s; 
}

.beep-boop {
  -ms-example: not-real;
  -o-example: not-real;
  example: not-real;
}

```
### Flex Center @extend
```sass
/* located in '/sass/modules/extends.scss'*/
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
```
```sass

.some-class {
  @extend .flex-center;
  color: $white75;
}

COMPILES TO...

.flex-center, .some-class {
  display: flex;
  align-items: center;
  justify-content: center;
}

.some-class {
  color: rgba(255,255,255,0.75);
}

/* WATCH OUT: @extend is a very powerful tool but:
  - Classes should be predetermined (Don't go extending all over, it gets wild)(Should be in extend.scss file)
  - Only on root selectors(no children).
```

### Colors

There is a colors file (/Content/sass/modules/colors.scss). There should be no new use of hex, rgb, or rgba in ANY of the partials. Only use predefined variables ($primary, $secondary... and so on) as defined in the colors file. Adding a color should require vetting. DO NOT add colors just because. Benefits of this approach: 
  * No color inconsistencies accross platform. 
  * Zero time spent memorizing color codes.
  * Easy color updates accross platform. 
  * Ease of tracking color use via global search (only one syntax per color). 

### Some No-no's 

##### DO NOT ADD OR USE any new styles to _media-tags-to-refactor.scss
This file will be deprecated eventually as media tags get moved to where they belong. Do not add, and do not use any selectors from this file. Make a class using the naming convention. 

##### DO NOT add any new styles to _bootstrap-overrides.scss OR _kendo-overrides.scss
These two files should be on their way out. These elements should be in component files. As the components get reworked and restyled, these files will thin out and cease to exist. 

##### DO NOT POLLUTE bootstrap classes with overrides. 
That creates a dependency where we cant use bootstrap classes for more than one purpose. 

One solution could be to create a prefix for our own library (Examples: "ol-btn ol-btn-primary"). These can be used alongside the bootstrap classes. 

