# Grid Generator


**A Lightweight grid generator** using SASS and Pug, its application is similar to bootstap by adding the markup to define the usage of the grid


### Custom settings

Set the variable `$gridwidth` to the desired max width set `$margin-*` to indicate the margins left and right of the main grid beyond the margin and the `$gutter-*` for the whle grid change you target viewport by modifiying `-md, -lg or -sm` set the number of `$columns` and you are set

There are a number of classes to alter the centering, reverse, center and full with no gutters or margin for mobile

___

## Grid.scss
###
___

```scss
// Mixins
@mixin mobile    { @media (min-width: 480px)  { @content } }
@mixin tablet    { @media (min-width: 768px)  { @content } }
@mixin tabletLrg { @media (min-width: 992px)  { @content } }
@mixin desktop   { @media (min-width: 1200px) { @content } }

// Max width
$gridwidth: 1000px;

// Margins desktop, tablet
$margin-md: 10px;
$margin-sm: 15px;

// Gutters desktop, tablet, mobile
$gutter-md: 20px;
$gutter-sm: 15px;
$gutter-xs: 10px;

// Number of columns
$columns: 12;

.container {
  max-width: $gridwidth;
  margin: 0 auto;
}

// Grid Mixins 
  
// Calculates padding for row and gutter for columns
@mixin calcPadding($padding) {
  padding-right: $padding;
  padding-left:  $padding;
}

// Creates 12 columns and offsets and calculates its width
@mixin createColumns($breakpoint) {
  @for $i from 1 through $columns {
    .col-#{$breakpoint}-#{$i} { 
      flex: ((100 / $columns) * $i) * 1%;
      max-width: ((100 / $columns) * $i) * 1%;
    }
    .offset-#{$breakpoint}-#{$i} { margin-left: ((100 / $columns) * $i) * 1% }
  }
}

// Flexbox grid
.row {
  display: flex;
  flex-flow: row wrap;
  transition: all 0.3s ease;
  @include calcPadding($margin-sm)
  &.reverse { flex-flow: row-reverse wrap-reverse }
  &.center { justify-content: center }
}

[class*='col'] {
  width: 100%;
  margin: 1em auto;
  @include calcPadding($gutter-xs)
  &.reverse { flex-flow: column-reverse wrap-reverse }
}

@include createColumns(xs)

// Media queries
@include mobile {
  .row.full { 
    @include calcPadding(0);
    [class*='col'] { @include calcPadding(0) }
  } 
}

@include tablet {
  .row { @include calcPadding($margin-sm) }
  @include createColumns(sm)
}

@include tabletLrg {
  .row { @include calcPadding($margin-md) }
  @include createColumns(md)
}

@include desktop {
  .row { @include calcPadding($margin-md) }
  [class*="col"] { @include calcPadding($gutter-md) }
  @include createColumns(lg)
}

// Utilities
.align-center { text-align: center }
.align-left   { text-align: left }
.align-right  { text-align: right }
```

Pug

```pug
.row
  .col-xs-12.col-sm-10.offset-sm-1.col-md-8.offset-md-2
    p Using #[code grids.scss] in a mobile first fashion to compute the number of columns based on html classes starting with #[code .col-] followed by the break point #[code -xs] for mobile #[code -sm] for tablet and #[code -md] for desktop and finally the ammount of columns that required #[code -12] for a full set, #[code -6] for half and so on, and the option to add offset by using the class e.j: #[code .offset-md-2]
```