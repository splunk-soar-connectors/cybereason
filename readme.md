[comment]: # " File: readme.md"
[comment]: # ""
[comment]: # "  Licensed under the Apache License, Version 2.0 (the \"License\");"
[comment]: # "  you may not use this file except in compliance with the License."
[comment]: # "  You may obtain a copy of the License at"
[comment]: # ""
[comment]: # "      http://www.apache.org/licenses/LICENSE-2.0"
[comment]: # ""
[comment]: # "  Unless required by applicable law or agreed to in writing, software distributed under"
[comment]: # "  the License is distributed on an \"AS IS\" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,"
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

-   The below-mentioned actions have been added. Hence, it is requested to the end-user to please update their
    existing playbooks by inserting | modifying | deleting the corresponding action blocks for this action on the earlier versions of the app.
	-   isolate specific machine
	-   unisolate specific machine
	-   upgrade sensor
	-   restart sensor
	-   query machine ip
