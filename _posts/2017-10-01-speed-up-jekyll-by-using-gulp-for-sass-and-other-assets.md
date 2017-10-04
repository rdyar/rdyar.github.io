---
---

I spent the last couple weeks learning/using Hugo for some reason. Hugo is really really fast at generating a site, but in the end I decided to keep using Jekyll, mostly cause I understand it better, and my sites are not that big that it is a problem.

But it is hard to go back to a slower workflow so I decided to see if I could make some changes to how I use Jekyll.

One of the things about hugo is that it doesn't currently have a sass processor, so if you want to use sass you have to use something like gulp to process the sass. I had a bit of an ah-ha moment and decided to see if I could move all of my asset stuff into a gulp task and see if that made a difference with how fast the site generates, and most importantly how fast it live reloads.

WOW! move sass into a gulp workflow, and tell jekyll to ignore where the final css file ends up and you can edit css and save it, and the change is reloaded instantly more or less - so jekyll is not regenerating, gulp is processing the sass into a css file and putting it in the site folder - all blazingly fast, with source maps. I am happy with my current css so it won't do much for me right now, but when I was coming up with my sites' css I was constantly waiting for changes and now it will be almost instant, and I can edit in Chrome's dev tools and save directly to whichever partial is needed. I had never used sourcemaps before, so this is pretty neat to me.

The other cool thing about using gulp for sass processing is that now Jekyll doesn't have to do it - and that makes it quicker. With Jekyll 3.5.2's speed improvement - before my gulp sass workflow -  my build time went down from around 15 seconds to about 6. Now with gulp doing the sass I was down to about 2.5 seconds. Some of this time saved was really from not using a plugin called `octopress-autoprefixer`. Just removing that from jekyll sped up the build by a couple seconds, but then I lost autoprefixer. With gulp processing the sass I get autoprefixer, source maps and cssnano all at once. 

Why stop with just sass? one of the things about Hugo was that it really made me think about what Jekyll needed to do vs what it didn't need to do. If there is no front matter, why have jekyll constantly nuking the site folder and redoing everything? Everything that was in my `assets` folder was just static files that could be moved on their own, no need to have jekyll wipe out all the content just to move over a js file or an image.

So in addition to gulp doing its sass thing, I also have it monitor the assets/js folder and move it - and uglify it as well. I don't use much js so this part could be improved with source maps also, but I don't have any use for that so I just have gulp minify it and move it to the `site/assets/js` folder.

I do the same for images, basically just moving them. I have a separate task to minify them, I run that on the source folder from time to time, no need to do it all the time. I am pretty decent at PS so my image files are usually pretty small to begin with.

So now for all my assets - sass, js and images, I have gulp watch them and when needed move them to the corresponding site/assets folder, and then it triggers BrowserSync to reload the browser as well. If there is a file with front matter, then jekyll rebuilds, and nukes everything in the site folder except the assets folder.

Now with all my static assets handled by gulp, my build time is just over half of a second. That is down from 6 seconds before using gulp for asset processing, and down from about 15 seconds before 3.5.2. And any sass changes - or js - or images - show up almost instantly.

One small problem with this workflow is that if I delete a file out of the source assets folder, it is not deleted from the site/assets folder.  I don't think that is a big deal, I will delete site/assets once in a while and it should be fine. If there is an extra image or three in there that is not used I don't think it will matter.

This is still new to me, it seems to work really well but there could still be issues with it. Feel free to contact me if you find something wrong.

Here is the Gulp file:
**Please note - this is for Windows - if you use a Mac line 58 should probably be just jekyll not jekyll.bat**

