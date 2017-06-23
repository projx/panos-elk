# Logstash, Elastic and Kibana configurations for Palo Alto Networks PanOS v8.x and ELK v5.x

These are the main config files that I've put together for consuming PanOS v8.x syslog into ELK v5.x, this was done as a little home project, where I run a PA-200 and wanted some visibility on traffic. 

## Prerequesites

* Thoroughly read this file before usage
* You must have a Palo Alto Networks firewall
* A linux server 
* Elastic Search v5.x installed
* Logstash v5.x installed
* Kibana v5.x installed

## Understanding my madness
I apologise now if the directory structure seems like maddness, but I wanted a layout that I could keep contained in a single location, so I could add all the ELK configs to a single repository and simply symlink where necessary.

Likewise, my logstash config might seem nuts. Which listens for syslog on 2-3 seperate ports, this so that I can send different syslog (traffic, threat etc) and use a seperate config file to distinguish between them (to allow clean seperation of the parsers).

## Installation
### Confgs
In each top directory is a conf.d, which contains the loadable configs for each component of ELK, you can either drop the content of these into your existing logstash/conf.d/ or elasticsearch/conf.d/ directory, or simply delete/rename your existing conf.d folders and symlink to the repository/logstash/conf.d directories.

### Logstash
At minimum you may need to make the follow changes to the logstash/conf.d/50-OUTPUT file
1. Change the "hosts" parameter to point to your Elasticsearch IP (probably localhost)
2. Change the "template" parameter path, this must point to the /logstash/templates/elasticsearch-template.json file 
3. Comment out the stdout { } section (which copies the parsed entries to the syslog, this is only useful for debugging)

### Firewalls
The Logstash configs are written to listen for Traffic syslog on TCP/UDP 1514, and also Threat/Web logs on 1515. You will need to configure your Palo Alto device accordingly.. Or you can change the way this works, such as sending your logs to Rsyslog.

## Getting Started
I'll leave this to you!


## To do:
- Add parser for IPS/IDS events

## Major thanks

These configs are heavily based upon articles from https://www.anderikistan.com/, cheers for the great articles. Unfortunately they were a little over engineered for my needs and I had problems using them with ELK 5.x and PanOS v8.x.

## Finally
If you find any issues or make enhancements please let me know. I don't have time to offer technical support or answer questions. So, if you have issues using this, then please ask on the Elastic.co forums..

## Legal bit
You can freely copy and butcher this as you like, I claim no rights on this. I offer no gaurantee this works. By downloading this, you acknowledge and accept full liability for any damage or mental anguish usage may cause.
