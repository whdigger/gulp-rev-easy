# gulp-rev-easy 
# Install
```
$ npm install gulp-rev-easy --save-dev
```
# Examples
```
assets\index.css
assets\index.js
test.html
gulpfile.js
```
### Input
```html
//test.html
<!doctype html>
<html lang="en">
<head>
  <title>gulp-rev-easy</title>
  <link type="text/css" rel="stylesheet" media="all" 
    href="assets/index.css?max_age=1024">
</head>
<body>
  <img data-src="assets/audrey-hepburn.jpg" 
    src="assets/audrey-hepburn.jpg">
  <script src="assets/index.js?max_age=1024"></script>
</body>
</html>
```

### Useage
```js
//gulpfile.js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy())
    .pipe(gulp.dest("./dist"))
})
```

```
gulp reveasy
```
### Output
```html
<!doctype html>
<html lang="en">
<head>
  <title>gulp-rev-easy</title>
  <link type="text/css" rel="stylesheet" media="all"
    href="assets/index.css?max_age=1024&amp;v=0a1085be">
</head>
<body>
  <img data-src="assets/audrey-hepburn.jpg" 
    src="assets/audrey-hepburn.jpg?v=7c5d110d">
  <script src="assets/index.js?max_age=1024&amp;v=3fffb693"></script>
</body>
</html>
```

# Options

 - cwd
 - revType
 - dateFormat
 - hashLength
 - suffix
 - fileTypes
 - elementAttributes
 - transformPath

## options.cwd


    type:string
    default:''
    
    set working directory for your assets

#### example
```js
/*
test\assets\index.css
test\assets\index.css
test\test.html
gulpfile.js
*/

var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({cwd:'./test/'}))
    .pipe(gulp.dest("./dist"))
})
```

## options.revType


    type:['hash'|date']
    default:'hash'
    
    set rev type 

#### example
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({revType:'date'}))
    .pipe(gulp.dest("./dist"))
    
})
```

```html
<link type="text/css" rel="stylesheet" media="all" 
  href="assets/index.css?max_age=1024">
```
```html
<link type="text/css" rel="stylesheet" media="all" 
  href="assets/index.css?max_age=1024&amp;v=201503061144">
```

## options.dateFormat

    type:string
    default:'yyyymmddHHMM'
    
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({revType:'date', dateFormat:'yymmddHHmm'}))
    .pipe(gulp.dest("./dist"))
    
})
```
#### Output
```html
<link type="text/css" rel="stylesheet" media="all" 
  href="assets/index.css?max_age=1024&amp;v=1503061144">
```

## options.hashLength

    type:integer
    default:8
    
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({revType:'hash', hashLength:5}))
    .pipe(gulp.dest("./dist"))
    
})
```
#### Output

```html
<!--default output-->
<img data-src="assets/audrey-hepburn.jpg" 
  src="assets/audrey-hepburn.jpg?v=7c5d110d">
```
-->
```html
<img data-src="assets/audrey-hepburn.jpg" 
  src="assets/audrey-hepburn.jpg?v=7c5d1">
```

## options.suffix

    type:string
    default:v
    
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({suffix:'hashkey'}))
    .pipe(gulp.dest("./dist"))
    
})
```
#### Output

``` html
<!--default output-->
<img src="assets/audrey-hepburn.jpg?v=7c5d110d">
```
-->
```html
<img src="assets/audrey-hepburn.jpg?hashkey=7c5d110d">
```

## options.fileTypes

    type:array
    default:['js','css','img']
    
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({fileTypes:['js']}))
    .pipe(gulp.dest("./dist"))
    
})
```
#### Output

```html
<!doctype html>
<html lang="en">
<head>
  <title>gulp-rev-easy</title>
  <link type="text/css" rel="stylesheet" media="all" 
    href="assets/index.css?max_age=1024">
</head>
<body>
  <img data-src="assets/audrey-hepburn.jpg"
    src="assets/audrey-hepburn.jpg">
  <script src="assets/index.js?max_age=1024&amp;v=3fffb693"></script>
</body>
</html>
```

## options.elementAttributes
```js

    /*
    how find target element 
     js: {
        name: 'script',
        src: 'src'
      }
      =>
      $('script').attr('src') = newpath
     use other selector in name such as
     {
        name:'.cached',
        src:'src'
      }
    */
    type:object
    default:{
      js: {
        name: 'script',
        src: 'src'
      },
      css: {
        name: 'link[type="text/css"]',
        src: 'href'
      },
      img:{
        name: 'img',
        src : 'src'
      }
    }
```   

```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({
        fileTypes:['img1'],
        elementAttributes:{
            img1:{
                name:'img',
                src:'data-src'
            }
        }))
    .pipe(gulp.dest("./dist"))
})
```
#### Output
```html
  <!--default out-->
  <img data-src="assets/audrey-hepburn.jpg" 
    src="assets/audrey-hepburn.jpg?v=7c5d110d">
```
-->
```html
  <img data-src="assets/audrey-hepburn.jpg?v=7c5d110d" 
    src="assets/audrey-hepburn.jpg">
```

```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({
        fileTypes:['img1'],
        elementAttributes:{
            img1:{
                name:'img',
                src:'data-src'
            }
        }))
    .pipe(gulp.dest("./dist"))
})
```
## options.transformPath
    type:function
    default:function(orgPath, ver){}
custom change path
```js
var gulp = require("gulp");
var reveasy = require("gulp-rev-easy");

gulp.task("reveasy", function (argument) {
  gulp.src("test.html")
    .pipe(reveasy({
        transformPath:function(orgpath, ver){
          var newpath = "http://s1.cdn.com/" + orgpath 
                          + (orgpath.indexOf('?') > -1 ? "&" : "?")  
                          + "v=" + ver;
          return newpath;
        }
    }))
    .pipe(gulp.dest("./dist"))
})
```
#### Output
```html
  <!--default out-->
  <img data-src="assets/audrey-hepburn.jpg" 
    src="assets/audrey-hepburn.jpg?v=7c5d110d">
```
-->
```html
<img data-src="assets/audrey-hepburn.jpg"
    src="http://s1.cdn.com/assets/audrey-hepburn.jpg?v=7c5d110d">
```
# Other
