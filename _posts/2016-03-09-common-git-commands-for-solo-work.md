---
---
Just learning Git and working by yourself? So am I. Here are my notes for using Git from the command line.

When working with the command line, I either open the command window by holding down the shift key while right clicking on the folder I am working in, or, in Sublime Text I right click on the parent folder and choose Open/Run - this opens Windows Powershell which is usually what I use, though I have not had any issues with the standard command prompt window.

## Commands
`git init` - this initializes a repository. I used to do this, but if all you want to do is host a website on GH pages using the gh-pages branch, then it is easier to start on GH and create the repo there, create the gh-pages branch, set it to default and delete the master branch. Then clone the repo down and start from there. When I do git init locally, I always end up with problems because locally you are in master and I haven't learned enough yet to switch to the gh-pages branch locally and push from there. Maybe one day.

`git clone http://your-repo-url` - This is how you get a copy of the repo locally. When you do this it will create a new folder locally with the repo name. So do this with the command prompt one folder up.

`git status` - tells you what it is tracking that has changes.

`git add yourfile.name` - adds that file into version control, pending a commit.

`git add .` - this adds all untracked files to version control, pending a commit. I think it may also remove files if you deleted some, but keep in mind they will still exist in the commit history.

`git commit` - commits files to the repo. You probably should get used to using the -m option to leave a message as well.

`git commit -m "my commit message here"` - this commits with a message so you know what changed at a glance.

`git commit -a -m "your message here"` - the -a option works like `git add .` but adds it to the commit, then -m adds the message.

`git push origin gh-pages` - pushes your changes up to the version control server (GitHub normally but could be BitBucket or your own private Git server).

`git push origin master` pushes to master branch.
