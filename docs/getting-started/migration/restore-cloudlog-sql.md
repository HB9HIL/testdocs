# Restore Cloudlog from an SQL Dump

If you have a full dump of your database, you can restore it this way:

1. Install Wavelog and set it up.
2. Drop all tables in the database.
3. Import your dump so that all tables are restored.
4. The migration system should now update your database with all necessary changes in case some changes has happened after your database dump.