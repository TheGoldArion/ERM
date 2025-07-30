ERM Bot Self-hosting guide

This guide is designed to help you set up ERM on your own server or local machine, whether you're using Windows or a Debian/Ubuntu-based Linux distribution.
ERM is a powerful moderation and staff management bot, designed for Emergency Response: Liberty County & Maple County Communities. By self-hosting, you can customise ERM to your own liking, add new features, and contribute to its development!

This document will walk you through the steps needed to run ERM on your own machine, including:

    Preparing your environment
    Installing dependencies
    Configuring the bot
    Running and troubleshooting

Contents:

    Self-Hosting on Windows
        Prerequisites
        Installing packages and dependencies
        Environment Configuration(.env Setup)
        Final Bot Setup and Running
    Self-Hosting on Ubtunu/Debian-based systems
        Prerequisites
        Installing Packages and Dependencies
        Environment Configuration (.env Setup)
        Final Bot Setup and Running


DISCLAIMER: ERM is licensed under the Attribution-NonCommercial-ShareAlike (CC BY-NC-SA) license. This license allows for the copy, distribution, and creation of adaptations of the material for non-commercial purposes, as long as proper attribution is given to the original creator and any adaptations are licensed under the same terms.

The CC BY-NC-SA license requires the following elements:

    BY: Credit must be given to the original creator
    NCL The material can only be used for non-commercial purposes
    SA: Adaptations must be licensed under the same terms

This means that your self-hosted bot must give credit to the original creators, you must not sell it, charge money for its use, include it in paid services, or make a profit from it, and any adaptations must also be licensed under the Attribution-NonCommercial-ShareAlike (CC BY-NC-SA) license.
Windows Self-Host Guide

This section will explain how to self-host ERM on Windows 10/11. May or may not work on Windows server, or other distributions.
Prerequisites

You will need these before your bot can go online:

    The ERM Repository
    Windows Terminal or Command Prompt (PowerShell also works)
    Python version 3.10+ for Windows
    Python3 pip (Included with Python 3.4+)
    pynacl (Optional, use if you want voice features)
    Git for Windows
    MongoDB Atlas URI
    Discord Bot Token


Installing Packages and Dependencies

You will need to download some packages for this to work, so let's walk through them.
Python 3.10+

First, you will need to Download Python. Run the installer, and make sure to check Add Python.exe to PATH, or else this won't work. Then, press Install now and wait for the install to finish. (You can customise your installation, but you will need to download pip & venv module.)
Git

You will also need to download Git. After downloading the latest version, follow the setup instructions. This will give you Git Bash, but you can use command prompt or powershell.
Cloning ERM

To download the ERM repository, you can either:

    Download the ZIP from GitHub and extract it.
    Or use Git:

To use git, open command prompt (Win + R, type in cmd and press enter) and run the following:

git clone https://github.com/mikeyerm/ERM.git

Once you have those installed, we now need to make a venv, and install our packages.

Setting up the Virtual Environment

Navigate to your ERM folder by doing the following:

cd C: \path\to\ERM-main

Then, run these next two commands:

python -m venv venv 
venv\Scripts\activate

If your command prompt says (venv), you have successfully set up the virtual environment.
Installing Requirements

Upgrade pip & tools with the following:

pip install --upgrade pip setuptools wheel

Then, you can install dependencies with this command:

pip install -r requirements.txt

Environment Configuration (.env Setup)

Congrats on making it past that stage! Now, your .env file must be configured to include things such as your Discord Bot Token, MongoDB Atlas URI, API keys, and more. Your .env file should be in your ERM-main folder, but might be called .env.template. Open the .env file in a code editor, such as Visual Studio Code, and edit the text. The following steps will explained how to fill out the .env file properly.
Required for bot to function:

    MongoDB Atlas URI
    Discord Bot Token
    Environment Type
    Custom Guild ID
    Panel API URL
    Sentry URL (Not required for bot to run, but recommended for error logging)

MongoDB Atlas URI

