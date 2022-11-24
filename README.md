[comment]: # "Auto-generated SOAR connector documentation"
# Cybereason

Publisher: Cybereason  
Connector Version: 2\.3\.0  
Product Vendor: Cybereason  
Product Name: Cybereason  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 5\.3\.0  

This app integrates with the Cybereason platform to perform investigative, contain, and corrective actions on Malop and Malware events

[comment]: # " File: README.md"
[comment]: # ""
[comment]: # "  Licensed under the Apache License, Version 2.0 (the 'License');"
[comment]: # "  you may not use this file except in compliance with the License."
[comment]: # "  You may obtain a copy of the License at"
[comment]: # ""
[comment]: # "      http://www.apache.org/licenses/LICENSE-2.0"
[comment]: # ""
[comment]: # "  Unless required by applicable law or agreed to in writing, software distributed under"
[comment]: # "  the License is distributed on an 'AS IS' BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,"
[comment]: # "  either express or implied. See the License for the specific language governing permissions"
[comment]: # "  and limitations under the License."
[comment]: # ""
## Overview

The Cybereason platform finds a single component of an attack and connects it to other pieces of
information to reveal an entire campaign and shut it down. There are two types of alerts that
Cybereason will create:

-   Malops: This stands for a Malicious Operation, and will describe machines, users, processes, and
    connections used in the attack.
-   Malware: These alerts are generated when a user tries to run a piece of malware.

## Playbook Backward Compatibility

-   The below-mentioned actions have been added. Hence, it is requested to the end-user to please
    update their existing playbooks by inserting \| modifying \| deleting the corresponding action
    blocks for this action on the earlier versions of the app.
    -   isolate specific machine
    -   unisolate specific machine
    -   upgrade sensor
    -   restart sensor
    -   query machine ip


### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a Cybereason asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**base\_url** |  required  | string | The URL of the Cybereason server to connect to\. This should be of the form 'https\://<server name or ip>\:<port>'
**verify\_server\_cert** |  optional  | boolean | If checked, will verify the SSL certificate of the Cybereason server
**username** |  required  | string | A valid username for connecting to the Cybereason server
**password** |  required  | password | A valid password for connecting to the Cybereason server
**malop\_historical\_days** |  required  | numeric | The number of days for which we want to get Malops \(This parameter will be used for the first\-time poll only, and will be ignored in subsequent polls\)
**malware\_historical\_days** |  required  | numeric | The number of days for which we want to get Malware \(This parameter will be used for the first\-time poll only, and will be ignored in subsequent polls\)
**override\_malop\_severity\_map** |  optional  | string | A JSON string that the user can add to override the default severity mapping for different malop types
**malware\_severity** |  optional  | string | The severity to apply for all malware events

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using supplied configuration  
[on poll](#action-on-poll) - Callback action for the on\_poll ingest functionality  
[delete registry key](#action-delete-registry-key) - Deletes the specified registry key for a given malop ID and machine name  
[get sensor status](#action-get-sensor-status) - Get the connectivity status for all machine sensors in a Malop  
[add malop comment](#action-add-malop-comment) - Add a comment to the provided Malop ID  
[update malop status](#action-update-malop-status) - Update status for the provided Malop ID such as Under Investigation, To review, etc  
[isolate machine](#action-isolate-machine) - Blocks all communication to and from the machine\. Communication with the Cybereason platform is not affected  
[unisolate machine](#action-unisolate-machine) - Unblocks all communication to and from the machine  
[isolate specific machine](#action-isolate-specific-machine) - Blocks all communication to and from the machine identified by the given Name or IP\. Communication with the Cybereason platform is not affected  
[unisolate specific machine](#action-unisolate-specific-machine) - Unblocks all communication to and from the machine identified by the given Name or IP\. Communication with the Cybereason platform is not affected  
[kill process](#action-kill-process) - Kills the active process on the machine  
[get remediation status](#action-get-remediation-status) - Gets the remediation status for a previously executed remediation action like Kill Process  
[set reputation](#action-set-reputation) - Blacklists / Whitelists / Removes a file hash reputation so that future malop detections can quickly identify the hash  
[query processes](#action-query-processes) - Queries a given malop to retrieve all processes  
[query machine](#action-query-machine) - Queries a given machine name to retrieve all that machine's information  
[query machine ip](#action-query-machine-ip) - Queries a given machine IP to retrieve all that machine's information  
[query users](#action-query-users) - Queries a given user to retrieve all user\-related details  
[query files](#action-query-files) - Queries a given filename to retrieve all file details  
[query domain](#action-query-domain) - Queries a given domain name to retrieve all details of that domain  
[query connections](#action-query-connections) - Queries a given name to retrieve all details of that connection  
[upgrade sensor](#action-upgrade-sensor) - Upgrade a sensor  
[restart sensor](#action-restart-sensor) - Restart a sensor  

## action: 'test connectivity'
Validate the asset configuration for connectivity using supplied configuration

Type: **test**  
Read only: **True**

Test the connectivity with the Cybereason server configured by the user\.

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'on poll'
Callback action for the on\_poll ingest functionality

Type: **ingest**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**container\_count** |  optional  | Maximum number of containers to ingest | numeric | 
**container\_id** |  optional  | Parameter ignored in this app | string | 
**start\_time** |  optional  | Parameter ignored in this app | numeric | 
**end\_time** |  optional  | Parameter ignored in this app | numeric | 
**artifact\_count** |  optional  | Parameter ignored in this app | numeric | 

#### Action Output
No Output  

## action: 'delete registry key'
Deletes the specified registry key for a given malop ID and machine name

Type: **correct**  
Read only: **False**

A malop can contain processes that write to Windows registry keys\. This action will fire a remediation action on the Cybereason console that will delete the registry key using the Cybereason sensor installed on the machine\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The ID of the malop for deleting the registry key | string |  `cybereason malop id` 
**machine\_name** |  required  | The name of the machine on which we want to delete the registry key | string |  `cybereason machine name` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.parameter\.machine\_name | string |  `cybereason machine name` 
action\_result\.data\.\*\.remediation\_id | string |  `cybereason remediation id` 
action\_result\.data\.\*\.initiating\_user | string |  `cybereason user` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'get sensor status'
Get the connectivity status for all machine sensors in a Malop

Type: **investigate**  
Read only: **True**

Each machine covered by Cybereason will have a sensor installed on it\. This action will extract the sensor status for all machines in a Malop\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we will get sensor status information | string |  `cybereason malop id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.data\.\*\.machine\_name | string |  `cybereason machine name` 
action\_result\.data\.\*\.machine\_id | string |  `cybereason machine id` 
action\_result\.data\.\*\.status | string |  `cybereason sensor status` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'add malop comment'
Add a comment to the provided Malop ID

Type: **generic**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to add a comment | string |  `cybereason malop id` 
**comment** |  optional  | The comment that we want to add to the malop | string | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.parameter\.comment | string | 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'update malop status'
Update status for the provided Malop ID such as Under Investigation, To review, etc

Type: **generic**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to update the status | string |  `cybereason malop id` 
**status** |  required  | The status that will be assigned to the malop | string |  `cybereason malop status` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.parameter\.status | string |  `cybereason malop status` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'isolate machine'
Blocks all communication to and from the machine\. Communication with the Cybereason platform is not affected

Type: **contain**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to isolate the machine | string |  `cybereason malop id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'unisolate machine'
Unblocks all communication to and from the machine

Type: **correct**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to unisolate the machine | string |  `cybereason malop id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'isolate specific machine'
Blocks all communication to and from the machine identified by the given Name or IP\. Communication with the Cybereason platform is not affected

Type: **contain**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**machine\_name\_or\_ip** |  required  | Name or IP of the machine that needs to be isolated | string |  `cybereason machine name or ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.machine\_name\_or\_ip | string |  `cybereason machine name or ip` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'unisolate specific machine'
Unblocks all communication to and from the machine identified by the given Name or IP\. Communication with the Cybereason platform is not affected

Type: **correct**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**machine\_name\_or\_ip** |  required  | Name or IP of the machine that needs to be unisolated | string |  `cybereason machine name or ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.machine\_name\_or\_ip | string |  `cybereason machine name or ip` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'kill process'
Kills the active process on the machine

Type: **contain**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to kill the active process | string |  `cybereason malop id` 
**remediation\_user** |  required  | The user\-id who is killing the process | string |  `cybereason user` 
**machine\_id** |  required  | Machine ID associated with that malop ID | string |  `cybereason machine id` 
**process\_id** |  required  | Cybereason Process ID of the process to kill | string |  `cybereason process id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.parameter\.remediation\_user | string |  `cybereason user` 
action\_result\.parameter\.machine\_id | string |  `cybereason machine id` 
action\_result\.parameter\.process\_id | string |  `cybereason process id` 
action\_result\.data\.\*\.remediation\_id | string |  `cybereason remediation id` 
action\_result\.data\.\*\.remediation\_status | string |  `cybereason remediation status` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'get remediation status'
Gets the remediation status for a previously executed remediation action like Kill Process

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The malop ID for which we want to get the remediation status | string |  `cybereason malop id` 
**remediation\_user** |  required  | The user ID that has requested the remediation action | string |  `cybereason user` 
**remediation\_id** |  required  | An ID that specifies a previously executed remediation action like Kill Process | string |  `cybereason remediation id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.parameter\.remediation\_user | string |  `cybereason user` 
action\_result\.parameter\.remediation\_id | string |  `cybereason remediation id` 
action\_result\.data\.\*\.remediation\_status | string |  `cybereason remediation status` 
action\_result\.data\.\*\.remediation\_message | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'set reputation'
Blacklists / Whitelists / Removes a file hash reputation so that future malop detections can quickly identify the hash

Type: **generic**  
Read only: **False**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**reputation\_item\_hash** |  required  | The item \(hash, IP\) for which we want to set the reputation | string |  `hash` 
**custom\_reputation** |  required  | The custom reputation that we want to set for the reputation item | string | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.reputation\_item\_hash | string |  `hash` 
action\_result\.parameter\.custom\_reputation | string | 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query processes'
Queries a given malop to retrieve all processes

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**malop\_id** |  required  | The ID of the malop for which we want to get process details | string |  `cybereason malop id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.malop\_id | string |  `cybereason malop id` 
action\_result\.data\.\*\.process\_id | string |  `cybereason process id` 
action\_result\.data\.\*\.process\_name | string |  `cybereason process name` 
action\_result\.data\.\*\.owner\_machine\_id | string |  `cybereason machine id` 
action\_result\.data\.\*\.owner\_machine\_name | string |  `cybereason machine name` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query machine'
Queries a given machine name to retrieve all that machine's information

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**name** |  required  | The name of the machine | string | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.name | string | 
action\_result\.data\.\*\.machine\_id | string |  `cybereason machine id` 
action\_result\.data\.\*\.machine\_name | string | 
action\_result\.data\.\*\.os\_version | string | 
action\_result\.data\.\*\.platform\_architecture | string | 
action\_result\.data\.\*\.is\_connected\_to\_cybereason | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query machine ip'
Queries a given machine IP to retrieve all that machine's information

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**machine\_ip** |  required  | The IP of a machine | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.machine\_ip | string |  `ip` 
action\_result\.data\.\*\.machine\_id | string |  `cybereason machine id` 
action\_result\.data\.\*\.machine\_name | string | 
action\_result\.data\.\*\.os\_version | string | 
action\_result\.data\.\*\.platform\_architecture | string | 
action\_result\.data\.\*\.is\_connected\_to\_cybereason | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query users'
Queries a given user to retrieve all user\-related details

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**user** |  required  | The name of the user | string |  `cybereason user` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.user | string |  `cybereason user` 
action\_result\.data\.\*\.element\_name | string | 
action\_result\.data\.\*\.domain | string |  `domain` 
action\_result\.data\.\*\.last\_machine\_logged\_into | string | 
action\_result\.data\.\*\.organization | string | 
action\_result\.data\.\*\.local\_system | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query files'
Queries a given filename to retrieve all file details

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**file\_name** |  required  | The name of the file | string |  `file name` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.file\_name | string |  `file name` 
action\_result\.data\.\*\.element\_name | string | 
action\_result\.data\.\*\.suspicion\_count | string | 
action\_result\.data\.\*\.signed | string | 
action\_result\.data\.\*\.SHA1\_signature | string | 
action\_result\.data\.\*\.size | string | 
action\_result\.data\.\*\.path | string | 
action\_result\.data\.\*\.product\_name | string | 
action\_result\.data\.\*\.company\_name | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query domain'
Queries a given domain name to retrieve all details of that domain

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**domain\_name** |  required  | The name of the domain | string |  `domain` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.domain\_name | string |  `domain` 
action\_result\.data\.\*\.element\_name | string | 
action\_result\.data\.\*\.malicious\_classification\_type | string | 
action\_result\.data\.\*\.is\_internal\_domain | string | 
action\_result\.data\.\*\.was\_ever\_resolved | string | 
action\_result\.data\.\*\.was\_ever\_resolved\_as\_second\_level\_domain | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'query connections'
Queries a given name to retrieve all details of that connection

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**connection\_name** |  required  | The name of the connection | string | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.connection\_name | string | 
action\_result\.data\.\*\.element\_name | string | 
action\_result\.data\.\*\.direction | string | 
action\_result\.data\.\*\.server\_address | string | 
action\_result\.data\.\*\.server\_port | string | 
action\_result\.data\.\*\.port\_type | string |  `port` 
action\_result\.data\.\*\.received\_bytes | string | 
action\_result\.data\.\*\.transmitted\_bytes | string | 
action\_result\.data\.\*\.remote\_address | string | 
action\_result\.data\.\*\.owner\_machine | string | 
action\_result\.data\.\*\.owner\_process | string | 
action\_result\.data\.\*\.dns\_query | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'upgrade sensor'
Upgrade a sensor

Type: **generic**  
Read only: **False**

Upgrade a sensor using the Cybereason sensor pylum ID provided as an input\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**pylumid** |  required  | The Cybereason sensor pylum ID targeted for upgrade \(comma\-separated IDs allowed\) | string |  `cybereason sensor pylum id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.pylumid | string |  `cybereason sensor pylum id` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'restart sensor'
Restart a sensor

Type: **generic**  
Read only: **False**

Restart a sensor using the Cybereason sensor pylum ID provided as an input\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**pylumid** |  required  | The Cybereason sensor pylum ID targeted for restart \(comma\-separated IDs allowed\) | string |  `cybereason sensor pylum id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.pylumid | string |  `cybereason sensor pylum id` 
action\_result\.data | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 