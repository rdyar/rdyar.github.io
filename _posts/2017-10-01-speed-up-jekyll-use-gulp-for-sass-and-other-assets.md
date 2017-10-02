---
---

I spent the last couple weeks learning/using Hugo for some reason. Hugo is really really fast at generating a site, but in the end I decided to keep using Jekyll, mostly cause I understand it better, and my sites are not that big that it is a problem.

But it is hard to go back to a slower workflow so I decided to see if i could make some changes to how I use Jekyll.

One of the things about hugo is that it doesn't currently have a sass processor, so if you want to use sass you have to use something like gulp to process the sass. I had a bit of an ah-ha moment and decided to see if I could move all of my assets stuff into a gulp task and see if that made a difference with how fast the site generates, and most importantly how fast it live reloads.

wow - move sass into a gulp workflow, and tell jekyll to ignore where the final css files ends up and you can edit css and save it and the change is reloaded instantly more or less - so jekyll is not regenerating, gulp is processing the sass into a css file and putting it in the _site folder - all blazingly fast, with source maps. This alone is a huge thing to me - though I am happy with my current css so it won't do much for me right now, but when I was coming up with my sites css I has constantly waiting for changes and now it will be almsot instant, and I can edit in dev tools and save directly to whichever partial is needed. I had never used 