To get your MONGO_URL, you need to make a free MongoDB Atlas URI.
To do so, you need to make an account with MongoDB — it's free, and doesn't need a credit card. You can sign in with GitHub or Google, or just use your email. Once you have completed the sign-up process, you need to create a cluster. You should be able to do this by pressing the "Create Cluster" button. The Name, Provider and Region do not matter, simply press "Create Deployment" and wait for it to finish creating the cluster. You should have gotten a popup asking you to connect to your cluster, and to make a database user, just press "create database user". To connect to your application, press the "Drivers" option, and copy the connection string.
Make sure to store this somewhere you will be able to access it from again.
Once done, go to "Network Access" on the sidebar, press "Add IP Address", click "Allow access from anywhere", and hit "Confirm". Paste the connecton string into the .env file after MONGO_URL=.
That should look like so, with your username, password and cluster name:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority

Environment Type & Discord Bot Token

Next, we will change the Environment Type to Custom. Your ENVIRONMENT in your .env file should look like so:

ENVIRONMENT=CUSTOM


To get your discord Bot Token, you need to create an application in the Discord Developer Portal. Once you have a bot token, you need to add another line into your .env file. Since our environment type is CUSTOM, we can't use PRODUCTION_BOT_TOKEN or DEVELOPMENT_BOT_TOKEN
Add a line anywhere, and type CUSTOM_BOT_TOKEN=. Paste your bot token after that.

So far, your .env file should look like this:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL= 
PRODUCTION_BOT_TOKEN= 
DEVELOPMENT_BOT_TOKEN= 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
BLOXLINK_API_KEY=


Sentry Logging and Bloxlink API

The following are not required, but some features might not work without a bloxlink API key.

You do not need to fill out PRODUCTION_BOT_TOKEN or DEVELOPMENT_BOT_TOKEN if you have ENVIRONMENT=CUSTOM and CUSTOM_BOT_TOKEN. Unless you have a bloxlink API Key (which you can get from here), you do not need to fill it out. If you want error logging with Sentry, sign up here, Install Sentry, select Python(vanilla) as the platform you want to monitor, press configure SDK (sentry-sdk has already been installed, you do not need to install it with pip again), and copy the URL in dsn="yoursentrysdkurl". You need to edit SENTRY_URL= to SENTRY_BASE_URL, and then you can paste it into your .env file.

Your .env file should be looking like this:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL=yoursentrysdkurl 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
BLOXLINK_API_KEY=yourbloxlinkapikey

Custom Guild ID and Panel API URL

The following fields need to be added to your .env file for your bot to be able to function.

CUSTOM_GUILD_ID=yourserverid 
PANEL_API_URL=[leave blank]

Your .env file should look like this now:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL=your://sentry.io.url/ 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
CUSTOM_GUILD_ID=yourserverid 
BLOXLINK_API_KEY=yourbloxlinkapikey 
PANEL_API_URL= [leave blank]

You can now save your .env file, and move onto the next step. That is everything that is required for your bot to host, everything else is just external APIs for er:lc, maple county, google sheets, etc.
Note: Make sure your file is saved as .env, not .env.template or something like .env.txt.

At the time of writing, the ERM repository is missing utils/advanced.py. The bot will not run without this file. Thankfully, we can replicate the contents of the file pretty easily.
Making missing utils/advanced.py file

Make a new file inside of the utils folder, and title it advanced.py. Then, copy and paste the following into it:


import discord

class FakeMessage:
    def __init__(self, content, author, channel, state):
        self.content = content
        self.author = author  
        self.channel = channel 
        self.guild = author.guild if hasattr(author, 'guild') else None
        self.created_at = discord.utils.utcnow()
        self._state = state
        self.attachments = []
        self.mentions = []
        self.mention_everyone = False
        self.role_mentions = []
        

Once you have completed all previous steps, it is time to run your bot. This has to be done inside of the Virtual Environment we created — if you are no longer in that terminal, run the following commands:

cd ~/path/to/ERM-main
python3 -m venv venv 
source venv/bin/activate 

If your terminal has changed from this format:

username@computername:~$

