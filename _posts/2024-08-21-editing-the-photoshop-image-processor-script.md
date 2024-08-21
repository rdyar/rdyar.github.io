---
---

I've always wanted to change the way the image processor works in Photoshop by having it move the original files to their own folder and then have the saved images be back in the parent folder. After a lot of trial and error I finally got it working.

The reason we want to do this is the online ordering system our [photo lab](https://prolabprints.com) uses gives us a customers originals files and then the rendered files with cropping for whatever they ordered. If we need to color correct the images we like to correct the original files and then re-render the order. We color correct using Adobe Camera Raw (ACR) and when done we run the images thru the image processor. But that outputs them to a JPEG sub folder and we would then have to drag them back up and overwrite the originals.

You can find what I did on [bitbucket](https://bitbucket.org/fpl619/rons-image-processor/src/master/).

Things I learned:

- Photoshop keeps all the scripts in the same global scope - so if you have the same variable names or function names in 2 different scripts they may clash. Since I duplicated the original image processor I had some issues with this. I add a prefix to a handful of variables and functions and it seems ok.
- AI is your friend, GH copilot understood what I wanted to do and helped in a couple places where I was lost.
