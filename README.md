# GNET

Product: https://www.gnetsystem.com.my/product-cam-g-onx.html

Version: G-ONX

## Finding 1 (high risk): Default Credentials Cannot be Changed. [CWE-1393]
The GNET G-ONX dashcam uses a fixed default SSID and password ("qwertyuiop"), which cannot be modified by users. 
Additionally, the SSID is continuously broadcast, making the device network perpetually open to unauthorized access.

## Finding 2 (low risk): Hardcoded credentials in APK to ports 9091 (API) and 9092 (RTSP)
The GNET G-ONX dashcam’s Android APK (GNET v2.6.2) contains hardcoded credentials that allow unauthorized access to device settings through ports 9091 and 9092. Once connected to the dashcam's SSID, an attacker can send a crafted command ("TibetList" and "000*") to retrieve device settings from port 9091. Additionally, credentials stored in plaintext, such as "admin" + "tib*" for port 9092 (stream) and "adim" + "000*" for settings, can be exploited by an attacker who gains access to the dashcam’s network.

## Finding 3 (critical risk): Bypassing of device pairing [CWE-798]
The GNET G-ONX dashcam’s pairing mechanism relies solely on the connecting device’s MAC address. By obtaining the MAC address through ARP scanning and spoofing it, an attacker can bypass the authentication process and gain full access to the dashcam’s features without proper authorization. ​

## Finding 4 (high risk): Remotely Dump Video Footage and Live Video Stream
Once connected and authenticated, we are able to list all video recordings on the SD card remotely via port 9091 which we would then convert from jdr to mp4. We were also able to open a secondary socket to 9092 to dump live feed after validating the challenge-response key. We then proceeded to extract sensitive data from the recordings including location.
<img width="778" alt="image" src="https://github.com/user-attachments/assets/e4558607-7fcc-44d5-9422-ccb65f73cf71" />


## Finding 5 (high risk): Managing Settings to Obtain Sensitive Data and Sabotaging Car Battery
After connecting to the dashcam, an attacker can obtain sensitive user and vehicle information through the GNET G-ONX dashcam’s settings interface. The attacker can then read configuration settings, including sensitive car and driver information, and modify various system parameters. Attackers can lower the dashcam volume while executing remote commands to avoid detection. Additionally, they can turn off battery protection, causing the dashcam to drain the vehicle’s battery when parked overnight. Remote attackers can also delete recorded footage that may serve as evidence, disable recording discreetly, or even perform a factory reset, leading to a complete loss of stored data.

## Finding 6 (medium risk): Public Domain Used for Internal Domain Name
Inspecting the DNS traffic, we also noticed that a configured internal domain name was using a public domain with ".com" as the tld and that domain name wasn't registered. We've registered that to protect dashcam owners from attackers, notified the manufacturer within 24 hours, and offered to handover the domain securely.
