---
title: Build and Host a Blog or Website for Free Without any Coding Skills in 50 mins
subtitle: Learn how build a hugo website or blog for free.
date: 2020-09-13T23:00:00+00:00
tags:
- beginner
- blog
- Hugo
draft: true

---
## **Before We Start**

here are a couple of things to keep in mind.

this is going to be a static website built using [Hugo](https://gohugo.io/documentation/). for a difference between static and dynamic websites read this [article](https://wpamelia.com/static-vs-dynamic-website/#:\~:text=Static%20websites%20are%20ones%20that,databases%20in%20addition%20to%20HTML.).

For this tutorial to work, you need to install Hugo and Golang if you don't already have it installed.

## **Installing Golang**

This tutorial was done using Ubuntu 20.04, the steps to install Golang on your OS might differ. Please refer to the [documentation](https://golang.org/doc/install?download=go1.15.2.linux-amd64.tar.gz#install) for your use case

* download Golang here [Go](https://golang.org/dl/go1.15.2.linux-amd64.tar.gz "Go")
* extract it into /usr/local, creating a Go tree in /usr/local/go

      tar -C /usr/local -xzf go1.15.2.linux-amd64.tar.gz
* Add /usr/local/go/bin to the `PATH` environment variable

    export PATH=$PATH:/usr/local/go/bin

* Verify that you've installed Go by opening a command prompt and typing the following command

      $ go version

  ## Installing Hugo
  * To install Hugo on Ubuntu, use

        sudo apt-get install hugo
  * to install Hugo on other OS, refer to the [docs](https://gohugo.io/getting-started/installing/ "Install Hugo")

    ## Create the website
    * on your terminal, type the following command

        hugo new site yousite_name
  * if everything goes well, you should see the following output. ![](/uploads/screenshot-from-2020-09-14-15-25-53.png)
  * then type the following 

        cd yousite_name  //change directory to your site name
        git init   //git init command creates a new Git repository.
    * Visit the Hugo [themes](https://themes.gohugo.io/ "Themes") site and pick any theme of your choice. Don't worry you will customize it to your taste. For the sake of this tutorial, I am using beautiful hugo theme, it is the same theme I have used for my website. 
    * After picking your favorite theme, download it by clicking the download button on the theme's home screen. It should take you directly to the Github repo of the theme. 

      ![](/uploads/screenshot-from-2020-09-14-15-41-54.png)
    * Download as zip, unzip it and rename the folder removing the master at the end of the folder name.

      ![](/uploads/a.png)
    * copy the folder to your website directory you created initially and put it under themes.
    * At this stage, your folder structure should look like this.

      ![](/uploads/screenshot-from-2020-09-14-15-55-33.png)

      ## Customizing the site.
      * First navigate to  theme, beautifulhugo, exampleSite and then content folder. Copy the post and the page folder from content and pate it in the content folder of your main site.
      * Copy the config.toml file from the exampleSite folder and replace it with the one in your main site folder. in my case my main site folder is sadaxx.

        ### Modifying the Config file

        there are a lot of things in the config file but  I am going to my best to explain the important ones. 
      * change the baseURL to the domain name of your site if you have one or just change it to a forward slash (/) if you don't have 
      * change the title to the title of your site
      * leave the theme name as it is
      * pygmentsStyle, _pygmentsUseClasses_ ,_pygmentsCodeFences_, _pygmentsCodefencesGuessSyntax_, _pygmentsUseClassic, pygmentOptions,_  are all used for code highlighting in your website. We won't get into this right now, this is a whole new blog post of its own for now leave the settings as it is. 
      * googleanylicts is to add googleanylicts to your website. replace the xxx with your tracking id. You can signup for google analytics for free and obtain the tracking id. It keeps track of how many people visit your site and from where.
      * _disqusShortname_ enables comments on your website. replace the xxx with your Disqus shortname. 
      * in the params section, change the subtitle to whatever you want
      * put your logo into the image folder, and change the logo section to point to your own logo. 
      * remove the # at the back of _hideAuthor = true_ and change the true to false if you want your name to show on every article. 
      * in the **author** section, put all your social media details there if you want to. 
      * The menu.main represent all the menu on the navbar, removing or adding will remove or add a new menu to the navbar, and of course, the URL points to a particular page or post so be sure to configure it appropriately 
      * After modifying, type 

            hugo serve

        ![](/uploads/screenshot-from-2020-09-14-16-42-01.png)
      * use see the above output in your terminal, navigate to your browser and type **_localhost:1313 _**to view ur customized website. 

      ### Modifying the Look and Feel of the site.

      To modify the colors and the look of the site, start with the css files

      #### CSS files. ![](/uploads/css.png)
    * The **main.css** takes care of the styling when you deploy it to production (that is when you host it on a platform say [Netlify](www.netlify.com))
    * **fonts.css** takes care of the font formatting, **syntax.css**, **codeblock.css**, and **highlight.css** all take of code formatting depending on the option you selected in the **config.toml** file. 
    * **hugo.easy.gallery.css** and **photoswipe.min.css** handle the formatting of pictures on the website. 

      #### The Html Files

      ![](/uploads/html.png)

      To add and remove components on the website, the HTML pages are your best bet. 
    * All the header related templates deal with the headers
    * The nav.html takes care of the navigation bar. To add or remove components on the navbar, modify the  nav.html
    * as you would expect all the post.html related handles the post section 