---

title: How to Build A Blog With Jekyll And GitHub Pages
tags: Blog
author: Anning Mao

---

## Why using GitHub Pages

If you need a lightweight personal blog service, what are the advantages of GitHub Pages over website building services such as WordPress?

* Firstly, it is completely free. Compared with other similar products, it can save a service fee, and the money saved can allow you to buy some thing else.
* There is no need to purchase cloud services to build by yourself, just follow the guidelines step by step, even if you don't understand the technical details.
* There are many supported functions and rich gameplay. You can bind your domain name, use free HTTPS, DIY your own theme, use plug-ins developed by others, etc.
* When the setup is completed, you only need to focus on article creation. Other things such as environment setup, system maintenance, and file storage are all handled by GitHub.

Of cause, as a free service, we must also comply with the official GitHub recommendations and restrictions. The size of the project and website should not exceed 1GB, and the content of the website should not be updated too frequently (no more than 10 versions per hour), and the upper limit of bandwidth usage is 100GB for each month.

On the whole, GitHub Pages can still be said to be one of best options for small and medium-sized blogs or project homepages.

## How to using GitHub Pages

### Basic page generation

Firstly, you need to register a GitHub account, and create a new repository.

![new repository](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/1.1.png)



After entering the page, you should fill in the domain name in the place of the Repository name and the format is `	username.GitHub.io`.

![formed name](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/1.2.png)



Then click Settings in the upper right corner

![setting](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/1.3.png)





Find the GitHub Pages option and choose a theme officially provided by GitHub

![Themes](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/1.4.png)

After the selection is completed, GitHub Pages will automatically generate the website for you. Click the Commit changes button on the interface where it is redirected, and the website can be accessed.

At this point, if you just want to make a resume that can be accessed on the Internet at any time, then you only need to modify your `index.md` file on the homepage of the GitHub Pages project.



## GitHub Pages generation tool

A simple HTML page may not meet your requirements other than displaying your resume. Therefore, we need to use the static template tool to take over the generation of your blog's articles, allowing you to focus on your creation. We have many options：

* Hexo written by Node.js
* Hugo written in Go
* Pelican written in Python
* And the more user-friendly Gridea

This blog will start with the example of Jekyll officially recommended by GitHub.

1. It is convenient to use RubyInstaller to install on Windows. Go to https://rubyinstaller.org/downloads/ to download the latest version of RubyInstaller. Note the difference between the 32-bit and 64-bit versions.

![ruby](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/2.1.png)





![Successful](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/2.2.png)

2. **Install RubyGems**

   It is convenient to download in Windows. After downloading, unzip it to any path. Open the cmd and enter the command:

   ```bash
   $ cd {unzip-path}
   
   $ ruby setup.rb
   ```

3. **Install jekyll**

   enter the command in cmd:

   ```ruby
   $ gem install jekyll
   $ gem install bundler
   ```

4. **Install jekyll-paginate**

   enter the command in cmd:

   ```ruby
   $ gem install jekyll-paginate
   ```

5. **Verification**

   enter the command in cmd:

   ```ruby
   $ jekyll -v
   ```

   


## Using Jekyll for you blog

### 1. Remote 2 Local

​	Using “clone” to create a local repository for the first time (Recommended) 

```bash
git clone https://GitHub.com/username/username.GitHub.io
```

​	or

​	Using “pull” to get the latest version from remote repository for further modification

```bash
git pull
```

### 2. Build a page with jekyll

1. move to your project directory

   ```
   cd 'your local repository path'
   ```

   

2. generate default pages\

   ```
   jekyll new . --force
   ```

   

The default interface looks very simple and ugly, but it doesn’t matter, you can find some beautiful themes in these websites according to your preferences

​								http://jekyllthemes.org/, https://jekyllthemes.io/, https://themes.jekyllrc.org/

The installation method is very simple. Under normal circumstances, you only need to download the theme package and unzip it completely, copy it to your GitHub Pages project directory, and overwrite your previous files. For some special themes, please refer to the installation steps given by the author.

### 3. Enable local real-time preview

Switch to the local blog repository, and enter in cmd:

```bash
$ bundle install
```

bundler will help you Install all of the required gems from your specified sources(the theme fork or clone from others should have a 'Gemfile', It stores the dependencies required)



```ruby
$ jekyll serve
```

or

```bash
bundle exec jekyll server
```

for more details:

https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll

### 4.Write some thing

the **/_posts** folder is where your blog posts will live. These files are generally Markdown or HTML. All posts must have YAML Front Matter, and they will be converted from their source format into an HTML page that is part of your static site.

To create a new post, all you need to do is create a file in the */_posts* directory. Jekyll requires blog post files to be named like these:

```
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.markdown
```

### 5.Push it to remote repository 

```bash
git add --all
git commit -m "Firs Push"
git push -u origin main:main
```



## Configure a custom domain name and use HTTPS for free

The domain names like 'anningmaoo.GitHub.io' do not looks elegant. And good news is that after May 1, 2018, GitHub Pages has begun to provide the enabling HTTPS service for custom domain names for free, and the process has been simplified. Now users no longer need to provide their own certificates, and they just  need to use a CNAME to point to your custom domain

Firstly, you have to register a domain name, and then add a resolution record to your DNS records. For example, I chose to add the subdomain name `anningcoding.life` to point to the GitHub Pages domain name `AnningMao.GitHub.io` that I just customized by CNAME. You can also add a record to point to your repository IP address directly. After the addition is complete, wait for DNS resolution to take effect (it may take several minutes for DNS resolution records to take effect globally).

![DNS](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/3.1.png)

Then go back to the Settings -> Pages that you entered at the beginning, fill in the subdomain name we just created, take my own domain name as an example, and click Save.

![add domain name](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/3.2.png)

After saving, GitHub needs a certain amount of time to generate the certificate and confirm whether the domain name resolution is correct. We only need to wait patiently. If success, the result will be displayed as follows:

![success](https://github.com/AnningMao/MarkDownImage/raw/main/How2BuildBlog/3.3.png)



end.