```js
var gulp        = require('gulp');
var browserSync = require('browser-sync');
var cp          = require('child_process');
var sass = require('gulp-sass');
var postcss    = require('gulp-postcss');
var sourcemaps = require('gulp-sourcemaps');
var autoprefixer = require('autoprefixer');
var watch = require('gulp-watch');
var uglify = require('gulp-uglify');
var cssnano = require('cssnano');
var imagemin = require('gulp-imagemin');
var htmlhint = require("gulp-htmlhint");

var messages = {
    jekyllBuild: '<span style="color: grey">Running:</span> $ jekyll build'
};
// Gulp as asset manager for jekyll. Please note that the assets folder is never cleaned
//so you might want to manually delete the _site/assets folder once in a while.
// this is because gulp will move files from the assets directory to _site/assets,
// but it will not remove them from _site/assets if you remove them from assets.

/**
 * Build the Jekyll Site
 */
gulp.task('jekyll-build', function (done) {
    browserSync.notify(messages.jekyllBuild);
    return cp.spawn('jekyll.bat', ['build'], {stdio: 'inherit'})
        .on('close', done);
});

/**
 * Rebuild Jekyll & do page reload when watched files change
 */
gulp.task('jekyll-rebuild', ['jekyll-build'], function () {
    browserSync.reload();
});

/**
 * Wait for jekyll-build, then launch the Server
 */

gulp.task('serve', ['jekyll-build'], function() {
    browserSync.init({
        server: "_site/"
    });
});

/**
 * Watch jekyll source files for changes, don't watch assets
 */
gulp.task('watch', function () {
    gulp.watch(['**/*.*', '!_site/**/*','!_assets/**/*','!node_modules/**/*','!.sass-cache/**/*' ], ['jekyll-rebuild']);
});

//watch just the sass files - no need to rebuild jekyll
gulp.task('watch-sass', ['sass-rebuild'], function() {
     gulp.watch(['_assets/sass/**/*.scss'], ['sass-rebuild']);
});

// watch just the js files
gulp.task('watch-js', ['js-rebuild'], function() {
     gulp.watch(['_assets/js/**/*.js'], ['js-rebuild']);
});

// watch just the image files
gulp.task('watch-images', ['images-rebuild'], function() {
     gulp.watch(['_assets/img/**/*.*'], ['images-rebuild']);
});

//if sass files change just rebuild them with gulp-sass and what not
gulp.task('sass-rebuild', function() {
     var plugins = [
        autoprefixer({browsers: ['last 2 version']}),
        cssnano()
    ];
     return gulp.src('_assets/sass/**/*.scss')
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(sourcemaps.init())
    .pipe(postcss(plugins))
    .pipe(sourcemaps.write('.'))
    .pipe( gulp.dest('_site/assets/css/') )
    .pipe(browserSync.reload({
      stream: true
    }))
});

gulp.task('js-rebuild', function(cb) {
    return gulp.src('_assets/js/**/*.js')
      .pipe(uglify())
      .pipe( gulp.dest('_site/assets/js/') )
      .pipe(browserSync.reload({
      stream: true
    }))
});

gulp.task('images-rebuild', function(cb) {
   
     return gulp.src('_assets/img/**/*.*')
      .pipe( gulp.dest('_site/assets/img/') )
      .pipe(browserSync.reload({
      stream: true
    }))
});

/**
 * Default task, running just `gulp` will 
 * compile the jekyll site, launch BrowserSync & watch files.
 */
gulp.task('default', ['serve', 'watch','watch-sass','watch-js','watch-images']);


//build and deploy stuff
gulp.task('imagemin', function() {
    console.log('Minimizing images in source!!');
 return gulp.src('_assets/img/**/*')
    .pipe(imagemin())
    .pipe(gulp.dest(function (file) {
        return file.base;
    }));
});

gulp.task('w3', function() {
gulp.src("_site/**/*.html")
    .pipe(htmlhint())
    .pipe(htmlhint.reporter())
})
// validate from the command line instead, works better
// npm install htmlhint -g
// htmlhint _site/**/*.html


```

Here is the package.json file - this loads all the dependencies needed in the Gulp tasks, you need to run 'npm install' in the root of your project to install them. They will end up in a `node_modules` folder, and this may take a while, there will be a lot of files. 

```json
{
  "devDependencies": {
    "autoprefixer": "^7.1.4",
    "browser-sync": "^2.2.0",
    "cssnano": "^3.10.0",
    "gulp": "^3.9.0",
    "gulp-cssnano": "^2.1.0",
    "gulp-htmlhint": "^0.3.1",
    "gulp-imagemin": "^2.4.0",
    "gulp-postcss": "^7.0.0",
    "gulp-rename": "^1.2.2",
    "gulp-sass": "^2.1.1",
    "gulp-sourcemaps": "^1.6.0",
    "gulp-uglify": "^1.5.1",
    "gulp-watch": "^4.3.5"
  }
}

```

In the jekyll config file you have to keep the assets folder from being deleted from the site folder. I am using `_assets` as the source folder and folders with an underscore are ignored by jekyll by default - not sure this is the best name for it though, might end up using something else:

```yaml
keep_files: [assets]   
```
