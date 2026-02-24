# Raspberry Pi Image Information

> **IMPORTANT: YOU ARE RESPONSIBLE FOR PROTECTING THE RASPBERRY PI & DOING BACKUPS**

## Where to download the image from!
You can download the Raspberry Pi Image from [https://www.wavelog.co.uk/assets/2020-02-17-raspbian-buster-wavelog.zip](https://www.wavelog.co.uk/assets/2020-02-17-raspbian-buster-wavelog.zip)

## Basic Details
SSH is enabled so if you plug the ethernet into the Pi you should be able to access it with

* Username: `pi`
* Password: `raspberry`

Wifi hasn't been configured so you'll have to do that using `sudo raspi-config`

You should be able to access Wavelog by just going to [http://wavelog.local](http://wavelog.local) the default login is

* Username: `m0abc`
* Password: `demo`

You will need to create a new station profile via the admin system and follow the rest of the guide for setting up HamQTH or QRZ but this should get you going.

## Where to find Wavelog files
Wavelog is stored in `/home/pi/www/Wavelog/` you can find the most important files in `/home/pi/www/Wavelog/application/config` which you might need to change to fit your requirements.

## Updating Wavelog
While there is a cronjob to update Wavelog weekly you can manually do it by logging to the raspberry pi on the command line and running `./update_wavelog.sh` from within the `/home/pi` directory

## Cronjobs
By default, there are three cronjobs set up to run weekly these are for doing the following
* Wavelog Update
* LOTW Users
* Clublog SCP

You can run `crontab -e` on the command line to change these or add more.

## Database: MariaDB
This image uses MariaDB instead of MySQL if you'd like to login to MariaDB the information is as follows

* Username: `root`
* Password: `6B6@&9EeSG9M&XnRN2e76`

Wavelog uses a database called `wavelog`

