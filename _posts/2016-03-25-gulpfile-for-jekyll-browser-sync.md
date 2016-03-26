---
---
I've been using browser-sync with Gulp for auto re-loading the browser whenever I save a file. It is pretty cool!

When I was researching how to do this I found a few different ways to do the Gulpfile, some of which were really long and obtuse. I started using one that seemed simple, and over time have been able to understand more of it and have simplified it a bit.

```javascript
var gulp        = require('gulp');
var browserSync = require('browser-sync');
var cp          = require('child_process');

var messages = {
    jekyllBuild: '<span style="color: grey">Running:</span> $ jekyll build'
};

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
 * Watch all files for changes, except the _site and other unneccessary folders
 */
gulp.task('watch', function () {
    gulp.watch(['**/*.*', '!_site/**/*', '!node_modules/**/*','!.sass-cache/**/*' ], ['jekyll-rebuild']);
});

/**
 * Default task, running just `gulp` will 
 * compile the jekyll site, launch BrowserSync & watch files.
 */
gulp.task('default', ['serve', 'watch']);
```
