---
---
**I have an updated Gulp file that I use now, it processes all asstes including SASS which speeds up the Jekyll build time quite a bit**  
[Speed up Jekyll use Gulp](/2017/10/01/speed-up-jekyll-by-using-gulp-for-sass-and-other-assets/)  

I've been using browser-sync with Gulp for auto re-loading the browser whenever I save a file. It is pretty cool!

When I was researching how to do this I found a few different ways to do the Gulpfile, some of which were really long and obtuse. I started using one that seemed simple, and over time have been able to understand more of it and have simplified it a bit more.

Line 30 below is subject to some confusion. The watch function is the meat of the Gulpfile - if you don't get this right you can get all kinds of weirdness - like the site not regenerating, or an endless loop of regeneration. Originally in this function I was specifying every folder to watch. So I was putting every sub folder in a list there, and if I added something new I had to remember to add it to the list. Then on SO I answered a question and the OP was using a wildcard that I was unaware of:  
`**/*.*`  
which is basically everything. He was using that but it was including the _site folder, which really needs to be excluded, and can be done with:  
`!_site/**/*`  

This works great, I then also exclude any other folders that are not part of the site.

Also, I am using a PC, line #12 needs to have the .bat at the end of jekyll. It looks like on OSX or Linux you can leave that off.

 {% highlight javascript linenos %}
var gulp        = require('gulp');
var browserSync = require('browser-sync');
var cp          = require('child_process');

var messages = {
    jekyllBuild: '<span style="color: grey">Running:</span> $ jekyll build'
};

/** * Build the Jekyll Site (windows) if you use Mac/Linux change jekyll.bat to just jekyll */
gulp.task('jekyll-build', function (done) {
    browserSync.notify(messages.jekyllBuild);
    return cp.spawn('jekyll.bat', ['build'], {stdio: 'inherit'})
        .on('close', done);
});

/** Rebuild Jekyll & do page reload when watched files change */
gulp.task('jekyll-rebuild', ['jekyll-build'], function () {
    browserSync.reload();
});

/** Wait for jekyll-build, then launch the Server */
gulp.task('serve', ['jekyll-build'], function() {
    browserSync.init({
        server: "_site/"
    });
});

/** Watch all files for changes, except the _site and other unneccessary folders */
gulp.task('watch', function () {
    gulp.watch(['**/*.*', '!_site/**/*', '!node_modules/**/*','!.sass-cache/**/*' ], ['jekyll-rebuild']);
});

/**Default task, running just `gulp` will compile the jekyll site, launch BrowserSync & watch files. */
gulp.task('default', ['serve', 'watch']);
{% endhighlight %}
