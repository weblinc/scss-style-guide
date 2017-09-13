# Weblinc SCSS Style Guide


## Weblinc Best Practices

The following rules are recommended best practices for the Weblinc platform. Many come from Harry Robert's [CSS Guidelines](http://cssguidelin.es).


### [Multiple Files](http://cssguidelin.es/#multiple-files)

Each file should serve one and only one purpose. Improves maintainability.

__Bad__
```scss
// components/_foo.scss
.foo { ... }

.bar { ... }
```

__Good__
```scss
// components/_foo.scss
.foo { ... }
```

```scss
// components/_bar.scss
.bar { ... }
```


### [Indenting](http://cssguidelin.es/#indenting)

Intent BEM elements to showcase relationships. Improves readability.

__Bad__
```scss
.foo { ... }

.foo__bar { ... }

.foo_bar--baz { ... }
```

__Good__
```scss
.foo { ... }

    .foo__bar { ... }

    .foo__bar--baz { ... }
```


### [Meaningful Whitespace](http://cssguidelin.es/#meaningful-whitespace)

Improves readability.

- 1 newline to express tight relationships
- 2 newlines to express loose relationships
- no newline between comment blocks and their related rule set

__Bad__
```scss
$parent-width: 100px;
$parent-child-width: 800px;
$parent-child-heading-color: $black;

/**
 * The `.parent` UI blah blah blah
 */

.parent { ... }

    .parent__child { ... }

        .parent__child-heading { ... }

    .parent__cousin { ... }
```

__Good__
```scss
$parent-width: 100px;

$parent-child-width: 800px;
$parent-child-heading-color: $black;

/**
 * The `.parent` UI blah blah blah
 */
.parent { ... }


    .parent__child { ... }

        .parent__child-heading { ... }


    .parent__cousin { ... }
```


### [DocBlock Commenting](http://cssguidelin.es/#high-level)

Use DocBlock style comments. Improves readability.

__Bad__
```scss
.foo { ... } /* some comment */
.foo { ... } // some comment
```

__Good__
```scss
/**
 * Some Comment
 */
.foo { ... }
```


### [Reverse Footnotes in Comments](http://cssguidelin.es/#low-level)

Improves maintainability. Improves readability.

__Bad__
```scss
.foo {
    position: relative;
    z-index: 30;
}

    .foo__bar {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 20;
    }
```

__Good__
```scss
/**
 * 1. provides positioning context for `.foo__bar`
 * 2. above `.foo__bar`
 */
.foo {
    position: relative; /* [1] */
    z-index: 30; /* [2] */
}

    /**
     * 1. below `.foo`
     */
    .foo__bar {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 20; /* [1] */
    }
```


### Use proactive `!default`

Improves maintainability.

__Bad__
```scss
$foo: bar;
```

__Good__
```scss
$foo: bar !default;
```


### Encapsulate contextual styles

Improves maintainability. Improves readability.

__Bad__
```scss
// components/_foo.scss
.foo {
    margin: 10px 0;

    .bar {
        background: $blue;
    }
}
```

```scss
// components/_bar.scss
.bar {
    background: $red;
}
```

__Good__
```scss
// components/_foo.scss
.foo {
    margin: 10px 0;
}
```

```scss
// components/_bar.scss
.bar {
    background: $red;

    .foo & {
        background: $blue;
    }
}
```


### Group related selectors on the same line

Improves readability.

__Bad__
```scss
dl,
dd,
ol,
ul,
th,
td { ... }
```

__Good__
```scss
dl, dd, ol, ul,
table, th, td { ... }
```



## SCSS Lint

