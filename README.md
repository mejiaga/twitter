# ConferenceTweetMapper
## General usage
This repository contains two Python scripts:
- stream.py

   This script allows to search Twitter for a particular conference hashtag and to return all the resulting tweets in a JSON file
   
- graphify.py

   This script takes a JSON file containing Tweets and transforms them to graph oriented object representing another view of the timeline of Tweets, Retweets, Quote, Hashtag, and Handles.
   
## Pre-requisites

In order to use those scripts you must have:
- Python 2.7 installed (only version tested so far)
- Pip installed
- Python packages installed:

   * tweepy
   * json
   * time
   * configparser
   * argparse
   * py2neo
   
- Neo4j Db installed, configured, and ready for connection

   I won't detail hee how to do this part, there are plenty of good tutorials on the Web

## stream.py
This script can be used as follow:

```
usage: stream.py [-h] {file,line} ...

Export tweets that match the search query

positional arguments:
  {file,line}  Add configuration from Ini file or through arguments
    file       Adding configuration from a file (Default: Ini/Default.ini)
    line       Adding configuration from a arguments in the command line

optional arguments:
  -h, --help   show this help message and exit
```

The file subcommand supports the following syntax:

```
usage: stream.py file [-h] [-i INI_FILE]

positional arguments:
  cmd

optional arguments:
  -h, --help            show this help message and exit
  -i INI_FILE, --ini_file INI_FILE
                        Path to the Ini file (Default: Ini/Default.ini)
```

The line subcommand supports the following syntax:

```
usage: stream.py line [-h] -s SEARCH -ck CONSUMER_KEY -cs CONSUMER_SECRET -ak
                      ACCESS_KEY -as ACCESS_SECRET -o OUTPUT_FILENAME
                      [-b BACKUP_INI_FILE_NAME]

positional arguments:
  cmd

optional arguments:
  -h, --help            show this help message and exit
  -s SEARCH, --search SEARCH
                        Twitter search filter
  -ck CONSUMER_KEY, --consumer_key CONSUMER_KEY
                        Twitter consumer key obtained from your Twitter account
  -cs CONSUMER_SECRET, --consumer_secret CONSUMER_SECRET
                        Twitter consumer secret obtained from your Twitter account
  -ak ACCESS_KEY, --access_key ACCESS_KEY
                        Twitter access key obtained from your Twitter account
  -as ACCESS_SECRET, --access_secret ACCESS_SECRET
                        Twitter access_secret obtained from your Twitter account
  -o OUTPUT_FILENAME, --output_filename OUTPUT_FILENAME
                        Name of the results output file
  -b BACKUP_INI_FILE_NAME, --backup_ini_file_name BACKUP_INI_FILE_NAME
                        Name of the Ini file to backup from this request parameters
```
 
## graphify.py
This script can be used as follow:

```
usage: graphify.py [-h] {file,line} ...

Import tweets in a Graph DB

positional arguments:
  {file,line}  Add configuration from Ini file or through arguments
    file       Adding configuration from a file (Default: Ini/Default.ini)
    line       Adding configuration from a arguments in the command line

optional arguments:
  -h, --help   show this help message and exit
```

The file subcommand supports the following syntax:

```
usage: graphify.py file [-h] [-i INI_FILE]

positional arguments:
  cmd

optional arguments:
  -h, --help            show this help message and exit
  -i INI_FILE, --ini_file INI_FILE
                        Path to the Ini file (Default: Ini/Default.ini)
```

The line subcommand supports the following syntax:

