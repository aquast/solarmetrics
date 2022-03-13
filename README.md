# Getting metrics from the modbus interface of the EPever solar charger

## Abstract
This document gives some instructions for setting up an web based interface to the EPever solar charger XTRA and TRACER series. It schould be easily adaptably to other solar chargers if they providing a modbus interface. MODBUS RJ45 to USB adapter also can be replaced with other connection solutions to the Raspberry. The telegraf software which is in task for collecting the data from the charger and send it to the influx data base has a widh range of input module configurations, with well documented examples.

One aim for providing this description for setting up a replacement for the EPever Hardware solutions, is to prevent you from buying the some of the EPever's additional hardware  like the eBox-WIFI-01 or eBox-BLE-01. In contrast to the solar charger, which is operating well, the eBoxes are both much to expensive for their cheapest interieur and also a reason for great security concerns.

So let's start

## Prerequisites

 1. Raspberry 3 or Raspberry 4
 2. RaspianOS
 3. Modbus Specification for your Solar Charger (if not EPever XTRA, TRACER Series
 4. MODBUS RJ45 to USB Adapter (likewise CC-USB-RS485-150U PC Communication Cable)
 5. Specification for the MODBUS Adapter
 
### Software for to access, store and provide the metrics:
 
 1. InfluxDB
 2. Telegraf
 3. Grafana


## Installation

InfluxDB and telegraf are both open source software under MIT license. The software is released and maintained by influxdata. Both, telegraf and influxDB can be acquired and updated by adding the influx repository to your Raspberry repository manager.

    export DISTRIB_ID=$(lsb_release -si); export DISTRIB_CODENAME=$(lsb_release -sc)
    curl -s https://repos.influxdata.com/influxdb.key | gpg --dearmor > /etc/apt/trusted.gpg.d/influxdb.gpg
    echo "deb [signed-by=/etc/apt/trusted.gpg.d/influxdb.gpg] https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" > /etc/apt/sources.list.d/influxdb.list

Now telegraf and influxDB shuold be installable and missing dependency should be fixed automatically.

    sudo apt update
    sudo apt install telegraf
    sudo apt install influxdb

## Configuration

Start and enable influxdb
    sudo systemctl enable influxdb
    sudo systemctl start influxdb

Create new database and user in influxdb and grant manipulation to new user
    sudo su
    influx
    CREATE DATABASE pvmetrics;
    CREATE USER "pvmetrics" WITH PASSWORD '********' WITH ALL PRIVILEGES;
    exit

Start and enable telegraf

    sudo systemctl enable telegraf
    sudo systemctl start telegraf

<u>IMPORTANT</u>

telegraf runs on its own user telegraf. ;-)  After installation the telegraf user has not sufficiant rights for starting telegraf on system boot. Therefore you HAVE TO put the telegraf user into group dialout via

    adduser telegraf dialout