The following rules are enforcible via [scss-lint](https://github.com/brigade/scss-lint). If you're linting your files properly any offenses should be caught automatically.


### Add only one space before flag ([BangFormat](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#bangformat))

Improves readability.

__Bad__
```scss
$color: #000!default;

.foo {
    @extend %placeholder ! optional;
}
```

__Good__
```scss
$color: #000 !default;

.foo {
    @extend %placeholder !optional;
}
```


### Each BEM element has one and only one parent ([BemDepth](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#bemdepth))

Reduces confusion.

__Bad__
```scss
.block__element__sub-element {}
```

__Good__
```scss
.block__element {}
```


### Remove borders with a zero ([BorderZero](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#borderzero))

Reduces file size.

__Bad__
```scss
border: none;
```

__Good__
```scss
border: 0;
```


### Don't chain classes ([ChainedClasses](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#chainedclasses))

Reduces specificity. Promotes reusable selectors.

__Bad__
```scss
.foo {
    padding: 5px;
}

.bar {
    margin: 5px;
}

.foo.bar {
    display: block;
}
```

__Good__
```scss
.foo {
    padding: 5px;
}

.bar {
    margin: 5px;
}

.new-class {
    display: block;
}
```


### Never use color keywords ([ColorKeyword](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#colorkeyword))

Improves maintainability.

__Bad__
```scss
$color: red;
```

__Good__
```scss
$color: #ff0000;
```


### Always define colors in variables ([ColorVariable](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#colorvariable))

Improves maintainability.

__Bad__
```scss
.foo {
    background-color: #ff0000;
}
```

__Good__
```scss
$foo-bg-color: #ff0000;

...

.foo {
    background-color: $foo-bg-color;
}
```


### Prefer traditional CSS comments ([Comment](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#comment))

Increases readability. These are removed during compilation.

__Bad__
```scss
.foo {
    z-index: 30; // above `.bar`, below `.baz`
}
```

__Good__
```scss
/**
 * 1. above `.bar`, below `.baz`
 */
.foo {
    z-index: 30; /* [1] */
}
```


### Don't commit `@debug` statements ([DebugStatement](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#debugstatement))

Keep your `@debug` statements out of the code base.


### Maintain a sane declaration order ([DeclarationOrder](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#declarationorder))

Improves readability.

Rule sets should be declared in the following order:

1. `@extend`
2. `@include` without inner `@content`
3. properties
4. `@include` with inner `@content`

__Bad__
```scss
.foo {
    background-color: $bg-color;

    @include icon {
        color: $icon-color;
    }

    @extend %heading;
    @include clearfix;
}
```

__Good__
```scss
.foo {
    @extend %heading;
    @include clearfix;
    background-color: $bg-color;

    @include icon {
        color: $icon-color;
    }

}
```


### Remove duplicate properties ([DuplicateProperty](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#duplicateproperty))

Improves maintainability. Reduces errors.

__Bad__
```scss
.foo {
    margin: 10px 0;
    padding: 10px;
    margin: 0;
}
```

__Good__
```scss
.foo {
    margin: 10px 0;
    padding: 10px;
}
```


### Don't start `@else` on a new line ([ElsePlacement](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#elseplacement))

Improves readability.

__Bad__
```scss
@if ($toggle) {
    /* do something */
}
@else {
    /* do something else */
}
```

__Good__
```scss
@if ($toggle) {
    /* do something */
} @else {
    /* do something else */
}
```


### Add newline between rule sets ([EmptyLineBetweenBlocks](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#emptylinebetweenblocks))

Improves readability.

__Bad__
```scss
.foo {
    margin: 0;
    .baz & {
        margin: 10px;
    }
}
.bar {
    padding: 10;
}
```

__Good__
```scss
.foo {
    margin: 0;

    .baz & {
        margin: 10px;
    }
}

.bar {
    padding: 10;
}
```


### Add newline to end of file ([FinalNewline](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#finalnewline))

Improves diff.

__Bad__
```scss
.foo {
    padding: 10px;
}
```

__Good__
```scss
.foo {
    padding: 10px;
}

```


### Reduce length of hexadecimals where possible ([HexLength](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#hexlength))

Reduces file size.

__Bad__
```scss
$red: #ff0000;
```

__Good__
```scss
$red: #f00;
```


### Use lowercase for hexadecimals ([HexNotation](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#hexnotation))

Reduces file size.

__Bad__
```scss
$red: #F00;
```

__Good__
```scss
$red: #f00;
```


### Hexadecimal values should be 3 or 6 characters ([HexValidation](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#hexvalidation))

Reduces errors.

__Bad__
```scss
$red: #f0;
$blue: #0000f92;
```

__Good__
```scss
$red: #f00;
$blue: #0000f9;
```


### Don't use IDs as selectors ([IdSelector](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#idselector))

Reduces specificity.

__Bad__
```scss
#foo {
    padding: 10px;
}
```

__Good__
```scss
.foo {
    padding: 10px;
}
```


### Don't use `!important` ([ImportantRule](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#importantrule))

Reduces specificity.

__Bad__
```scss
.foo {
    padding: 10px !important;
}
```

__Good__
```scss
.foo {
    padding: 10px;
}
```


### Import files properly ([ImportPath](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#importpath))

Improves best practices.

__Bad__
```scss
@import "foo/_bar.scss";
@import "_bar.scss";
@import "_bar";
@import "bar.scss";
```

__Good__
```scss
@import "foo/bar";
@import "bar";
```


### Four spaces for indentation ([Indentation](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#indentation))

Improves readability.

__Bad__
```scss
.foo {
  padding: 10px;
}

  .foo__bar {
    margin: 10px 0;
  }
```

__Good__
```scss
.foo {
    padding: 10px;
}

    .foo__bar {
        margin: 10px 0;
    }
```


### Add leading zero to decimals ([LeadingZero](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#leadingzero))

Improves readability.

__Bad__
```scss
.foo {
    margin: .5em;
}
```

__Good__
```scss
.foo {
    margin: 0.5em;
}
```


### Don't duplicate selectors ([MergeableSelector](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#mergeableselector))

Improves maintainability.

__Bad__
```scss
.foo {
    padding: 10px;
}

.bar {
    margin: 10px 0;
}

.bar.ui-menu {
    background: $gray;
}

.foo {
    text-decoration: underline;
}
```

__Good__
```scss
.foo {
    padding: 10px;
    text-decoration: underline;
}

.bar {
    margin: 10px 0;

    &.ui-menu {
        background: $gray;
    }
}
```


### Use kebab-case for naming ([NameFormat](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#nameformat))

Improves readability.

__Bad__
```scss
@function fooBar { ... }
@mixin BarBaz { ... }
%baz_qux { ... }
$quux_corge: ...;
```

__Good__
```scss
@function foo-bar { ... }
@mixin bar-baz { ... }
%baz-qux { ... }
$quux-corge: ...;
```


### Don't nest selectors past a depth of 3 ([NestingDepth](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#nestingdepth))

Reduces specificity.

__Bad__
```scss
.foo {
    .foo__bar {
        .foo__baz {
            &:hover {
                ...
            }
        }
    }
}
```

__Good__
```scss
.foo { ... }

    .foo__bar { ... }

        .foo__baz {
            &:hover {
                ...
            }
        }
```


### Don't nest selectors within a media query

Improves maintainability.

__Bad__
```scss
.component { ... }

.component--modifier { ... }

    .component__element {
        @include respond-to($medium-breakpoint) {
            color: $element-color;

            .component--modifier & {
                color: $modified-element-color;
            }
        }
    }
```

__Good__
```scss
.component { ... }

.component--modifier { ... }

    .component__element {
        @include respond-to($medium-breakpoint) {
            color: $element-color;
        }

        .component--modifier & {
            @include respond-to($medium-breakpoint) {
                color: $modified-element-color;
            }
        }
    }
```


### Only extend placeholder selectors ([PlaceholderInExtend](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#placeholderinextend))

Reduces file size.

__Bad__
```scss
.foo {
    @extend .bar;
}
```

__Good__
```scss
.foo {
    @extend %bar;
}
```


### Maintain a sane property sort order ([PropertySortOrder](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#propertysortorder))

Improves maintainability. Properties within this list should be ordered appropriately. Properties not within this list should come below ordered properties.

```
display

position
top
right
bottom
left
z-index

margin
margin-top
margin-right
margin-bottom
margin-left

margin-collapse
margin-top-collapse
margin-right-collapse
margin-bottom-collapse
margin-left-collapse

padding
padding-top
padding-right
padding-bottom
padding-left

width
height
max-width
max-height
min-width
min-height

float
clear

color

font
font-style
font-family
font-weight
font-variant
font-smoothing

line-height
letter-spacing
word-spacing

text-align
text-indent
text-shadow
text-overflow
text-rendering
text-transform
text-decoration
text-size-adjust

word-break
word-wrap

white-space

background
background-size
background-color
background-image
background-repeat
background-position
background-attachment

border
border-top
border-right
border-bottom
border-left

border-image
border-spacing
border-collapse

border-color
border-top-color
border-right-color
border-bottom-color
border-left-color

border-style
border-top-style
border-right-style
border-bottom-style
border-left-style

border-width
border-top-width
border-right-width
border-bottom-width
border-left-width

border-radius
border-top-right-radius
border-bottom-right-radius
border-bottom-left-radius
border-top-left-radius
border-radius-topright
border-radius-bottomright
border-radius-bottomleft
border-radius-topleft

box-shadow
```


### Spell properties correctly ([PropertySpelling](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#placeholderinextend))

Reduces errors.

__Bad__
```scss
.foo {
    dipslay: none;
}
```

__Good__
```scss
.foo {
    display: none;
}
```


### Control property value units ([PropertyUnits](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#propertyunits))

Supports best practices.

- `line-height` should be unitless

__Bad__
```scss
.foo {
    font-size: 16px;
    line-height: 20px;
}
```

__Good__
```scss
.foo {
    font-size: 16px;
    line-height: 1.25;
}
```


### Use two colons to prefix pseudo-elements ([PseudoElement](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#pseudoelement))

Improves readability. Supports best practices.

__Bad__
```scss
.foo {
    &:before { ... }
    &::hover { ... }
}
```

__Good__
```scss
.foo {
    &::before { ... }
    &:hover { ... }
}
```


### Don't prefix selectors with elements ([QualifyingElement](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#qualifyingelement))

Reduces specificity.

__Bad__
```scss
div.foo { ... }

input[type=text] { ... }

div#foo { ... }
```

__Good__
```scss
.foo { ... }

[type=text] { ... }

#foo { ... }
```


### Don't write selectors past a depth of 3 ([SelectorDepth](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#selectordepth))

Reduces specificity.

__Bad__
```scss
.foo .bar .baz .qux { ... }

.foo {
    .bar .baz .qux { ... }
}
```

__Good__
```scss
.foo .bar .baz { ... }

.foo {
    .bar .baz & { ... }
}
```


### Use BEM-like syntax ([SelectorFormat](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#selectorformat))

Improves maintainability. Improves readability. More info [here](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/).

__Bad__
```scss
.foo {
    background: $white;

    &.active {
        background: $yellow;
    }
}

    .foo-child {
        padding: 10px;
    }
```

__Good__
```scss
.foo {
    background: $white;
}

.foo--active {
    background: $yellow;
}

    .foo__child {
        padding: 10px;
    }
```


### Use shorthand properties wherever possible ([Shorthand](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#shorthand))

Reduces file size.

__Bad__
```scss
.foo {
    margin: 10px 0 10px 0;
}
```

__Good__
```scss
.foo {
    margin: 10px 0;
}
```


### Each property should appear on it's own line ([SingleLinePerProperty](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#singlelineperproperty))

Improves readability.

__Bad__
```scss
.foo {
    margin: 10px 0; padding: 10px;
}
```

__Good__
```scss
.foo {
    margin: 10px 0;
    padding: 10px;
}
```


### Add one space after comma ([SpaceAfterComma](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceaftercomma))

Improves readability.

__Bad__
```scss
.foo {
    color: rgba(0,0,0,.5);
}
```

__Good__
```scss
.foo {
    color: rgba(0, 0, 0, .5);
}
```


### Add one space after comment ([SpaceAfterComment](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceaftercomment))

Improves readability.

__Bad__
```scss
/**
 *Foo
 */
```

__Good__
```scss
/**
 * Foo
 */
```


### Add one space after property colon ([SpaceAfterPropertyColon](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceafterpropertycolon))

Improves readability.

__Bad__
```scss
.foo {
    padding:10px;
}

.foo {
    padding:    10px;
}
```

__Good__
```scss
.foo {
    padding: 10px;
}
```


### Don't add spaces after property name ([SpaceAfterPropertyName](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceafterpropertyname))

Improves readability.

__Bad__
```scss
.foo {
    padding : 10px;
}
```

__Good__
```scss
.foo {
    padding: 10px;
}
```


### Add one space after variable colon ([SpaceAfterVariableColon](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceaftervariablecolon))

Improves readability.

__Bad__
```scss
$foo:bar;
$foobar:baz;
```

__Good__
```scss
$foo:     bar;
$foobar:  baz;
```


### Don't add spaces after variable name ([SpaceAfterVariableName](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spaceaftervariablename))

Improves readability.

__Bad__
```scss
$foo : bar;
```

__Good__
```scss
$foo: bar;
```


### Add a space around operators ([SpaceAroundOperator](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spacearoundoperator))

Improves readability.

__Bad__
```scss
.foo {
    width: $bar-width+$bar-width;
}
```

__Good__
```scss
.foo {
    width: $bar-width + $bar-width;
}
```


### Add one space before curly brace ([SpaceBeforeBrace](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spacebeforebrace))

Improves readability.

__Bad__
```scss
.foo{
    padding: 10px;
}

.bar
{
    padding: 10px;
}

.bar--baz{ padding: 10px 0 0; }
.bar--corge{ padding:  0 0 10px; }
```

__Good__
```scss
.foo {
    padding: 10px;
}

.bar {
    padding: 10px;
}

.bar--baz   { padding: 10px 0 0; }
.bar--corge { padding: 0 0 10px; }
```


### Don't add spaces between parenthesis ([SpaceBetweenParens](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#spacebetweenparens))

Improves readability.

__Bad__
```scss
.foo {
color: rgba( 0, 0, 0, .5 );
}
```

__Good__
```scss
.foo {
    color: rgba(0, 0, 0, .5);
}
```


### Use single quotes for string literals ([StringQuotes](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#stringquotes))

Reduces file size. Double quotes can be used when avoiding escape characters.

__Bad__
```scss
.foo {
    content: "bar";
}
```

__Good__
```scss
.foo {
    content: 'bar';
}

.foo {
    content: "bar 'baz' qux";
}
```


### End statements with a semicolon ([TrailingSemicolon](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#trailingsemicolon))

Supports best practices. Reduces errors.

__Bad__
```scss
$foo: bar

.foo {
    margin: 10px 0;
    padding: 10px
}
```

__Good__
```scss
$foo: bar;

.foo {
    margin: 10px 0;
    padding: 10px;
}
```


### Remove whitespace from line endings ([TrailingWhitespace](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#trailingwhitespace))

Reduces file size. Improves diffs.


### Don't add unnecessary trailing zeros to measurements ([TrailingZero](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#trailingzero))

Reduces file size.

__Bad__
```scss
.foo {
    font-size: .500em;
}
```

__Good__
```scss
.foo {
    font-size: .5em;
}
```


### Explicitly specify transitions ([TransitionAll](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#transitionall))

Improves maintainability. Reduces errors.

__Bad__
```scss
.foo {
    transition: all .5s ease-in;
}
```

__Good__
```scss
.foo {
    transition: color .5s ease-in,
                margin-bottom .5s ease-in;
}
```


### Measurements should not contain unnecessary decimals ([UnnecessaryMantissa](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#unnecessarymantissa))

Reduces file size.

__Bad__
```scss
.foo {
    margin: 1.0em;
}
```

__Good__
```scss
.foo {
    margin: 1em;
}
```


### Don't use unnecessary parent references ([UnnecessaryParentReference](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#unnecessaryparentreference))

Reduces file size.

__Bad__
```scss
.foo {
    & > .bar {
       padding: 10px;
    }
}
```

__Good__
```scss
.foo {
    > .bar {
        padding: 10px;
    }
}
```


### Use relative paths for assets ([UrlFormat](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#urlformat))

Reduces errors.

__Bad__
```scss
.foo {
    background-image: url('https://foo.com/assets/image.jpg');
}
```

__Good__
```scss
.foo {
    background-image: url('assets/image.jpg');
}
```


### Wrap URLs in single quotes ([UrlQuotes](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#urlquotes))

Improves readability.

__Bad__
```scss
.foo {
    background-image: url(assets/image.jpg);
}
```

__Good__
```scss
.foo {
    background-image: url('assets/image.jpg');
}
```


### Prefer functional variables ([VariableForProperty](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#variableforproperty))

Improves readability. Improves maintainability. Enforced for:

- `color`
- `font`
- `font-size`
- `font-family`
- `background`
- `background-color`
- `border`
- `border-top`
- `border-right`
- `border-bottom`
- `border-left`
- `border-radius`
- `box-shadow`

__Bad__
```scss
.foo {
    color: #f00;
    font-family: sans-serif;
}
```

__Good__
```scss
$red: #f00;
$font-family: 'sans-serif';

.foo {
    color: $red;
    font-family: $font-family;
}
```


### Don't use vendor prefixes ([VendorPrefix](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#vendorprefix))

Improves developer efficiency. Automatically added by [autoprefixer](https://github.com/ai/autoprefixer-rails).

__Bad__
```scss
.foo {
    -webkit-transition: none;
    transition: none;
}
```

__Good__
```scss
.foo {
    transition: none;
}
```


### Don't add a unit for zero values ([ZeroUnit](https://github.com/brigade/scss-lint/blob/master/lib/scss_lint/linter/README.md#zerounit))

Reduces file size.

__Bad__
```scss
.foo {
    margin: 0px;
}
```

__Good__
```scss
.foo {
    margin: 0;
}
```
