---
layout: post
title: "Setting up this blog"
date: 2015-01-01 22:24:21 -0600
comments: true
categories: [coding, blogging]
---

For 2015, I've decided to start publishing the information that I've digested instead of just spreading it haphazardly via word-of-mouth or retweeting links in social media. To that end, I decided to setup a blog using Github Pages.

I run a stock Ubuntu 14.04 desktop system at home, so I opened up Chrome and contemplated my options for blogging engines. My preliminary research found Jekyll, Octopress, Middleman, and Nanoc. I opted to stay simple and go with Octopress as my first attempt, since that came well recommended.

I needed to install Ruby on my system and just to try something different (I use RVM at work), I opted to use chruby and ruby-install:

```bash
# chruby
wget -O chruby-0.3.9.tar.gz https://github.com/postmodern/chruby/archive/v0.3.9.tar.gz
tar -xzvf chruby-0.3.9.tar.gz
cd chruby-0.3.9/
sudo make install

# ruby-install
wget -O ruby-install-0.5.0.tar.gz https://github.com/postmodern/ruby-install/archive/v0.5.0.tar.gz
tar -xzvf ruby-install-0.5.0.tar.gz
cd ruby-install-0.5.0/
sudo make install

# install latest ruby (2.1.3)
ruby-install ruby

# use it
chruby 2.1.3

# setup bash to autoload it
echo "source /usr/local/share/chruby/chruby.sh" >> ~/.bashrc
echo "source /usr/local/share/chruby/auto.sh" >> ~/.bashrc
echo "chruby 2.1.3" >> ~/.bashrc

```

Then I installed Octopress by cloning the repo and running the commands in its tutorial.

```bash
git clone git://github.com/imathis/octopress.git octopress
cd octopress
gem install bundler
bundle install
rake install
```

I configured Octopress to deploy to GitHub Pages:

```bash
rake setup_github_pages
rake generate
rake preview
```

For what it's worth, I could not get the `rake preview` to work until I installed a Javascript runtime. I chose to install node.js using apt-get:

```bash
sudo apt-get install nodejs
```

I made some minor tweaks to the generated `_config.yml` file to customize it for my name and title, then I generated this blog post:

```bash
rake new_post["Setting up this blog"]
vim source/_posts/2015-01-01-setting-up-this-blog.markdown
```

Lastly, I committed my changes and published the post.

```bash
git commit -a
git push
rake deploy
```

