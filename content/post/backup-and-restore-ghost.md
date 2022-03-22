+++
author = "Johannes Pirmann"
categories = ["ghost", "infrastructure", "server", "linux"]
date = 2021-08-27T14:59:31Z
description = ""
draft = false
image = "__GHOST_URL__/content/images/2021/08/ghost.jpeg"
slug = "backup-and-restore-ghost"
title = "How to backup and restore Ghost"

+++


In this article I will describe how to create a backup of the Ghost database and directory, back it up and restore it.

## Prerequisites:

* Ghost running on a VPS
* Google Drive, or similar

# Creating the Backup

## Step 1 - Backup the database

In this step we will create a copy of the database and the directory.

### Step 1.1 - Create a mysql backup user

Within your server login to mysql:

```shell
mysql -u root -p
```

Now within mysql we are going to create a user dedicated to backups only. For this run the following command and replace the <password> with a strong password:

```shell
mysql> CREATE USER 'backup'@'localhost' IDENTIFIED BY '<password>';
```

Now the user also needs the correct privileges to create a backup:

```shell
mysql> GRANT ALL ON your_ghost_database.* TO 'backup'@'localhost';
mysql> FLUSH PRIVILEGES;
```

> If you're not sure how your database is named you can also look it up in the config.production.json. You can find it in the root directory of your ghost installation.

You can now exit mysql:

```shell
mysql> quit
```

### Step 1.2 - Create a mysql config file

We are going to create a mysql config file to not have to retype the password every time we execute a mysql command.

Create the file with your editor (nano/vim):

```shell
vim ~/.my.cnf
```

And add the following content to it:

```shell
[client]
user=backup
password='<password>'
```

Save and close the file and adapt the privileges of it:

```shell
chmod 600 ~/.my.cnf
```

### Step 1.3 - Create a backup

You should now be able to execute the following command:

```shell
mysqldump your_ghost_database > your_ghost_database.sql
```

Within your current directory you should now have the your_ghost_database.sql file.

## Step 2 - Backup your content

The following command will create a compressed archive of your content:

```shell
tar -zvcf ./content.tar.gz /var/www/ghost/content
```

* cf: Archive something
* v: verbosely list files processed
* z: filter the archive through gzip

## Step 3 - Save Backup in Google Drive

For this we are using rclone.

```shell
sudo apt-get intall rclone
```

Configure rclone with the following command. [Andy Ibanez](https://www.andyibanez.com/) has a good [tutorial](https://www.andyibanez.com/posts/rclone-basics-encryption/) on how to use rclone.

```shell
rclone config
```

## Step 4 - Put it together

Now we are going to create a script **~/Backups/backup.sh** which will execute all those steps for us.

```shell
#!/bin/bash

current_date=$(date +'%Y-%m-%d_%H-%M')
echo "Starting backup of ghost instances!"

echo "Creating backup folder for $current_date"
mkdir "/home/jp/Backups/ghost/$current_date"

echo "Dumping database"
mysqldump your_ghost_database | gzip > "/home/jp/Backups/ghost/$current_date/your_ghost_database.sql.gz"

echo "Compressing content folder"
tar -zvcf "/home/jp/Backups/ghost/$current_date/content.tar.gz" --absolute-names /var/www/ghost/content > /dev/null

echo "Saving to Google Drive"
/usr/bin/rclone copy --update --verbose --transfers 30 --checkers 8 --contimeout 60s --timeout 300s --retries 3 --low-level-retries 10 --stats 1s "/home/jp/Backups/ghost/$current_date/" "google-drive:Server_Backups/ghost/$current_date/"

echo "Finished Backup!"
```

## Step 5 - Cronjob

Add a cronjob:

```shell
crontab -e
```

Add the following line:

```shell
0 0 * * * /home/jp/Backups/backup.sh
```



---

# Restoring the backup

## Step 1 - Download data

```shell
rclone copy google-drive:/Server_Backups/ ~/
```

## Step 2 - Extract the files

```shell
gzip -d ./Server_Backups/ghost/$date/your_ghost_database.sql.gz
gzip -d  ./Server_Backups/ghost/$date/content.tar.gz
tar -xf ./Server_Backups/ghost/$date/content.tar
```

## Step 3 - Replace the database

First, login to mysql and run the following commands:

```shell
mysql> DROP DATABASE your_ghost_database;
mysq> CREATE DATABASE your_ghost_database;
mysql> quit
```

Now import your uncompressed database:

```shell
mysql your_ghost_database < your_ghost_database.sql
```

## Step 4 - Replace the content

```shell
sudo rm -r /var/www/ghost/content
mv ~/Server_Backups/ghost/date/content /var/www/ghost/
```



# Conclusion

We just backed up our ghost instance and restored it from a backup. If something went wrong try running **ghost doctor** and checking the permissions.

Sources:* [https://dev.to/kvizdos/how-to-automatically-backup-ghost-blogs-4he1](https://dev.to/kvizdos/how-to-automatically-backup-ghost-blogs-4he1)
* [https://www.andyibanez.com/posts/rclone-basics-encryption/](https://www.andyibanez.com/posts/rclone-basics-encryption/)
* [https://crontab.guru/](https://crontab.guru/)

