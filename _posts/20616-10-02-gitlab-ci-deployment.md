I'm currently playing around with [https://gitlab.com](https://gitlab.com) and their CI for deploying a jekyll website.

My current workflow publishes my sites to S3/Cloudfront after running them thru a Rake task that minifies the js/css/html files. I also have octopress-autoprefixer installed and that allows jekyll to autoprefix the css as it builds the site.

I'm pretty happy with all 3 of these things in my current setup, but that means I always have to build locally and then deploy to the S3. I would really like to be able to edit things online in the repository directly - when I have just a minor change - as it is so much easier.

I think I will be able to do this with Gitlabs CI, but I have not found any examples that do all 3 things:

- Auto-prefix the css (working)
- Minify all the files
- Deploy to S3

So far I think I have the auto-prefix part solved, but it was not easy. Simply adding the octopress gem didn't work, there was an error about there not being a js runner, which is where the rubyracer gem comes in. Once I added that it was having an error: `jekyll 3.2.1 | Error:  "\xE6" on US-ASCII`. That seemed to do with the locale not being set to utf-8, so I found a post with a way to set that, added it in and now it is working with this gitlab.ci.yml file:

~~~
  image: ruby:2.3
  test:
   stage: test
   script:
    - gem install jekyll
    - jekyll build -d test
    artifacts:
     paths:
     - test
    except:
    - master
  pages:
    stage: deploy
    script:
  - apt-get update -y
  - apt-get install -y locales
  - echo "en_US UTF-8" > /etc/locale.gen
  - locale-gen en_US.UTF-8
  - export LANG=en_US.UTF-8
  - export LANGUAGE=en_US:en
  - export LC_ALL=en_US.UTF-8
  - gem install therubyracer
  - gem install jekyll
  - gem install octopress-autoprefixer
  - jekyll build -d public
  artifacts:
    paths:
    - public
  only:
  - master
~~~
