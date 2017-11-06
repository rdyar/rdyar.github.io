---
---

Over the weekend I setup a Discource forum on AWS to play around with, it went pretty well but there were some quirks.

I was just testing so I chose the T2 Micro with 1 GB ram, which is the minimum needed to run it per their docs. What the docs don't tell you is how much storage you need. I just chose the lowest amount - 8 GB and went with that. When I ran the setup part it failed and eventually I figured out it was running out of space. I reconfigured the drive and changed it to 16 GB and once it finished resizing the install seemed to work better.

It still kept failing with this message: `Unfortunately, there was an error changing containers/app.yml`.

I have no idea what that meant, but it seemed like it didn't like saving that file so I opened it to see what was in it - `sudo vi containers/app.yml`. In there I could see it wasn't changing the password for the email server, so I typed that in myself and saved and quit - `:wq`. Vi is kind of a weird editor, takes a little getting used to.

Once I did that re-running the ./discourse-setup still did not work, though it was getting all the correct settings.

I ended up triggering the build manually by doing `./launcher rebuild app` this worked and it went thru a bunch of steps to get going and after a few minutes I was able to reach the discourse app in the browser. 

Next problem was getting emails, I used AWS SES for email, not exactly sure which step got the emails to work but I did all of the following:

- verified my email address in SES
- edited the app.yml to require TLS - this was commented out but I think it said it was on by default
- edited app.yml to set the email address from field to the email I was using since it was different from the domain I was using.

Once i did those things the emails started to work and everything else was fine. I think the last one was what did it.

I did this on a T2 micro with 16 GB storage. With a couple users making posts and using the site it seems to run at less than 2% of cpu. it runs nice and snappy.
