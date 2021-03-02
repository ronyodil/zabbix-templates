# Zabbix 3.4 - E2Guardian 4.1.x & 5.0.x Template
This is a fork from [Xenadmin](https://github.com/xenadmin/zabbix-templates)

## Introduction
This is my approach for a performance monitoring Template of the content filtering proxy [E2guardian](http://e2guardian.org/).

This template was created on and for Zabbix 3.4 and E2Guardian 4.1.x on Debian 9 Stretch.  
There is no obvoious reason I'm aware of, that would prevent this from working with other Zabbix 3.x releases or other Linux Distributions. 

It's tested working on Ubuntu 16.04 with E2guardian 5.0.2 as well.
It's working on Debian 10 with Zabbix 4.0 and e2guardian 5.4.

## Installation

1. Zabbix Agent (Agent or Passive) should be installed, configured and working.
1. Add the corresponding [User parameter](https://www.zabbix.com/documentation/3.4/manual/config/items/userparameters) to your `zabbix_agentd.conf`.
   1. (I personally prefer to *not* alter the `zabbix_agentd.conf`. Instead I work with the `Include=` defined in the default `zabbix_agentd.conf` aka `Include=/etc/zabbix/zabbix_agentd.d/*.conf`.)
   1. `UserParameter=e2guardian[*],tail -1 $1 | cut -f $2`
   1. Don't forget to restart the zabbix agent afterwards. 
1. The [E2Guardian dstats.log](https://github.com/e2guardian/e2guardian/blob/master/notes/dstats_format) settings should be configured and enabled. Example:
   1. `dstatlocation = /path/to/dstats.log`
   1. `dstatinterval = 300  # = 5 minutes`
   1. Don't forget to restart e2guardian afterwards. 
1. Import the Template.
   1. Optional: Convert the `Zabbix agent (active)` item type to `Zabbix agent` passive via Mass update. 
1. Link the Template to your E2guardian Host.
1. Check the Template User Macro, if the pre-configured path to the e2guardian `dstats.log` file fits your configuration.

## Notes

1. Latest Version of Template is provided with Item type `Zabbix agent (active)`, but can easily be switched via Zabbix Frontend Mass Update!
1. Includes 1 Graph which uses configured count of max http-workers as Y axis. 
1. Includes 2 Triggers which check if worker or log queue build up. 

## Example Screenshot of Latest Data

![Latest Data](example01.png)

## Changes
- February 2018: initial commit.
- 03 April 2018: Switch to Zabbix agent (active) & converted system.run item keys to proper Zabbix custom [User parameters](https://www.zabbix.com/documentation/3.4/manual/config/items/userparameters).
- 20 December 2018: Added a few more graphs.
- 02 March 2021: Added graphs and changed parameters to match 1 minute interval in dstats. Also switched to passive agent.
