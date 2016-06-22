---
---
I have an existing windows t2 Micro running on AWS EC2 and wanted to play with another one so I spun it up and couldn't rdp into it. I tried everything i could think of - allow any ip on the rdp port, turned off my local firewall, nuked the instance and started another one (2 more) and it was the same story.

I googled it and came to:  
[https://simple-help.com/kb---troubleshooting-problems-accessing-your-ec2-instance-externally](https://simple-help.com/kb---troubleshooting-problems-accessing-your-ec2-instance-externally)

Which fixed it - the second instance didn't have a gateway, so I created one, and then had to add the second route to allow interent to pass thru to the vpc.

Now it works.
