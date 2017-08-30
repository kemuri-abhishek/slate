---
title: RCMS Startup Guide

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://r-cms.jp/'>RCMS Official</a>

search: true
---

# Setting Up the Environment

To set up the environment for testing and development purpose, one must install the following softwares and applications

  * Docker
  * Virtualbox for docker machines
  * Sublime Text (Or any other text editor)
  * Git
  * Java JDK (For test cases)
  * Selenium server standalone 2.53.1 jar (for text cases)
  * Chrome, Mozilla, Safari, Internet Explorer, Opera (for cross browser testing)

## Setting Up docker

You need to install docker on you machine in order to create a virtual environment. 
Here is the link to download docker stable version:   

* [For Mac](https://download.docker.com/mac/stable/Docker.dmg) or <https://download.docker.com/mac/stable/Docker.dmg>
* [For windows](https://download.docker.com/win/stable/InstallDocker.msi) or <https://download.docker.com/win/stable/InstallDocker.msi>
* [For Ubunutu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) or <https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/>


### Steps
1. Once installed, create a docker machine (if not already created) named default  using the command: docker-machine create default

2. To verify, enter docker-machine ls command to list all the available docker machine.

3. Get the docker container site URL from the site administrators on which you are working.

4. The site URL would look somewhat like below: 
<br />
`curl -L "https://rcms-backup.s3.amazonaws.com/docker_install_shell/docker_install_shell30843d7b0faba03977e3aa24c8231302?AWSAccessKeyId=15TV6X9W3KCCT8808ER2&Expires=1503975580&Signature=leFKoCTARfmhiG0%2BZF1IDYAf9J8%3D" | sh`

5. Make sure you have cloned the correct repositories before proceeding. If not, please ask the system administrators to give access to the following repositories:
  * <https://github.com/diverta/RCMS-sites>
  * <https://github.com/diverta/RCMS-OpenDev-ClosedBeta>  
  _(Skip if already done earlier)_<br />

6. Once given the access to the above repositories, fork and clone the repositories to your machine.
  <br /> _(Skip if already done earlier)_<br />

7. Once you have the latest container URL and repositories, goto the following link to get the script to generate a container  : <http://terashima.dev.diverta.jp/dockerconv.php>

8. You will be asked to enter following details in the form: <br />
  * Temporary Dir(GIT_RCMS) => temporary folder to store temporary data  
  * OpenDev Dir => Path to RCMS-OpenDev-ClosedBeta repository  
  * sites Dir => Path to RCMS-sites repository  
  * Docker Machine Name => default  
  * Docker Machine IP => 192.168.99.100 (can be different but we use this one)  
  * Docker起動コマンド => The URL obtained in above step.  
<br />

9. On submitting you will find following script: <br />_(Just an example)_  
<code>
rm -Rf /Users/kemuri/github/tmp/sites/122544
  mkdir -p /Users/kemuri/github/tmp/sites/122544/install/122544/
  chmod -R 0777 /Users/kemuri/github/tmp/sites/122544
  eval "$(docker-machine env default)"
  curl -L "http://d.dev.diverta.jp/599c0ab2ec273" | sh
</code>
<br /> Copy the script / commands and paste it in the terminal window.At the end of execution, it will ask for the machine user password. Enter the password and proceed.

10. To verify the container installation, run docker ps -a command and look for the container status and hit http://192.168.99.100/ in the web browser.  
<br />

If everything went well without any errors, you would be able to see the website on the local environment. If some error occurs, please consult the site administrator.
<br />
To login to a container environment, use the following commands,
<br />
`docker ps : to find the active container id`<br />
`docker exec -it <container-id> bash`
<br />to login to the container

<br />
Once logged in to the container, you can now access and modify the local database postgres and the source code that is currently executing on the container for the testing purpose.


# RCMS Contents and Features

There are a lot of features and modules embedded in RCMS. Here, only a few but most common are described briefly. You are likely to work on one or many of these at a time.
The modules covered here include,

* Page Structure
* Custom CSS / Design
* Header and Footer settings
* Template management
* Testcases
* Member Management 

Let’s get started with the mentioned list.


## Page Structure

### Usecase
Page structure is used to manage the page/ urls in a website and their structure. 
It has the list off all the possible urls that the website has. With page structure we can manage the   modules that a page should include and also modify the positioning of those modules.

### Features:
* Add custom name to the page
* Add custom route/ url for the page
* Make hierarchy of pages (A page can be assigned under any parent page, TOP is the default parent of every page)
* Add access restriction/ Read restrictions. Three options are provided, 1. Anyone can read, 2. Only a set of particular members can read, 3. Only a set of particular members can read (password protected)
* Include page in sitemap for SEO and indexing.
* Enable page for mobile view, JSON formatted etc.
* Set layout of the page
* Add custom CSS for the page
* Manage Main module and sub modules

### Management Url
[Url](http://192.168.99.100/management/page/page_list/) : <http://192.168.99.100/management/page/page_list/>


## Custom CSS / Design

### Usecase
If you want to add some css libraries or some custom css files, you can do this using design / CSS module. Here you have options to upload css files as well as you can create css files in the online editor also. 

### Features:
* Manage external css files included in the website.
* Used to manage default theme design like primary color, secondary color etc configuration.
* Can add custom Header Image
* Can edit existing CSS files in online editor

### Management Url
[Url](http://192.168.99.100/management/design/design_list/) : <http://192.168.99.100/management/design/design_list/>


## Header and Footer settings

### Usecase
In order to edit the Header content and design or including some css or library etc, the header and footer templates can be found using the above url.
You can add META tags and other SEO related content in the head template.
To add custom scripts which are required globally, you can add the javascripts in the footer template.

### Things to be taken care of:
* Since the header and footer are included in almost all the pages, try not to add any page specific code as it may affect other pages and UI etc.

### Management Url
[Url](http://192.168.99.100/management/headerfooter/headerfooter_list/) : <http://192.168.99.100/management/headerfooter/headerfooter_list/>


## Template management

### Usecase
Templates are page specific design which can be edited in order to update view or functionality of a page. These can be imagined as embedded HTML with smarty for the views.
If you want to edit a view, find the template from the template list and select Edit template (leftmost column). An editor will appear with the current state of the template which you can edit accordingly.

### Things to be take care of:
* Make sure you are in the right template before editing.
* Other should not be editing the same template at the same template. (You can use the history feature to look for the changes that has previously been made to the template)
* Template could be generic i.e. can be used by different views also. So be sure your change won’t break other views.
* Every template has its own history maintained, so be sure not to override someone else’s changes. You can revert to previous version or can compare two versions of the template using history feature.

### Management Url
[Url](http://192.168.99.100/management/templateedit/templateedit_list/) : <http://192.168.99.100/management/templateedit/templateedit_list/>


## Testcases
### Usecase
### Features
### Management Url

## Member Management
### Usecase
### Features
### Management Url


# Time and Task Tracking

## myHours
## TimeSheet
## Redmine Task management portal
## Activecollab task tracker