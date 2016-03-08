---
---

Yesterday I setup my first GH pages project website as a sub domain for my business -  [ProLabPrints Photo Lab](http://prolabprints.com). Usually I host on Amazon S3, but for this project I wanted to try out GH pages. This post goes over a few things that threw me off.

**_This post is mostly for hosting a jekyll site as a sub domain on a GitHub project repo using the gh-pages branch._**

The site I have hosted like this is a status monitor website which tells you if a few of our web services are working or not. The site is [http://status.prolabprints.com](http://status.prolabprints.com) and the repo is [https://github.com/rdyar/plp-status](https://github.com/rdyar/plp-status).

## GH pages Branch Setup

I've been playing with GH Pages for a while, and for the most part don't have trouble with existing projects, but when starting a new one I have been having trouble. The trouble usually has to do with having an existing project folder locally (no version control yet) and wanting to push it up to a new GH repo. Apparently that is not how you do it.

**This is for project pages - not your one username repo.**

This is how I think I am going to do this in the future.

Do the following things on the GH website first.

- Make a new repo, add the readme file, .gitignore file and a license if you want.
- Create a gh-pages branch - just click on the Branch button and type in 'gh-pages' and press enter.
- Make gh-pages branch the default. Click on the Branches link, then change the default branch.
- Delete the Master branch. Click on Branches then click the trash can delete button next to the Master branch. This is optional, but if you are only using the gh-pages branch the Master branch seems weird to keep around, and this makes things simpler.

Now do the following locally:

- In the parent folder of where you keep your projects locally, open up a command prompt (on windows I do 'shift + right click' 'Open command window here') and clone the repo you created above by copying the https link from your repo on GH and in your command window type:  
 ```git clone https://github.com/yourusername/your-repo-name.git```  
 (example make sure you change this to what you copied off GH).
   - I do the clone inside the parent folder of where I keep my projects locally. Do not create a new sub folder with your new projects' name, and don't do this from within your existing project. When you clone from GH it will create a folder with the GH repo name. You are going to use this as the folder for your project.
- Now you should have a new folder locally named whatever you named the repo on GH.
- Copy your project files into this folder, this is where it will exist now.
- Make sure you have a .gitignore file! in the next steps we are going to add all the files into Git to track them. Now is the time to make sure your .gitignore file is going to ignore files like _site, .sass-cache, s3_website.yml, .node-modules etc.. Especially make sure you are not committing anything with passwords or keys. Once you commit all your files it is a pain to remove anything, and since this example is with GH which has free acounts that are public you need to make sure you ignore files that you don't want others to see, as well as things like _site that just aren't needed.
- You need to add all the files and commit them, then push this to GH.
   - In your command window (you now need to be in the project folder) do:
      - ```git add .``` this adds all the files 
      - ```git commit -m "initializing message"``` this commits the tracked files, the message is whatever you want to type.
      - ```git push origin gh-pages``` this pushes your local files to your GH project repo.  

With any luck you should now have pushed your new project up to your new GH repo, into the gh-pages branch, ready to serve up your jekyll site.

## Setting the CNAME up to Serve as a Sub Domain

My main site is hosted thru Amazon S3. While I think GH Pages is great, I like S3 better for hosting my actual business critical website. Our status website isn't particularly critical so I wanted it to be on Gh and then run as a sub domain of our main website: [http://status.prolabprints.com](http://status.prolabprints.com).

In order to do a sub domain you just add a file named CNAME to your repo. There is no file extension, and it has to be all caps. Inside this all you put is your sub domain - so mine was just 'status.prolabprints.com'. There is no http or https in front, just the name.

Then you need to add the CNAME in your DNS (this is outside of GH). We use AWS for pretty much everything, including their DNS service Route 53. In Route 53 it is easy to add a CNAME record, you just click 'Create Record Set' and type in a name which is the sub domain you want - for me this was 'status'. Then you choose the Type from the dropdown - CNAME. Then you type in the Value... to me this seems obvious that it would be the url of the live repo site http://rdyar.github.io/plp-status. But after an hour it wasn't working. So then I tried it without the 'http://' part, still no luck. I had seen in the GH docs a reference to using just your username.github.io so eventually I tried 'rdyar.github.io' and it worked. I had assumed this was for hosting your username repo via the Master branch, but apparently that is what you put for any CNAME as well.

Now it all works and it is pretty cool!

## Video Showing How to Push to gh-pages Branch

I made a video showing the first part above - pushing existing web site to a new repos' gh-pages branch.

 <div class="responsive-video margin-bottom-30">
                                 <iframe width="940" height="480" src="https://www.youtube.com/embed/iyFjdmzcpws?modestbranding=1;autohide=1&amp;showinfo=0&amp;rel=0" frameborder="0" allowfullscreen></iframe>
                            </div>