to this:

(venv) username@computername:~/path/to/ERM-main$ 

You should be able to run the following command, which should start up your bot:

python3 main.py

Well done! Your bot should now be running! If you close your terminal, or turn off your computer, your bot will stop running. If you want to safely stop the bot from running, press Ctrl + C inside of the terminal.
Ubuntu/Debian Self-Host Guide

This section will explain how to self-host ERM on Debian/Ubuntu-based systems, which includes Linux Mint, Pop!_OS, Zorin OS, Lubuntu, Xubuntu, and any other distribution that uses the apt package manager.
Prerequisites

You will need these before your bot can go online:

    The ERM Repository
    A terminal that supports Bash (such as GNOME Terminal, Xfce Terminal, etc.)
    Python version 3.10+
    Python3 pip (Python 3.4+ has pip enabled by default.)
    Python venv module (Optional, but you will need it if you are depending on this guide to know how to self-host ERM.)
    pynacl (Optional, only use if you want voice features)
    Git
    MongoDB Atlas URI
    Discord Bot Token


Installing Packages and Dependencies

Note: All packages are necessary for the entire bot to function, unless you are manually editing files to remove modules. These instructions rely on using setuptools wheel to read requirements.txt and install all packages required. This is the recommended method, unless you are using another method.

ERM Repository

You can install the ERM repository by going here, pressing the green "code" button, and pressing "Download ZIP". Once done, extract the folder to somewhere you can store it, such as a folder on your Desktop, or straight on your desktop.

Python, pip & venv
Next, you will need a few python packages to run the python files. To check if you have python & pip installed, run:

python3 --version 

pip --version 


ERM runs on python 3.10, so make sure your python version is 3.10 or later. To check if you have venv installed, you need to run:

python3 -m venv --help


If you do not have the required files, you can install all three of them in one command by running:

sudo apt update 
sudo apt install python3-venv python3-full -y

System Dependencies

Once you have those installed, you will need to install system dependencies for Levenshtein & pycryptodome.
To install those system dependencies, run:

sudo apt install build-essential libffi-dev python3-dev libssl-dev libxml2-dev libxslt1-dev zlib1g-dev libjpeg-dev -y

This will install necessary system dependencies for Levenshtein and pycryptodome.
Git

Next step is to install Git. You can install Git by running the following commands:

sudo apt update
sudo apt install git -y

Virtual Environment

The next few commands involve setting up a virtual environment, which is best practice if you don't want to break your system. Replacing ~path/to/ERM-main with the actual directory to where you put ERM-main, and with ~ representing your home directory(/home/username/), run the following command:

cd ~/path/to/ERM-main

Your terminal should now include the location of your folder, and look something like this:

username@computer-name:~/path/to/ERM-main$

If it does, well done! If it doesn't, either try again or read the error message. Then, run these next 2 commands:

python3 -m venv venv

source venv/bin/activate

Your terminal should now have (venv) at the start, looking like this:

(venv) username@computer-name:~/path/to/ERM-main$

Installing Requirements.txt

Next, we will use pip/setuptools/wheel to install all of our required packages from requirements.txt!
Run this command inside of your venv to install setuptools/wheel:

pip install --upgrade pip setuptools wheel

And finally, to download all of our packages:

pip install -r requirements.txt

If you made your venv outside of the ERM-main folder, you may have to run pip install -r /ERM-main/requirements.txt instead.
Environment Configuration (.env Setup)

Congrats on making it past that stage! Now, your .env file must be configured to include things such as your Discord Bot Token, MongoDB Atlas URI, API keys, and more. Your .env file should be in your ERM-main folder, but might be hidden from your file manager. To view the .env file, you can open the ERM-main folder in a code editor, such as Visual Studio Code, and edit the text. The following steps will be explained to fill out the .env file.
Required for bot to function:

    MongoDB Atlas URI
    Discord Bot Token
    Environment Type
    Custom Guild ID
    Panel API URL
    Sentry URL (Not required for bot to run, but recommended for error logging)

MongoDB Atlas URI

