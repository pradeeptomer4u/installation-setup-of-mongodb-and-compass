# installation-setup-of-mongodb-and-compass
installation/setup of mongodb and compass

# Mongodb Compass

wget https://downloads.mongodb.com/compass/mongodb-compass_1.14.1_amd64.deb;

sudo dpkg -i mongodb-compass_1.14.1_amd64.deb;

# install .deb package for visual code

sudo dpkg -i code_1.29.0-1542008808_amd64.deb

sudo apt install libgconf-2-4

apt --fix-broken install

sudo apt --fix-broken install

sudo dpkg -i mongodb-compass_1.14.1_amd64.deb;

mongodb-compass 

sudo apt update

# Mongodb Server

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 4B7C549A058F8B6B

echo "deb http://repo.mongodb.org/apt/debian "$(lsb_release -sc)"/mongodb-org/4.2 main" | sudo tee /etc/apt/sources.list.d/mongodb.list

sudo apt update

sudo apt install mongodb-org

sudo systemctl start mongod.service

sudo systemctl enable mongod.service

sudo systemctl status mongod.service

# copy database backup from ws s3 and Restore database of Mongodb

USERNAME="XXXXXXXX"

PASSWORD="XXXXXXXXXXXXXXXXX"

BASE=~/Downloads

cd $BASE

rm -rf  database_folder

aws s3 cp s3://backups/$1.tar.gz database_folder.tar.gz

mkdir database_folder

tar -xzf database_folder.tar.gz

cd $BASE/backup/$1

mongorestore --port 27017 -u $USERNAME -p $PASSWORD --db database database_folder/ --batchSize=30

# or

sudo mongorestore --gzip --archive=/var/backups/mongodb/database_`date -d"$1 days ago" +%Y-%m-%d`.gz --port 27017 

# Taking backup of Mongodb Database and send to aws s3

#!/bin/bash

sudo mongodmp --port 32343 --db database --gzip --archive=/var/backups/mongobackups/database_`date -d"$1 days ago" +%Y-%m-%d"`.gz

aws s3 cp "/var/backups/mongobackups/database_`date "+%Y-%m-%d"`.gz" s3://backups/

aws s3 cp s3://backups/database_`date -d"$1 days ago" +%Y-%m-%d`.gz /var/backups/mongodb/

# create mongo User#####

use test

db.createUser({user: "admin",pwd: "admin123",roles: [ "readWrite", "dbAdmin" ] });

# Troubleshooting with mongodb start failed

#################### create start.sh file ##################

#!/bin/bash

sudo mongod --fork --auth --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log

#########################end #####################

sudo systemctl status mongod

sudo nano /etc/systemd/system/mongodb.service

sudo rm /var/lib/mongodb/mongod.lock

sudo mongod --repair

sudo service mongod start

sudo service mongod status

sudo chmod -R 0777 /var/lib/mongodb

sudo service mongod status

sudo service mongod start

sudo service mongod status

