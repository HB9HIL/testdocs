# Change Log

Change Log reflects changes which might be of interest or will require you to make changes to an active install of Wavelog

# Before Updating
**Always take a backup** before updating Wavelog, this is just best practices, if you use a bash script to update Wavelog remember to take periodic backups. That said Wavelog is generally fixable with some knowledge of MySQL.

## Important - Station Profiles

I recently made what some might class as a breaking change to Wavelog, this will only affect upgraded installs, where you will need to create a station profile then assign QSOs to it, this should work by going to the Station Profiles area, and pressing the buttons however if you only have 1 profile and can't get qsos to assign run `index.php/station/assign_all`

### Ongoing notes
Date: 25/02/2019
* Frequency changed to bigint (https://github.com/wavelog/Wavelog/pull/248) (if your using an old install you need to adjust your database)
* Station Profiles tables missing ID and primary key (https://github.com/wavelog/Wavelog/pull/251)
* Changed database to **UTF8mb4**
* Fixed API Test button [https://github.com/wavelog/Wavelog/pull/254](https://github.com/wavelog/Wavelog/pull/254)

Date: 16/06/2019

* I've added a feature to look up LoTW active users which is then displayed when logging a contact which is helpful for awards, to import the csv from LoTW you'll need to let PHP execute for much longer periods and use the "index.php/lotw/load_users" url within Clodulog to import this will take a long time.

Date: 20/06/2019

* You can now batch upload QSOs to Clublog for more information see the wiki page [[Clublog Upload]]

Date: 17/06/2019

* DXCC Awards is now only shows bands you have had QSOs on.
* API for QSO creation added this accepts ADIF string via json - [Find out how to use the QSO API](https://github.com/wavelog/Wavelog/wiki/API)

Date: 23/06/2019

* There is now a Wavelog config file `application/config/wavelog.php` this will now be the file where anything todo with configuration of Wavelog will be stored going forward.
* By default times are now not shown to anyone who isn't logged in, this can be changed by going to `application/config/wavelog.php`

Date: 24/06/2019

* You can now set measurement to "Miles" or "Km" within the config/wavelog.php which is probably prefered for European users.

## Missing Fields in the log table

I recently added the missing ADIF fields to the schema if you run an old version of Wavelog you might need to add these yourself.

```SQL
ALTER TABLE `TABLE_HRD_CONTACTS_V01`  ADD `COL_ADDRESS_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_CREDIT_SUBMITTED`,  ADD `COL_AWARD_GRANTED` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_ADDRESS_INTL`,  ADD `COL_AWARD_SUMMITED` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_AWARD_GRANTED`,  ADD `COL_CLUBLOG_QSO_UPLOAD_DATE` DATETIME NULL DEFAULT NULL  AFTER `COL_AWARD_SUMMITED`,  ADD `COL_CLUBLOG_QSO_UPLOAD_STATUS` VARCHAR(20) NULL DEFAULT NULL  AFTER `COL_CLUBLOG_QSO_UPLOAD_DATE`,  ADD `COL_COMMENT_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_CLUBLOG_QSO_UPLOAD_STATUS`,  ADD `COL_COUNTRY_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_COMMENT_INTL`,  ADD `COL_SILENT_KEY` VARCHAR(2) NULL DEFAULT NULL  AFTER `COL_COUNTRY_INTL`,  ADD `COL_SKCC` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_SILENT_KEY`,  ADD `COL_DARC_DOK` VARCHAR(10) NULL DEFAULT NULL  AFTER `COL_SKCC`,  ADD `COL_FISTS` INT(10) NULL DEFAULT NULL  AFTER `COL_DARC_DOK`,  ADD `COL_FISTS_CC` INT(10) NULL DEFAULT NULL  AFTER `COL_FISTS`,  ADD `COL_HRDLOG_QSO_UPLOAD_DATE` DATETIME NULL DEFAULT NULL  AFTER `COL_FISTS_CC`,  ADD `COL_HRDLOG_QSO_UPLOAD_STATUS` VARCHAR(10) NULL DEFAULT NULL  AFTER `COL_HRDLOG_QSO_UPLOAD_DATE`,  ADD `COL_MY_ANTENNA` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_HRDLOG_QSO_UPLOAD_STATUS`,  ADD `COL_MY_ANTENNA_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_ANTENNA`,  ADD `COL_MY_CITY_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_ANTENNA_INTL`,  ADD `COL_MY_COUNTRY_INTL` INT(6) NULL DEFAULT NULL  AFTER `COL_MY_CITY_INTL`,  ADD `COL_MY_DXCC` INT(6) NULL DEFAULT NULL  AFTER `COL_MY_COUNTRY_INTL`,  ADD `COL_MY_FISTS` INT(10) NULL DEFAULT NULL  AFTER `COL_MY_DXCC`,  ADD `COL_MY_IOTA_ISLAND_ID` VARCHAR(10) NULL DEFAULT NULL  AFTER `COL_MY_FISTS`,  ADD `COL_MY_NAME_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_IOTA_ISLAND_ID`,  ADD `COL_MY_POSTCODE_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_NAME_INTL`,  ADD `COL_MY_RIG_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_POSTCODE_INTL`,  ADD `COL_MY_SIG_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_RIG_INTL`,  ADD `COL_MY_SIG_INFO_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_SIG_INTL`,  ADD `COL_MY_SOTA_REF` VARCHAR(50) NULL DEFAULT NULL  AFTER `COL_MY_SIG_INFO_INTL`,  ADD `COL_MY_STREET_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_SOTA_REF`,  ADD `COL_MY_USACA_COUNTIES` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_STREET_INTL`,  ADD `COL_MY_VUCC_GRIDS` VARCHAR(50) NULL DEFAULT NULL  AFTER `COL_MY_USACA_COUNTIES`,  ADD `COL_NAME_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_MY_VUCC_GRIDS`,  ADD `COL_NOTES_INTL` LONGTEXT NULL DEFAULT NULL  AFTER `COL_NAME_INTL`,  ADD `COL_QRZCOM_QSO_UPLOAD_DATE` DATETIME NULL DEFAULT NULL  AFTER `COL_NOTES_INTL`,  ADD `COL_QRZCOM_QSO_UPLOAD_STATUS` VARCHAR(10) NULL DEFAULT NULL  AFTER `COL_QRZCOM_QSO_UPLOAD_DATE`,  ADD `COL_QSO_DATE` DATE NULL DEFAULT NULL  AFTER `COL_QRZCOM_QSO_UPLOAD_STATUS`,  ADD `COL_QSO_DATE_OFF` DATE NULL DEFAULT NULL  AFTER `COL_QSO_DATE`,  ADD `COL_QTH_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_QSO_DATE_OFF`,  ADD `COL_REGION` VARCHAR(25) NULL DEFAULT NULL  AFTER `COL_QTH_INTL`,  ADD `COL_RIG_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_REGION`,  ADD `COL_SIG_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_RIG_INTL`,  ADD `COL_SIG_INFO_INTL` VARCHAR(255) NULL DEFAULT NULL  AFTER `COL_SIG_INTL`;

ALTER TABLE `TABLE_HRD_CONTACTS_V01` ADD `COL_SOTA_REF` VARCHAR(30) NULL DEFAULT NULL AFTER `COL_SIG_INFO_INTL`, ADD `COL_SUBMODE` VARCHAR(25) NULL DEFAULT NULL AFTER `COL_SOTA_REF`, ADD `COL_UKSMG` VARCHAR(64) NULL DEFAULT NULL AFTER `COL_SUBMODE`, ADD `COL_USACA_COUNTIES` VARCHAR(255) NULL DEFAULT NULL AFTER `COL_UKSMG`, ADD `COL_VUCC_GRIDS` VARCHAR(255) NULL DEFAULT NULL AFTER `COL_USACA_COUNTIES`;
```

## MySQL Strange

If you find you get weird SQL errors then you might need to set the following on your MySQL setup `SET @@global.sql_mode= 'NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';` which seems to resolve the issues.

If you get an error about dates with 00 in them you'll need to run the following command on MySQL `SET SQL_MODE='ALLOW_INVALID_DATES';`