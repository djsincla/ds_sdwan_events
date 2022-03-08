# Name
ds_sdwan_events

# Function

Extract the VMware(r) SD-WAN Event Log to Splunk via the VCO REST API. 

The API call to VeloCloud Orchestrator (VCO) specifies an interval to minimize the performance impact to VCO of frequent API calls. It is recommended an interval of 120-600 seconds to poll VCO.

There is some overlap between API calls to VCO (interval x 2) to ensure no records are missed. We assume the record ID associated with each VCO event record is ascending and we save the last record ID written and only write records to Splunk which are greater than the saved record ID.

We mask and encrypt the VCO API Token and save to the Splunk Password DB.

# Author
Dwayne Sinclair

# License
Splunkbase Developer Distribution License - https://cdn.apps.splunk.com/static/misc/eula.html

# Support / Disclaimer
This is supported by Dwayne Sinclair as-is without any impled warranty. I make every efffort to keep this code validated against current versions of Splunk, Splunk Python API, and VMware SD-WAN VCO. Do not hesitate to reach out to be if you have questions or issues.

For support, you can log an issue at https://github.com/djsincla/ds_sdwan_events/issues or send an email to support@beyondcli.com

# Version
2.0.2

# Change Log
- Updated to Python3
- Added Support for VCO API Tokens Replacing Passwords
- 7/16/21   Tested ok with VCO Version 4.2.1
- 7/16/21   Tested ok with Splunk Enterprise 8.2.1
- 7/16/21   Updated to latest version of splunklib and deleted external python libraries from ~/bin
- 7/16/21   Updated version numbers...  
            1.0.x (main) is original Python2 version of code. 
            1.1.x (python3) is Python3 with userid and password.
            1.2.x (Token3) is Python3 with security token.
- 12/17/21  Updated to the latest version of splunklib the Splink Python API.
            Added Dashboards     
- 12/23/21 2.0.0 Renamed from TA_Velocloud to ds_sdwan_events and repackaged for submission to Splunk. 
- 12/24/21 2.0.1 Updated location of SplunkLib to /lib as recommended in Splunk API Documentation.
- 03/07/22 2.0.2 Added default interval = 300 to default/inputs.conf.
- 03/07/22 2.0.3 Minor edits to release notes.
- 03/07/22 2.0.4 Minor Splunk Cloud validation edits.

# With thanks to:
Ken Guo, Andrew Lohman, Kevin Fletcher

# Installation / Setup
- If you downloaded from github, rename the folder to "ds_sdwan_events" and copy to $SPLUNK_HOME/etc/apps and restart Splunk.
- Once the application is installed, go to Settings/Data Inputs/SD-WAN VCO Event Log and add the VCO together with username and authentication token associated with username. 
- Note that the username is used as a key to store state information in Splunk DB. It also helps as a reminder as to what username was used to generate the VCO API Token from.
- VCO API Tokens have a maximum life of 12 months. Set a calendar entry to remember to renew the API token. An upcoming release will perform token automatic renew.  

# Dependencies
-	Splunk Enterprise 8.0+
-	Python 3.x
-	VMware SD-WAN VCO Enterprise Username and API Token.
-	Enterprise user account must be “Superuser”, “Standard Admin”, or “Customer Support” role.
-   Tested to VCO V4.2

# Configuration: Settings/Data Inputs/SD-WAN VCO Event Log:

Required values are:

Name – The name given to this Modular Input. It is recommended to give it the name of the VeloCloud Orchestrator and Enterprise.

VCO URL – The https URL of the VMware SD-WAN VCO. As an example, https://vco84-usvi1.velocloud.net

Username – The VeloCloud Orchestrator username for a VCO Enterprise User.

API Token – The API Token generated against this username.

Optional values are:

More Settings – Exposes additional configuration options. 

Interval – Polling interval in seconds between requests to the VeloCloud Orchestrator for event log data. Default is 300 seconds. Minimum is 120 seconds.

Source type, Host, and Index options are Splunk environment specific. Your Splunk administrator will recommend appropriate setting to use. 

# Issues
03/2021 - Currently there is no automatic refresh of the API Token. Add a calendar item as a reminder to refresh the token before it expires.

# Logging
Modular input event logging is to the splunkd.log file found at ../Splunk/var/log/splunk/splunkd.log. Filter on sdwan to find all log messages associated with this modular input.
