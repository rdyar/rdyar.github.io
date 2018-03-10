When you use `Find in Files` with Sublime text and you are using Gulp you probably have a zillion files in the `node_modules` directory which will probably pollute your search results.

You can exclude stuff on the `Where` line by using a `-`.

Here is what I have found useful to exclude using the Where box:

-node_modules/*,-_assets/*,-_site/*

This is excuding node_modules, the _site folder and the assets folder where I keep all the CSS, images and JS.