```
usage: graphify.py line [-h] [-type DB_TYPE] [-proto PROTOCOL]
                        [-lang LANGUAGE] [-server SERVER_NAME]
                        [-port SERVER_PORT] -pwd DB_PASSWORD [-set RESULT_SET]
                        -name CONFERENCE_NAME -loc CONFERENCE_LOCATION -time
                        CONFERENCE_TIME_ZONE -start CONFERENCE_START_DATE -end
                        CONFERENCE_END_DATE [-purge PURGE_BEFORE_IMPORT]
                        [-fname FILTER_ORGANIZER_TWITTER_SCREENAME]
                        [-fhash FILTER_CONFERENCE_HASHTAG]
                        [-b BACKUP_INI_FILE_NAME]

positional arguments:
  cmd

optional arguments:
  -h, --help            show this help message and exit
  -type DB_TYPE, --db_type DB_TYPE
                        For future use: indicate db type
  -proto PROTOCOL, --protocol PROTOCOL
                        For future use: indicate protocol to connect to db
  -lang LANGUAGE, --language LANGUAGE
                        For future use: indicate language to query the db
  -server SERVER_NAME, --server_name SERVER_NAME
                        FQDN of the db server
  -port SERVER_PORT, --server_port SERVER_PORT
                        server socket hosting the db service
  -pwd DB_PASSWORD, --db_password DB_PASSWORD
                        service password to access the db
  -set RESULT_SET, --result_set RESULT_SET
                        Result set file from streaming script (Default: Output/search.json)
  -name CONFERENCE_NAME, --conference_name CONFERENCE_NAME
                        Name of the conference for the master node
  -loc CONFERENCE_LOCATION, --conference_location CONFERENCE_LOCATION
                        Location of the conference for the master node
  -time CONFERENCE_TIME_ZONE, --conference_time_zone CONFERENCE_TIME_ZONE
                        Number of (+/-) hours from UTC reference of the conference's timezone
  -start CONFERENCE_START_DATE, --conference_start_date CONFERENCE_START_DATE
                        First day of the conference in dd/mm/yyyy format
  -end CONFERENCE_END_DATE, --conference_end_date CONFERENCE_END_DATE
                        Last day of the conference in dd/mm/yyyy format
  -purge PURGE_BEFORE_IMPORT, --purge_before_import PURGE_BEFORE_IMPORT
                        Indicate if the graph must be deleted before importing (Default: false)
  -fname FILTER_ORGANIZER_TWITTER_SCREENAME, --filter_organizer_twitter_screename FILTER_ORGANIZER_TWITTER_SCREENAME
                        Twitter screename that helps to filter out organizer tweets and retweets
  -fhash FILTER_CONFERENCE_HASHTAG, --filter_conference_hashtag FILTER_CONFERENCE_HASHTAG
                        Hashtag of the conference
  -b BACKUP_INI_FILE_NAME, --backup_ini_file_name BACKUP_INI_FILE_NAME
                        Name of the Ini file to backup from this request parameters
```

## Ini file example
In the Ini folder you should find a Default.ini file describing the format expected for a global Ini file:

```#Default initialization filter
#All dates shall be in format dd/mm/yyyy

[DEFAULT]
output_filename = Output/search.json
search = #Identiverse

[Twitter]
consumer_key = x7XOywj3rgsSKn9MXkBNBpecr
consumer_secret = o4lHJPQQyVGQaojTGPTHR9BOXuEzvRzbghBkPMrAyaBoPRQjvd
access_key = 3994061302-XFnBADeFR4WBxZzmXAy4sXn2sSHu6aYldjZgbVk
access_secret = g4FCmePHacsvv7nJOp2sJRaY9nbnH556s97whuOKILFrI

[Graph]
db_type = Neo4j
protocol = bolt
language = cypher
server_name = localhost
server_port = 7687
db_password = Identiverse

[Processing]
result_set = Output/search.json
conference_name = Identiverse 2018
conference_location = Boston
conference_time_zone = -4
conference_start_date = 24/06/2018
conference_end_date = 27/06/2018

[Misc]
purge_before_import = false
filter_organizer_twitter_screename = Identiverse
filter_conference_hashtag = Identiverse
```

## General limitations and advices
Using those scripts, you understand that:
- Having two scripts allows to separate the two operations independently
- Scripts do not check for file existence at the time of exporting (results and configuration), so be careful if you don't want one to be overwritten
- Twitter search public API will not return unindexed results, some results older than 7 days, or maybe all the results you may get by using the UI version of it
- stream.py search filter aims has been designed to target conference hashtag... but it is a standard Twitter search filter supporting all the options Twitter allows
- graphify.py does only support Neo4j, bolt protocol, and cipher language as for now

## Roadmap
- Better date management
- Change the Post and Pre conference period id to something speciifc to the conference upload to prevent cross mapping
- Change the Days of conference period id to something speciifc to the conference upload to prevent cross mapping
- Follow RT, Reply, Quote up and down a la treeverse
- Correct name attribue of object Source to remove href