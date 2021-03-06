Host Up
============

<br />

## hostUp - A simple URL Status Checker for Uptime
#### Written with MeteorJS using NodeJS, HTML5, Javascript, and MongoDB

![Imgur Image](https://imgur.com/EtEcrzE.gif)

<a id="hostUp-toc" name="hostUp-toc"></a>
## Table of Contents
* [Running in Dev Mode](#dev-mode)
* [User Basics](#user-basics)
* [Demo Site](http://hostup.routemehome.org) demouser: demo  /  demo password: demoadmin
* [Configuration](#config)
* [Production Mode](#prod-mode)
  * [Production Mode - Need to Install](#prod-mode-needs)
  * [Production Mode - Run It Forever](#prod-mode-forever)
  * [Production Mode - Build](#prod-mode-build)
    * [Production Mode - Set Some Environmental Variables](#env-vars)
* [Install with Docker](#install-with-docker)
* [To Do Still](#to-do-still)
* [Contribute](#contribute)
* [License](#license)

<br />

<a id="dev-mode" name="dev-mode"></a>
### Running in Dev Mode

To run it in Dev mode you'll need to do the following:

1. Get Meteor Installed

    Windows - There's an installer out there.
    Mac and Linux use this command:

`curl https://install.meteor.com/ | sh`

2. Clone this repo
3. cd into the cloned repo directory
4. run the command

`meteor npm install`

5. now run the command

`meteor`

If you get any errors, such as needing to install babel-runtime, or bcrypt, just use the commands provided, then run the `meteor` command again to make sure it starts.

Once it's running you should see this in your terminal:

```
[[[[[ ~/hostUp ]]]]]

=> Started proxy.
=> Started MongoDB.

=> Started your app.

=> App running at: http://localhost:3000/
```

Or something very similar.

<a id="user-basics" name="user-basics"></a>
### User Basics
Now you can navigate to localhost:3000 as indicated to see the page.  Use the Menu icon in the upper left of the screen to get to the host input screen.  Input your first url to check.  Please use full URLs including http or https.

Once you've added a URL or two to check, you can navigate back to the Host List view. The hosts you are monitoring should show in a grid, and you can see their status, the last time they were checked, and the next time they'll be checked in the view. You can edit or delete a host using the Action icons at the right of each column.

Clicking on a column will bring up the Ping data for that host.  It brings back the last 1000 ping time results in a chart.

The menu icon in the upper left allows you to move to the input screen and back to the monitor screen.

*NOTE*: this application just checks to see if the site is up or not.  This is not a watcher for changes.

<a id="config" name="config"></a>
### Configuration
You can setup default time in minutes for each check of the entered sites to occur, as well as the max number of sites a user can have.

#### Email Setup
You can setup the system to email out a notice to a specified email for each site you are monitoring.  You must setup the SMTP send mail information and save it.

I have tested this and it does work.

##### Example
Email Username: greg

Email Password: f93kfjaldj459%4k3l# 4JJ

SMTP Server URL: smtp.gmail.com

SMTP Port: 587

*Note*: Gmail is a tricky little beast. If you setup Gmail on a remote server such as with Digital Ocean or some other VPS provider you'll need to set up your security in Gmail to allow it to send.

It's better to use a self-hosted email server, or something like MailGun or similar system intended for emailing out from an automated system.

<a id="prod-mode" name="prod-mode"></a>
### Production Use
To run this project in a production environment, you'll want to get a few things installed.  I'm going to use instructions for a Linux / Ubuntu 16.04 or 18.04 LTS server.

I do run all of my public servers on Digital Ocean.  I love their ease of setup, and ease of use.  There's my plug.  I get nothing for it, so please sign up and enjoy.

<a id="prod-mode-needs" name="prod-mode-needs"></a>
### What you need
  1. A server running some fairly recent form of linux. First, Update the server to the latest files.

    sudo apt update && sudo apt upgrade -y

  2. NPM and NOdeJS installed on that server.  Currently you'll want NPM version 5.10 and NOde version 8.11.3 - Here are some instructions on how to install NodeJS on Ubuntu 18.04.  It's from Digital Ocean, but it applies regardless.

    https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04

  3. Meteor installed on the server (for the build).  You can go to `https://www.meteor.com/`, or you can use this install script (provided by MeteorJS on their page).

    curl https://install.meteor.com/ | sh

  4. You'll need to install git on the server.  For Ubuntu just do this command:

    sudo apt install git

  5. You'll need to install MongoDB, either on this same server, or on another server, and know how to point to it (it's not hard, really).

    https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04

  Again, I'm pointing to a Digital Ocean article, but it's the same for any Ubuntu install essentially.

<a id="prod-mode-setup" name="prod-mode-setup"></a>
### Setup and Install - Production Mode
  6. Clone the repository from github.  

    git clone https://github.com/bmcgonag/hostUp.git

  7. Change into the directory created by cloning the repo.

    cd hostUp

  8. Inside the hostUp directory, you'll want to run the `meteor` command.

    meteor

  This will get the version of meteor necessary for the build, as well as get the initial packages installed.

  You'll likely need to `CTRL+C` after it starts, as you'll get some errors in the terminal window.

  Not to worry, this is normal.

  9. Run the `meteor npm install` command.

    meteor npm install

  This will install the necessary npm packages.  

  You may still get a few errors, where it says to install certain packages using the

    meteor npm install <package name>

  command, just go with it.

  I used packages for ShellJS, HighCharts, and possibly a couple more.

  10.  Re-run the `meteor` command.

    meteor

  Now you should get output like this

    [[[[[ ~/hostUp ]]]]]                          

    => Started proxy.  
    => Started MongoDB.
    => Started your app

    => App running at: http://localhost:3000/

   11. Test out the basec page loading and functions by going to your server ip or URL at port 3000, for example `10.21.3.41:3000` or `http://mysuperawesomeurl.com:3000`

   12. If everything is working, then use `CTRL+C` to stop the meteor app from running, and let's build.

<a id="prod-mode-forever" name="prod-mode-forever"></a>
### Production Mode - Run It Forever
13a. I never say it, I noteiced, but you will need NodeJS and NPM installed on this server for Production mode.

- You'll need NodeJS 8.15.1
      
13. Install "forever" from npm onto your production server.

    npm i -g forever

>
> NOTE: you need to run the -g option with root privileges. (this may require `sudo` or whatever varian of it for your chosen OS)
>

Forever is an application that will watch your app and make sure it comes back up should it crash for some reason.
    
    14. We need to build the meteor app to run for production.  It's not hard, just takes some time.

From the app directory created when you cloned the git repo, run the following command.  Notice that I'm telling the app to build in a different directory than the one I'm in.

<a id="prod-mode-build" name="prod-mode-build"></a>
#### Production Mode - Build
    meteor build --directory ../hostUp-node

This 'compiles' the app and minifies all the css and js as well as makes the actual nodejs capable version.

15. When it's done, move back one directory, and into the newly created build directory.

    cd ../hostUp-node

16. Now, do an `ls` and notice the `bundle` directory.

    cd bundle

17. Now we need to install dependencies.

    cd programs/server  --> so the full path we are in is `~/hostUp-node/bundle/programs/server`

    npm install

This will install all npm dependencies for the production system.

18. Now we need to set some environmental variables for our app to use when running.

First let's set the MONGO_URL (tell the app where to connect to our Mongo DB).  Remember this can be run locally, but certainly doesn't have to be, so yoru MONBO_URL may refer to a different server URL or IP.

<a id="env-vars" name="env-vars"></a>
#### Set Some Environment Variables
    export MONGO_URL="mongodb://127.0.0.1:27017/hostUp"

This command tells the system to find mongodb on our local server, and to use port 27017 (however, if you told mongo to run on a different port, then please change that number to match the port number you selected), and to use a database called "hostUp".

19. Now we'll tell our app what it's main URL is (basically what a user will type into their browser to get to our web based application).

    export ROOT_URL="http://<your web site name or IP>"

20. Now, we tell our app that it should run on a port.  If you want someone one to just type the name of your site and get straight to your app, then use port 80.  Any other port (besides 443) will need the user to enter the port after your web site name like this `http://<my-site-name>:port`

>
> NOTE: you can also setup NGinX web server to act as a reverse proxy to your site.  There are several tutorials on this, as well as tutorials on using LetsEncrypt to get a free SSL Certificate.   I highly recommend doing this if you intend to have a production site.
>

    export PORT=80

### Now try the site.
We can run a very simple test to make sure we've set everything up properly.

Move back to the `bundle` directory:

`cd ..`  <-- move back one folder level

`cd ..`  <-- move back one folder level again.  We should now be in the bundle directory

Now, Run the command:

    node main.js

from the `bundle` directory in our production app main directory.

Give it about 10 seconds, then if you don't get any errors in the terminal, go to your web site or IP.  Don't forget, that if you didn't specify a port, use port 3000, and if you specified a port other than 80, enter that port after your site name and a colon.

The site should come up to a login / welcome page.

If it does AWESOME!

If not, it's time for trouble shooting.

### Make the Site Stay Up And Running Unattended
Now, we want our site to continue running.  The issue with the `node` command, is that when we close our terminal, it will kill our node command.

For this we use a tool called "forever".

So, first, in the terminal do the key combo `CTRL+C`, this will tell our node app to stop.

Next we want to start the app again using "forever".

It's easy.

    forever start -l forever.log -o output.log -e error.log main.js

What this says is start our main.js with the forever command.  Output anything that forever would put out to the terminal window into a file called forever.log, and any normal output from our app to output.log, and any errors that happen to a file called error.log.

In my setup the file forever.log is located in /home/<my user>/.forever

When you hit enter, it should start.  Now you can close the terminal window and still get to your web application.

Enjoy!

<a id="install-with-docker" name="install-with-docker></a>
## Install with Docker
You can use the Docker Image from https://hub.docker.com/repository/docker/bmcgonag/hostup

I suggest using Docker Compose for this, as it needs a MongoDB installation to run as well, so you can run MongoDB in a separate container, and keep it all nice and clean. 

### Setup for Docker Compose
1. Install Docker and Docker Compose.
2. Create a new Directory for this install.

`mkdir hostup`

3. Move into the directory.

`cd hostup`

4. Create a docker-compose.yml file.

`nano docker-compose.yml`

5.  Copy the following into the file:

```
version: '3.0'

services:
  hostup:
    container_name: hostup
    image: bmcgonag/hostup:1.0
    restart: always
    ports:
      - "3000:3000"
    links:
      - mongo
  mongo:
    container_name: hostup_mongo
    image: mongo
    volumes:
      - ./data:/data/db
    ports:
      - "27017:27017"
```

Adjust the ports as needed in the compose file above, then save the file.

6. Run the docker-compose.yml file with

`docker-compose up -d`

After the system is up, reach it through "http://localhost:3000", or "http://<ip of server>:3000" unless you changed the port mapping (in which case use your port).

<a id="to-do-still" name="to-do-still"></a>
## To Do Still
I would like to add a per URL timer setting.  I had this in originally, but found that the way I set it up it eventually created a cascading set of checks that became so frequent they could never finish before the next set was running.  

<a id="contribute" name="contribute"></a>
## Contribute
I'm always 100% open to contributions from anyone willing to help out, learn or otherwise.

<a id="license" name="license"></a>
## License
This software is licensed as AGPL v3.0, please respect it.