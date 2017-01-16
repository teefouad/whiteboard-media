# whiteboard-media

A node-sass importer for a set of SASS mixins that make your life a little bit easier when it comes to developing responsive styles. It allows creating breakpoints and defining styles for each breakpoint in a simple way.

<br>
<br>

## Installation

```
npm install --save-dev whiteboard-media
```

<br>
<br>

## Usage

You can use this importer in node-sass or any project that depends on node-sass. The only thing you need to do to make this work is add the importer to the options and include the required variables or mixins.

### node-sass

```javascript
var sass = require('node-sass');
var wbMediaImporter = require('whiteboard-media');

sass.render({
  file: './scss/styles.scss',
  importer: wbMediaImporter
});
```

### gulp-sass

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');
var wbMediaImporter = require('whiteboard-media');

gulp.task('sass', function() {
  return gulp.src('scss/**/*.scss')
    .pipe(sass({
      importer: wbMediaImporter
    })
    .pipe(gulp.dest('./css'));
});
```

### grunt-sass

```javascript
module.exports = function(grunt) {
  var wbMediaImporter = require('whiteboard-media');

  grunt.initConfig({
    sass:{
      options: {
        importer: wbMediaImporter
      },
      ...        
    }
  });
}
```

Once the importer is setup, you can import the variables and mixins to start using them in your code.

```sass
@import 'wb/media'
```

<br>
<br>

## Breakpoints

Responsive breakpoints are stored in the SASS variable `$wb-breakpoints`. By default, the following breakpoints are defined:

```sass
$wb-breakpoints: (
  xs: null,
  sm: 544px,
  md: 768px,
  lg: 992px,
  xl: 1200px
)
```

You can add a new breakpoint using `wb-add-breakpoint($breakpoint-name, $breakpoint-value)`, for example:

```sass
$wb-breakpoints: wb-add-breakpoint(mobile, 640px)

// $wb-breakpoints: (
//  xs: null,
//  sm: 544px,
//  md: 768px,
//  lg: 992px,
//  xl: 1200px,
//  mobile: 640px
// )
```

In the same manner, you can remove a breakpoint using `wb-remove-breakpoint($breakpoint-name)`, for example:

```sass
$wb-breakpoints: wb-remove-breakpoint(xl)

// $wb-breakpoints: (
//  xs: null,
//  sm: 544px,
//  md: 768px,
//  lg: 992px
// )
```

<br>
<br>

## Mixins

A set of useful mixins are provided to help you with developing your responsive styles.

### wb-media($min-value, $max-value)

Prints a media query based on given values for minimum and maximum widths.

```sass
@include wb-media(320px, 640px) {
  body {
    background: black
  }
}

/*
prints the following:

@media screen and (min-width: 320px) and (max-width: 639px) {
  body {
    background: black
  }
}
 */
```

To exclude one of the constricts, set its value to `null`.

```sass
@include wb-media(320px, null) {
  body {
    background: black
  }
}

/*
Prints the following:

@media screen and (min-width: 320px) {
  body {
    background: black
  }
}
 */
```

<br>
---
<br>

### wb-media-only($breakpoint)

Prints the provided styles specifically and only for the given breakpoint.

```sass
@include wb-media-only(md) {
  body {
    background: black
  }
}

/*
Prints the following:

@media screen and (min-width: 768px) and (max-width: 991px) {
  body {
    background: black
  }
}
 */
```

<br>
---
<br>

### wb-media-and-larger($breakpoint)

Prints the provided styles for the given breakpoint and larger breakpoints.

```sass
@include wb-media-and-larger(md) {
  body {
    background: black
  }
}

/*
Prints the following:

@media screen and (min-width: 768px) {
  body {
    background: black
  }
}
 */
```

<br>
---
<br>

### wb-media-and-smaller($breakpoint)

Prints the provided styles for the given breakpoint and smaller breakpoints.

```sass
@include wb-media-and-smaller(md) {
  body {
    background: black
  }
}

/*
Prints the following:

@media screen and (max-width: 991px) {
  body {
    background: black
  }
}
 */
```

<br>
---
<br>

### wb-media-through($breakpoint-a, $breakpoint-b)

Prints the provided styles through the given breakpoints.

```sass
@include wb-media-through(md, lg) {
  body {
    background: black
  }
}

/*
Prints the following:

@media screen and (min-width: 768px) and (max-width: 1199px) {
  body {
    background: black
  }
}
 */
```

<br>
---
<br>

### wb-make-responsive()

Iterates over the breakpoints stack ($wb-breakpoints) and sets 3 global variables which may be used when writing responsive styles.

The three global variables are:

_$wb-breakpoint:_   
The breakpoint name.

_$wb-breakpoint-prefix:_   
The breakpoint name followed by a dash.

_$wb-breakpoint-suffix:_   
The breakpoint name preceded by a dash.

```sass
@include wb-make-responsive {
  .text-center#{$wb-breakpoint-suffix} {
    text-align: center
  }
}

/*
Prints the following:

.text-center-xs {
  text-align: center
}

@media screen and (min-width: 544px) {
  .text-center-sm {
    text-align: center
  }
}

@media screen and (min-width: 768px) {
  .text-center-md {
    text-align: center
  }
}

@media screen and (min-width: 992px) {
  .text-center-lg {
    text-align: center
  }
}

@media screen and (min-width: 1200px) {
  .text-center-xl {
    text-align: center
  }
}
 */
```
