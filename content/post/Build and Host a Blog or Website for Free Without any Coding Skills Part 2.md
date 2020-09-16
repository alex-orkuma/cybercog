---
title: Build and Host a Blog or Website for Free Without any Coding Skills Part 2
subtitle: Deployment to Netlify, Buying and Attaching a Domain Name.
date: 2020-09-16T10:30:00+00:00
tags:
  - beginner
  - blog
  - Hugo
  - netlify
  - github
  - deployment
  - namecheap
  - domain name
---

In this tutorial we are going to talk about how to deploy our blog from the previous [post](https://cybercog.co/post/build-and-host-a-blog-or-website-for-free-without-any-coding-skills-in-50-mins/) to netlify. Before we deploy to netlify, make sure you have a github account and it is set up locally on your system. We will first push the code to github then pull from to netlify.

## Commiting Code to Github

- signin to your github account
- create a new repository by clicking on the cross next to the bell on the navbar, give it the same name as the name of your site's folder.
  ![](/uploads/createrepo.png)

- After creating the repository, copy the url of the repository that is shown on the screen, it should be in the form of "https://github.com/your-user-name/your-site-name.git"
- Navigate to your terminmal in a seperate directory from your site's directory and type the following command:
  {{< highlight bashrc  >}}
  git clone https://github.com/your-user-name/your-site-name.git
  {{< / highlight >}}
  and press enter.
- Navigate to the parent directory of your site in my own case it's sadaxx, copy everything from that directory and paste it into the directory of the repository you just clonned from github. It should have the same name as the parent directory of your site.

- Go back to your terminal, change directory into the directory of the github repository you just clonned.

- Type the following commands:

  {{< highlight bashrc  >}}
  git add .
  {{< / highlight >}}

  {{< highlight bashrc  >}}
  git commit -m "type your commit message"
  {{< / highlight >}}

  {{< highlight bashrc  >}}
  git push origin master
  {{< / highlight >}}
  your code will be pushed to your github account.

## Deploying your Website to Netlify

- Visit [Netlify](netlify.com) then click on signin
- From the signin options, select sigin through gihub
- wait for it to authenticate and that is it you have succefully connected your github account to netlify.
- On the dashboard, click on "New site from Git"
- Select Github from the option available, wait for it to authorize.
- From the list, select the repository you created initially.
- Netlify will automatically detect it is a hugo site and populate it with the necessary configuration parameters.
- Click on deploy button.
- Wait for a few minuests for Netlify to do it's magic, build and deploy your website.
  If everything goes well, you should see a screen similar to this
  ![](/uploads/deployed.png)
- To access your website, look on the dashoboard and copy the link that starts with "https://.." When you create a new site, netlify gives you a random public netlify sub domain name to be able to access your website directly from the browser. Don't worry, you will learn how to change this and attach your own domain name.
- To modify the automatically asigned name, click on domain setting on the dashoboard and modify the domain name.

  **NOTE:**The domain name must be unique gloabally and because you have not attached your domain name yet, it will still carry the netlify top domain.

## Buying a Domain Name.

There are a lot of places online where you can buy a domin name, godaddy, namecheap Aws route53 to name a few. For the sake of this tutorial we are going to buy a domain name from [Namecheap](https://www.namecheap.com/)

This tutorial asumes you already have a name in mind so we won't be discussing how to choose a name for your domain.

- Navigate to [namecheap](https://www.namecheap.com/).
- On the search bar, enter the you want your domain name to have. namecheap will check if the domain name is taken or not. that is why it is very important to choose an uinque name.
  Don't worry if the name is taken, name cheap will suggest other ones for you or you can come up with another one.

![](/uploads/domainname.png)

As you can see from above my .com is not taken.

- If you have found the one you like, go ahead and click on the cart icon to add it to cart,
- Navigate to the cart click on checkout and input your credit card details, you will be asked for other informations too like address, email address. Just go ahead and fill it and buy the domain name.

## Attaching a Namecheap Domain Name to a Netlify Website.

To attach your namecheap domain to a netlify website, refer to this article on [Dev.to](https://dev.to/easybuoy/setting-up-domain-with-namecheap-netlify-1a4d)
