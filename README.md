# AirBNB-Hacker

## Python Script to Find Last-minute Deals on Airbnb:
To find the best deals on Airbnb, you need to check the listings and you need to do it often. This repetitive process can be automated. This script is essentially an Airbnb a notification service as AirBNB does not have one. 

The script runs on a compute instance on Google Cloud. It periodically scrapes all listings for a particular query and sends you an email if a new one has been found. 

## Overview and How to Edit Script:

### Step 1: You want to create a separate Gmail account for this and turn ON less secure apps. See https://support.google.com/accounts/answer/6010255?hl=en#zippy=%2Cif-less-secure-app-access-is-off-for-your-account for more info

### Step 2: Find what you want on the site and grab the URL
First, you have to narrow down what you want. Let’s say our main goal is to find the best & most affordable place possible. For NYC, all the good places under $2k/mo are usually booked well in advance, but people do cancel and new listings come and go almost immediately due to very high demand. Also listers may offer discounts. So you go on Airbnb and narrow down your query and now you have something that looks like this:

![alt text](https://i.insider.com/5e471a0e3b62b76bed4fe3a2)

Now you copy the URL. This is where the new listing could appear and hence what we will be searching and data mining, periodically, once every three hours.

### Step 3: Edit Script with Your Details 
#### Step 3.1: Edit line 1 with your search URL in the AirBNB_Hacker.py as shown below:
~~~
url = "your url here"
~~~
#### Step 3.2: Edit line 42 to line 44 with your email details in the AirBNB_Hacker.py as shown below:
~~~
sender_email = "your sender gmail"
receiver_email = "email of whom you want to send it to"
password = "password for sender_email"
~~~

## How to run it in the cloud
To automate things you have to put this script on a compute instance on google cloud. To get started, get a free account on google cloud (https://cloud.google.com/free/?gclid=Cj0KCQiA15yNBhDTARIsAGnwe0VIXDyF0Sv_1DI9I_ck1V1oWln5ZL6aog4b2_DOyBdmWsPxweLkDWoaAvtfEALw_wcB&gclsrc=aw.ds) and follow these steps:

  Leave everything default as it was except for these settings:
  Go to compute engine, select create an instance.
  Select server location/region.
  The minimum hardware specs: e2micro machine type.
  Allow HTTP & HTTPS traffic.
  Go to the management dropdown, change premptibility to “on”. Smash the create button.

  Once the new machine is up and running, you’ll want to SSH into it. See below:
  
![alt text](https://miro.medium.com/max/700/1*pnUgZLXGCP03-U9E9ZocPg.png)

  Install Python and Miniconda with these commands in the terminal:
  ~~~
  curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  bash miniconda3-latest-Linux-x86_64.sh
  ~~~
  Next, you’ll want to upload the python script file (AirBNB_Hacker.py) after SSHing into the terminal. 
  ![image](https://user-images.githubusercontent.com/55500076/144329725-d67d17bd-792c-4b60-a858-6799692c0991.png)

  Also, pip install all these libraries by running below code: (Only needs to be ran once)
  ~~~
  !pip install time
  !pip install json
  !pip install smtplib
  !pip install ssl
  !pip install os
  !pip install selenium # Selenium is our web scraper. 
  !pip install email
  !pip install webdriver_manager.chrome 
  ~~~

  To Test everything, make sure it works by running
  ~~~
  python AirBNB_Hacker.py
  ~~~

  Now we can use crontab to schedule this script to be run once every 3 hours.
  ~~~
  crontab -e
  ~~~
  
  Add this to the first line in the crontab file you opened with the previous command:
  ~~~
  0 */3 * * * yourpythondirectory AirBNB_Hacker.py >> cronlog.log 2>&1
  ~~~
  
This code runs script.py once every 3 hours and outputs the output into cronlog.log. Replace yourpythondirectory with the location of your Python interpreter which can be found with “which python”. You can later check the output of cronlog with “cat cronlog.log” and the logs for the actual cronjob with “grep CRON /var/log/syslog”. If it’s not working be sure to check these two places.

Lastly, you might want to receive notifications from your Email app on your phone. It helped me alot.

