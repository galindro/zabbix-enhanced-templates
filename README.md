# zabbix-enhanced-templates
Enhanced templates for Zabbix server. These templates adds new items and triggers for deep OS monitoring

## List of templates

### Template Linux Limits

This template get the results of ulimit command

### Template Linux Memory

This template was built using [vm.memory.size] (https://www.zabbix.com/documentation/2.4/manual/appendix/items/vm.memory.size_params) function and all of its parameters.

### Template Linux Processes

This template uses [get_proc_stats.sh script] (https://github.com/galindro/zabbix-enhanced-templates/blob/master/scripts/get_proc_stats.sh) to search the PID of a given process or processes by using LLD discovery. For discovery, the script will need as first parameter, a JSON string and, as second parameter, the string discovery. 
 
```bash
# Script help:

 Usage:

  For discovery: ./get_proc_stats.sh <json> discovery
   * Example #1: Discover process pids with exactly names "trivial-rewrite" and "qmgr" that are owned by postfix and discover process pids with "zabbix" on its name and "/usr/bin" regexp on its entire command line -> ./get_proc_stats.sh '{"postfix":{"exactly":["trivial-rewrite","qmgr"]},"root":{"name":["zabbix","ssh"],"cmd":["/usr/bin"]}}' discovery
   * Example #2: ./get_proc_stats.sh '{"postfix":{"exactly":["trivial-rewrite","qmgr"]},"root":{"name":["zabbix","ssh"],"cmd":["/bin/sh"]}}' discovery

  For counters: ./get_proc_stats.sh <pid> <options>
   * Example #1: Retreive process file descritors quantity -> ./get_proc_stats.sh 928 fd
   * Example #2: Retreive process state -> ./get_proc_stats.sh 928 state
```

### Template Linux Vulnerabilities

This template is intended to show common vulnerabilities found in some Linux packages and libraries

## Installation

### Zabbix Server

* Import the required value mappings into database

```bash 
mysql -h $MYSQL_HOST -u $MYSQL_USER -p $MYSQL_DATABASE < sql/value-mappings.sql
```

* Import the templates through Zabbix web interface

### Zabbix Client

* Run this script on each monitored host:

```bash 
apt-get update && apt-get -y install smem jq
mkdir -p /etc/zabbix/scripts/
cd /etc/zabbix/scripts/
wget https://raw.githubusercontent.com/galindro/zabbix-enhanced-templates/master/scripts/get_proc_stats.sh
chmod 0700 get_proc_stats.sh
```

## Configuration

### Template Linux Processes

* Add it to the host
* Create a user macro called {$PROCS_TO_SEARCH} with the required JSON for processes search
* Wait for discovery process to find the PIDs
