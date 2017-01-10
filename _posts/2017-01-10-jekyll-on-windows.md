---
---
Just some random notes on getting jekyll setup on a new windows box.

Used portable-jekyll.

When I first tried to run jekyll server, it didn't know it had jekyll install, so I ran `gem install jekyll` which installed it.

I added in the correct Environment Variables - ruby/bin, devkit/bin, curl/bin. Also made a new one for SSL (portable-jekyll has notes about it). Rebooted each time as they don't seem to get picked up until windows restarts.

Then it was complaining about `jekyll-watch` and `rouge` so I installed both of them too.

Eventually it started to work, but was super slow - maybe 60 seconds before it did anything.

I ran `gem list` to see all the gems that were installed, and almost all had multiple versions.

I then ran `gem cleanup` which removed a bunch of old gems. It also complained about removing ones that other things depended on, I usually said no to those.

After cleanup it ran very quickly. Then it complained about:  
`C:/portable-jekyll/ruby/lib/ruby/gems/2.0.0/gems/json-2.0.2/lib/json/version.rb:4: warning: previous definition of VERSION was here`

there were several lines like this, so I ran `gem uninstall json` and it asked which one, I said all. I then did `gem list` again and json was still there, then did `jekyll s` and the errors were gone.

Then as it was generating it said:
`Please add the following to your Gemfile to avoid polling for changes:`  
 `gem 'wdm', '>= 0.1.0' if Gem.win_platform?`
 
 So I added `gem 'wdm', '>= 0.1.0'` to the gemfile and this message went away and all is well.
