# usefull_scss
some usefull scss  
### Push auto  
`@mixin push--auto {  
    margin: {   
        left: auto;  
        right: auto;  
    }  
}`  
#### Usage:
`.some__block {  
  @include push--auto;  
}`  

### Pseudo
`@mixin pseudo($display: block, $pos: absolute, $content: ''){  
    content: $content;  
    display: $display;  
    position: $pos;  
}`
#### Usage:
`.some__block::after {  
  @include pseudo;  
  //other styles  
}`

### Responsive ratio
`@mixin responsive-ratio($x,$y, $pseudo: false) {  
    $padding: unquote( ( $y / $x ) * 100 + '%' );  
    @if $pseudo {  
        &:before {  
            @include pseudo($pos: relative);  
            width: 100%;  
            padding-top: $padding;  
        }  
    } @else {  
        padding-top: $padding;  
    }  
}`
#### Usage:
`.some__block {  
  @include responsive-ratio(16,9);  
  //other styles  
}`

### Triangles
`@mixin css-triangle($color, $direction, $size: 6px, $position: absolute, $round: false){  
    @include pseudo($pos: $position);  
    width: 0;  
    height: 0;  
    @if $round {  
        border-radius: 3px;  
    }  
    @if $direction == down {  
        border-left: $size solid transparent;  
        border-right: $size solid transparent;  
        border-top: $size solid $color;  
        margin-top: 0 - round( $size / 2.5 );  
    } @else if $direction == up {  
        border-left: $size solid transparent;  
        border-right: $size solid transparent;  
        border-bottom: $size solid $color;  
        margin-bottom: 0 - round( $size / 2.5 );  
    } @else if $direction == right {  
        border-top: $size solid transparent;  
        border-bottom: $size solid transparent;  
        border-left: $size solid $color;  
        margin-right: -$size;  
    } @else if  $direction == left {  
        border-top: $size solid transparent;  
        border-bottom: $size solid transparent;  
        border-right: $size solid $color;  
        margin-left: -$size;  
    }  
}`
#### Usage:  
`.some__block::after {  
  @include css-triangle(#0e0e0e, top, 12px);  
  //other styles  
}`  

### Font styles
`@mixin font-source-sans($size: false, $colour: false, $weight: false,  $lh: false) {  
    font-family: 'Source Sans Pro', Helvetica, Arial, sans-serif;  
    @if $size { font-size: $size; }  
    @if $colour { color: $colour; }  
    @if $weight { font-weight: $weight; }  
    @if $lh { line-height: $lh; }  
}`

### Placeholders
`@mixin input-placeholder {  
    &.placeholder { @content; }  
    &:-moz-placeholder { @content; }  
    &::-moz-placeholder { @content; }  
    &:-ms-input-placeholder { @content; }  
    &::-webkit-input-placeholder { @content; }  
}`
#### Usage:
`input, textarea {  
    @include input-placeholder {  
        color: $grey;  
    }  
}`

### Media queries
`$breakpoints: (  
    "phone":        400px,  
    "phone-wide":   480px,  
    "phablet":      560px,  
    "tablet-small": 640px,  
    "tablet":       768px,  
    "tablet-wide":  1024px,  
    "desktop":      1248px,  
    "desktop-wide": 1440px  
);  
@mixin mq($width, $type: min) {  
    @if map_has_key($breakpoints, $width) {  
        $width: map_get($breakpoints, $width);  
        @if $type == max {  
            $width: $width - 1px;  
        }  
        @media only screen and (#{$type}-width: $width) {  
            @content;  
        }  
    }  
}`
#### Usage:
`.site-header {  
    padding: 2rem;  
    font-size: 1.8rem;  
    @include mq('tablet-wide') {  
        padding-top: 4rem;  
        font-size: 2.4rem;  
    }  
}`

### Z-index - this one should be placed in global variables and then imported
`@function z($name) {  
    @if index($z-indexes, $name) {  
        @return (length($z-indexes) - index($z-indexes, $name)) + 1;  
    } @else {  
        @warn 'There is no item "#{$name}" in this list; choose one of: #{$z-indexes}';  
        @return null;  
    }  
}  
$z-indexes: (  
    "outdated-browser",  
    "modal",  
    "site-header",  
    "page-wrapper",  
    "site-footer"  
);  
`
#### Usage:
`.site-header {  
    z-index: z('site-header');  
}`

### Hardware support  
`@mixin hardware($backface: true, $perspective: 1000) {  
    @if $backface {  
        backface-visibility: hidden;  
    }  
    perspective: $perspective;  
}`

### Truncate - boundary should be width of the element
`@mixin truncate($truncation-boundary) {  
    max-width: $truncation-boundary;  
    white-space: nowrap;  
    overflow: hidden;  
    text-overflow: ellipsis;  
}`
tip:
you can just use display: block instead of max-width, and if it's inside flex element it works fine.
