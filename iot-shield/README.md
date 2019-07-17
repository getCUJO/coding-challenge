# IoT Shield Challenge
Lets build a tiny security app to protect IoT devices based on some allowed behavioral patterns.
We have a fancy AI powered system that can provide some profile data for devices. Based on it we can contain devices that were hacked and stop them from accessing malicious content.
All of the input will be provided in the `input.txt`, it will provide device profile via `profile_update` and `profile_create` messages and activity that we're observing for various devices.
Everything is provided as JSON, one entry per line:
```
{"type": "profile_create", "model_name": "iPhone", "default": "allow", "whitelist": [], "blacklist": ["bad_site.com"], "timestamp": 1562827784}
{"type": "profile_update", "model_name": "iPhone", "blacklist": "bad_site_2.com", "timestamp": 1562827784}
{"type": "profile_update", "model_name": "iPhone", "whitelist": "bad_site.com", "timestamp": 1562827784}
{"type": "request", "request_id": "66d03a6947c048009f0b34260f35f3bd", "model_name": "iPhone", "device_id": "7fcbe1dff23947e5b3db8a20c1a2f8c0", "url": "facebook.com", "timestamp": 1562827784}
{"type": "request", "request_id": "4cbe5e573971474b8de99f9af24b8949", "model_name": "iPhone", "device_id": "7fcbe1dff23947e5b3db8a20c1a2f8c0", "url": "bad_site_2.com", "timestamp": 1562827784}
{"type": "request", "request_id": "fe5328eead7e417ab94413afbae907fd", "model_name": "iPhone", "device_id": "7fcbe1dff23947e5b3db8a20c1a2f8c0", "url": "facebook.com", "timestamp": 1562827784}
{"type": "request", "request_id": "288e1ec951384e6cae2b0de7520afef7", "model_name": "iPhone", "device_id": "7fcbe1dff23947e5b3db8a20c1a2f8c0", "url": "bbc.co.uk", "timestamp": 1562827784}
```
Where default value shows whether network activity is allowed or blocked by default. Whitelist and blacklist allows to override values - if the url starts with a value that is in the blacklist it must be blocked. Reverse is true for the whitelist.

Output of the application must output responses to the requests, whether the request must be blocked or allowed:
```
{"requestId": "66d03a6947c048009f0b34260f35f3bd", "action": "allow"}
{"requestId": "4cbe5e573971474b8de99f9af24b8949", "action": "block"}
```

If a device has a default policy of `block` and sends invaild requests lets move it to quarantine by sending a `quarantine` message:
```
{"action": "quarantine", "deviceId": "cdc7f7ecc2fa400bb30a72735027d915"}
```
#### Output of the application:
Output responses (allow/block commands for all of the requests)

#### Output statistics:
- How many devices were protected by our solution.
- How many devices could have been protected if profile updates would come sooner.
- How many blocks were issued incorrectly due to late updates profiles.