To get your MONGO_URL, you need to make a free MongoDB Atlas URI.
To do so, you need to make an account with MongoDB — it's free, and doesn't need a credit card. You can sign in with GitHub or Google, or just use your email. Once you have completed the sign-up process, you need to create a cluster. You should be able to do this by pressing the "Create Cluster" button. The Name, Provider and Region do not matter, simply press "Create Deployment" and wait for it to finish creating the cluster. You should have gotten a popup asking you to connect to your cluster, and to make a database user, just press "create database user". To connect to your application, press the "Drivers" option, and copy the connection string.
Make sure to store this somewhere you will be able to access it from again.
Once done, go to "Network Access" on the sidebar, press "Add IP Address", click "Allow access from anywhere", and hit "Confirm". Paste the connecton string into the .env file after MONGO_URL=.
That should look like so, with your username, password and cluster name:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority

Environment Type & Discord Bot Token

Next, we will change the Environment Type to Custom. Your ENVIRONMENT in your .env file should look like so:

ENVIRONMENT=CUSTOM


To get your discord Bot Token, you need to create an application in the Discord Developer Portal. Once you have a bot token, you need to add another line into your .env file. Since our environment type is CUSTOM, we can't use PRODUCTION_BOT_TOKEN or DEVELOPMENT_BOT_TOKEN
Add a line anywhere, and type CUSTOM_BOT_TOKEN=. Paste your bot token after that.

So far, your .env file should look like this:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL= 
PRODUCTION_BOT_TOKEN= 
DEVELOPMENT_BOT_TOKEN= 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
BLOXLINK_API_KEY=


Sentry Logging and Bloxlink API

The following are not required, but some features might not work without a bloxlink API key.

You do not need to fill out PRODUCTION_BOT_TOKEN or DEVELOPMENT_BOT_TOKEN if you have ENVIRONMENT=CUSTOM and CUSTOM_BOT_TOKEN. Unless you have a bloxlink API Key (which you can get from here), you do not need to fill it out. If you want error logging with Sentry, sign up here, Install Sentry, select Python(vanilla) as the platform you want to monitor, press configure SDK (sentry-sdk has already been installed, you do not need to install it with pip again), and copy the URL in dsn="yoursentrysdkurl". You need to edit SENTRY_URL= to SENTRY_BASE_URL, and then you can paste it into your .env file.

Your .env file should be looking like this:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL=yoursentrysdkurl 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
BLOXLINK_API_KEY=yourbloxlinkapikey

Custom Guild ID and Panel API URL

The following fields need to be added to your .env file for your bot to be able to function.

CUSTOM_GUILD_ID=yourserverid 
PANEL_API_URL=[leave blank]

Your .env file should look like this now:

MONGO_URL=mongodb+srv://username:password@cluster-name.mongodb.net/?retryWrites=true&w=majority  
ENVIRONMENT=CUSTOM 
SENTRY_URL=your://sentry.io.url/ 
CUSTOM_BOT_TOKEN=yourdiscordbottoken 
CUSTOM_GUILD_ID=yourserverid 
BLOXLINK_API_KEY=yourbloxlinkapikey 
PANEL_API_URL= [leave blank]

You can now save your .env file, and move onto the next step. That is everything that is required for your bot to host, everything else is just external APIs for er:lc, maple county, google sheets, etc.
Note: Make sure your file is saved as .env, not .env.template or something like .env.txt.
Final Bot Setup and Running

Once you have completed all previous steps, it is time to run your bot. This has to be done inside of the Virtual Environment we created — if you are no longer in that terminal, run the following commands:

cd ~/path/to/ERM-main
python3 -m venv venv 
source venv/bin/activate 

If your terminal has changed from this format:

username@computername:~$

to this:

(venv) username@computername:~/path/to/ERM-main$ 

You should be able to run the following command, which should start up your bot:

python3 main.py

Well done! Your bot should now be running! If you close your terminal, or turn off your computer, your bot will stop running. If you want to safely stop the bot from running, press Ctrl + C inside of the terminal. 
