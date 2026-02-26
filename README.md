# Weishaupt CanApiJson  - Weishaupt CAPI
**(e.g.: {"ID":"12345678","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"020201258202000101"}}})**  

Anything you do with your Weishaupt gas boiler or system unit and/or compatible devices in connection with the information published here is at your own risk.
No liability is accepted for damages!

**short:**  
Read values from your and control your Weishaupt gas heating device, oil heating system or heat pump with:  
Curl (AmigaOS, OpenWrt, Dos, Windows, Linux, OpenWrt);  
Controlling your Weishaupt device even with:  
FHEM, Node-Red, OpenHab, IoBroker, Home Assistant implementations should be possible without any problems.  
Basically, you just need to know the CanApiJson datagram structure and the CanApiJson datagram magic patterns mapping table(see below).
  
**long:** just keep reading  
  
Weishaupt CanApiJson - CAN bus-like / CAN open-like protocol via JSON is a communication protocol between:  
"**Systemgerät**" (48301122172, 48301122242, 48301122512, 48301122522) and other Weishaupt (compatible) devices like a:  
"**Gateway WEM-Modbus**" data protocol converter ( 48300002722 ) or a:  
"**Gateway WEM-KNX**" data protocol converter ( 48300002012 )
  
**secifications/declareations (some kind of):**  
  
**WEM** = Weishaupt Energy Manager / Weishaupt Energy Management  
**Weishaupt WEM bus protocol** = "Weishaupt WEM-Bus-Protokoll"  
**(Weishaupt) CanApiJson protocol** = Weishaupt CanApiJson - CAN bus-like / CAN open -like via JSON -protocol  
**(Weishaupt) CanApiJson register/object number** = Weishaupt CanApiJson block address number or CanApiJson register/object number = Weishaupt WEM CanApiJson register/object number  
  
The resulting generated block address/register number(magic pattern - see mapping table below) on the Weishaupt CanApiJson protocol side may vary or could look different and it could depend on the way of presence of your Weishaupt devices or on your constellation of Weishaupt devices:  
  
**WTC** (WTC1 / WTC2 / ...)  
**SG** (SG 1 / SG 2 /...)  
**HK** (HK 1 / HK 2 /...)  
**WW** (WW 1 / WW 2 /...)  
**Sol** (SOL 1 / SOL 2 /...)  
**RF**, **RG1**, **RG2**, **KA**  
  
It is therefore very likely that the “Gateway WEM-Modbus” (or a “Gateway WEM-KNX”) first needs/reads the Weishaupt SG1 system table:  
http://wem-sg/sd/systable.csv  
for a suitable Weishaupt CanApiJson block address/register number specification/generation to create functioning JSON telegrams.  
  
Here you will find research results regarding the details and structure/function of the communication interface ("JSON") of a Weishaupt system device / control unit ("Systemgerät" - "SG"/"SG1").  

**If you have any Weishaupt CanApiJson realated news or corrections or any updates, please feel free to improve this documentation.**

This document makes it easy to directly integrate, read, control, regulate, and modify parameters of Weishaupt devices that use this CanApiJson protocol (gas boiler, heat pump, etc.) in DOS, Windows, AmigaOs, OpenWRT, Node-RED, Home Assistant, FHEM, ioBroker, etc.  
  
If the JSON function is enabled in the settings of the Weishaupt "Systemgerät" (SG / SG1 - Weishaupt control unit for the gas boiler/heat pump/...) and the Weishaupt "Systemgerät" is connected to the local network via the RJ-45 interface (DHCP server enabled or manually assigned IP address), then this address can be accessed with a browser:  
  
http://admin:Admin123@wem-sg/ajax/CanApiJson.json
  
Well, accessing this address with a browser is primarily for testing purposes only.  
Actual writing/reading values ​​from the "Systemgerät" / the heating control unit works differently:  
**via "POST" commands** (or via CURL)  
    
See details below.  
Furthermore a **Weishaupt Gateway WEM-Modbus** can be used to connect the world of Weishaupt WEM protocol with the world of Modbus TCP protocol.
This can also be used to find out which specific Modbus TCP register (and which documented function/specific value of the heating control[temperatures, pressures, operating states, etc.]) is transfered to the equivalent data point of the Weishaupt CanApiJson protocol.

##  Weishaupt Systemgerät - possible hardware names and item numbers / product codes:  
WEM-Systemgerät 2.5 -- Weishaupt Ersatzteil komplett mit SD-Karte - ersetzt: 48301122172, 48301122242, 48301122512  
Weishaupt WEM-Systemgerät 2.6 kpl. mit SD-Card: 48301122522  
  
### MAC address(es) (possible):  
00:23:7e:??:??:?? [Elster_??:??:??]  (Kuhlmannstrasse 10, 31785 Hameln, DE)
  
### Host name and addresses(URI / URN / URL):  
hostname: WEM-SG  
http://wem-sg (sg = Systemgerät)  
http://wem-sg.local/  
http://admin:Admin123@wem-sg/  
http://admin:Admin123@wem-sg.local/  
http://wem-sg/script/einstellung.js  
http://wem-sg/script/Form_eth_log.js  
http://wem-sg/script/ajax.js  
http://admin:Admin123@wem-sg/ajax/CanApiJson.json -->JSON content changes every 30 seconds (polling).  
http://wem-sg/sd/systable.csv --> could be figured out with the help of network package sniffing between "Systemgerät" and "Gateway WEM-Modbus"  

## Gateway WEM-Modbus (Weishaupt 48300002722)  

### MAC address(es) (possible):  
d0:76:50:3?:??:?? [TAPKOTechnol_?:??:??] (TAPKO Technologies GmbH, Im Gewerbepark A15, 93059 Regensburg, Bayern, DE)  
  
http://mod-whgw-301501:8080/index.shtml  
<img width="1788" height="886" alt="Weishaupt-Gateway_WEM-Modbus-01-masked" src="https://github.com/user-attachments/assets/de524872-ca1c-429f-a04d-0d7da4675611" />  
  
http://mod-whgw-301501:8080/wem.shtml  - before cofigure the IP setting to the Weishaupt Systemgerät:  
<img width="1788" height="886" alt="Weishaupt-Gateway_WEM-Modbus-02" src="https://github.com/user-attachments/assets/fb5c2cad-1068-46e3-9b38-87616e3a4e9f" />  
  
http://mod-whgw-301501:8080/wem.shtml  - after cofigure the IP setting to the Weishaupt Systemgerät:  
<img width="1920" height="1080" alt="Weishaupt-Gateway_WEM-Modbus-02a" src="https://github.com/user-attachments/assets/f13044b7-466d-4716-8ab3-3938f4129e4e" />  
  
http://mod-whgw-301501:8080/modbus.shtml  
<img width="1920" height="1803" alt="Weishaupt-Gateway_WEM-Modbus-05a" src="https://github.com/user-attachments/assets/8c185b37-27e4-4c88-ad1c-e42b1b148095" />  
  
### Host name and address(es):  
hostname: MOD-WHGW-xxyyzz (xxyyzz = second part of the MAC address)  
http://MOD-WHGW-xxyyzz:8080  
http://MOD-WHGW-xxyyzz.local:8080  
  
## Simple communication test to your Weishaupt - boiler / heat pump / "Systemgerät" / ... :  
**open:**  
`http://admin:Admin123@wem-sg/ajax/CanApiJson.json` in your browser.

Then, you should see something like this:  
`{"ID":"12345678","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"020201258202000101"}}}`

If you have got a compatible device, which can communicate to the Weishaupt system device / control unit ("Systemgerät" - "SG"/"SG1"), then you can put a computer in between both devices and bridge both network sockets.  
Then you can use Wireshark to see what happens betwwen both devices.  
  
**-->  schematics:**  
`Weishaupt SG1` <--> `network interface 1 of your computer`|`your computer`|`network interface 2 of your computer` <--> `another device that can communicate to the Weishaupt SG1` (Weishaupt Gateway WEM-Modbus / WEM-KNX)  
The device which can understand the Weishaupt WEM / CAN bus-like protocol and which can communicate to the Weishaupt SG1 will **"POST"** a message like this as a request to the Weishaupt SG1:  
  
`POST /ajax/CanApiJson.json HTTP/1.1`  
`Host: 192.168.178.124  (example Weishaupt SG1 IP address)`  
`Authorization: Basic YWRtaW46QWRtaW4xMjM=`  
`Referer: http://192.168.178.124/`  
`Content-Length: 215`  
`Connection: keep-alive**`  
  
`{"ID":"12345678","SRC":"DDC","CAPI":{"NN":5,"N01":{"VG":"010700254100000100"},"N02":{"VG":"010700253200000200"},"N03":{"VG":"010700253302000200"},"N04":{"VG":"010700253700000200"},"N05":{"VG":"010901263900000100"}}}` 
  
Then there will be a response from **SG1**:  

`HTTP/1.0 200 OK`  
`Content-Type: application/json`  
`Date: Sun, 22 Feb 2026 19:29:43 +0000`  
`Last-Modified: Sun, 22 Feb 2026 19:29:43 +0000`  
  
`{"ID":"12345678","SRC":"SYS","CAPI":{"NN":5,"N01":{"VG":"020700254100000100"},"N02":{"VG":"020700253200000200b2"},"N03":{"VG":"020700253302000200ab"},"N04":{"VG":"020700253700000200c4"},"N05":{"VG":"020901263900000100"}}}`  
  
As you can see, JSON technique will be used for the CAN / CAN open like Weishaupt WEM communication protocol - explanations:    
  
**ID** = ID_number --> seems to be the same everytime.(did Weishaupt forget to implent something here - why so unique?)  
**SRC** = Source of the message/telegram (DDC/SYS)   
**DDC** = ID_name of the Weishaupt Gateway Modbus-WEM  
**SYS** = ID_name of the Weishaupt "Systemgerät" (SG / SG1)  
**NN** = amount of following telegrams/messages --> "N01","N02","N03",... ("NN":5 means: "N01"{  },...,"N05"{  })  
**VG** = ?VG stands for what? --> signals that a CAN frame / message / telegram with data will follow  

The main part / the most important thing is the mesage/telegram after: **"VG"** - e.g. {**"VG"**:"02 07 00 2533 02 0002 00ab"} and here is what I could find out:  
Currently no guarantee for accuracy.  

|   CM   |   MI   |   MX   |   OX   |   OS   |   VS   |   VA   | (official descriptions and field size: http://wem-sg/script/Form_eth_log.js)  
| 1 Byte | 1 Byte | 1 Byte | 2 Byte | 1 Byte | 2 Byte | x Byte |  (field size)  

**CMD cases:**  
case 1:		//GET  (in this case: "DDC" wants to **GET** data)
case 2:		//Response  (in this case: "SYS" wants to **Response** data)
case 3:		//SET (NUMERIC)  
case 4:		//ACK  
case 19:	//SETS  (set string)
case 17:	//GETS  (get string)

**1 byte** - CM - command  
  
**1 bytes** - MI - CAN ID/group: 02 / 07  

**1 bytes** - MX - CAN - sub-ID / sub-groupc/ Member Index: 00 / 01 /  

**2 byte** - OX - object / object index: 2541 / 2532 / 2533 / 2537 / 2639 /
  
**1 byte** - OS - Offset  
  
**2 bytes** - VS - value size - amount of bytes to send / to receive: 01 / 02  
  
**x byte(s)** - value(s) inside the data point / register  (temperatures, pressures, states, date, time, etc...)
  
  
If you have better information or any relevant technical terms (perhaps from the CAN open / CAN bus world), please let me know.  
  
Inside the Weishaupt "Gateway WEM-Modbus" I did select just one Modbus TCP register and did sniff the network with the help of Wireshark.  
In Wireshark, I did use this display filter: (frame contains 43:41:50:49) to just see the frames with: "CAPI".  

So, I did repeat this for every single selectable Modbus TCP register in the web interface of the Weishaupt Gateway WEM-Modbus:
    
**a)** on the Weishaupt Gateway WEM-Modbus web interface: selection of just one/next Modbus TCP register which has to be transfered/converted to the Weishaupt WEM CanApiJson world  
**b)** sniffing the network - 6 request/response pairs ("DDC"/"SYS" messages) while using the wireshark filter: "(frame contains 43:41:50:49)"  
**c)** storing the result into a .pcapng file  
**d)** disable the previously selected Modbus TCP register in the Weishaupt Gateway WEM-Modbus web interface  
**e)** goto "a)"  

Different request-response pairs (“DDC”/“SYS” message block/pair) have different cycle times.  
To ensure the consistency and uniqueness of the Weishaupt CanApiJson messages (nothing else is sent from the gateway)  
and to figure out the cycle/polling time and to clearly determine both,  
6 request-response pairs (“DDC”/“SYS” message block/pair) for each individual activated Modbus TCP register were dumped/sniffed.  
  
For example (see mapping table below):  
Modbus register: 1030 (Weishaupt CanApiJson block/register/address: 25_3302) will by polled every 30 seconds by the Weishaupt Gateway WEM-Modbus.  
Modbus register: 114 (Weishaupt CanApiJson block/register/address: 27_f902) will be polled every 10 seconds by the Weishaupt Gateway WEM-Modbus.
  
Then I did use a script to keep just the important things inside the .pcapng network package dump files and to make some reformatting changes for clarity.  
So you can see below that each object/register on the Weishaupt CanApiJson side has a suitable register on the Modbus TCP side.  
  
Then you can check the meaning of each Modbus register here:  
https://www.loebbeshop.de/media/67944/file/static/pdf/weishaupt/manual-wem-modbustcp.pdf  
  
Now, there is a direct Weishaupt WEM CanApiJson register/object number <--> Modbus TCP register conversation possible. 
   
  
As you can see in the dump/mapping table below, the value of the: **Modbus register 118** (Weishaupt WEM block/register/address: 25_6002_0002) is: 0235.  
  
--> Regarding the pdf file (https://www.loebbeshop.de/media/67944/file/static/pdf/weishaupt/manual-wem-modbustcp.pdf)  
the **Modbus register 118** is: **heating water buffer tank temperature_top** ("**Pufferspeicher Temperatur oben**").  
  
--> The value is: **0235**[HEX] = **565** = **56,5 °C** --> This value matches the **heating water buffer tank temperature_top** while siffing the communication.  
  
  
## Mapping table (some kind of a)  
--> a certain Weishaupt address/object on the Weishaupt CanApiJson -side consists of:  
  ID **group** sub-group **object**  
--> additional information like: "register 1030 - **HK** - HK2"  
are the information from the "**Gateway WEM-Modbus**" web interface --> http://mod-whgw-301501:8080/modbus.shtml  
  
**"SRC":"...", "VG"**:"ID **group** sub-group **object** number-of-bytes **data**", modbus register - **Weishaupt device group** - device name:  
  
**"SRC":"DDC", "VG"**:"01 **0201** 25 **3302** 0001 **00**", register 1030 - **HK** - HK2  
**"SRC":"SYS", "VG"**:"02 **0201** 25 **3302** 0001 **02**"  
T_cycle: 30 s  

"SRC":"DDC", "VG":"01_0200_25_3302_0001_00", register 100 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_3302_0001_02"  
T_cycle: 30 s  

"SRC":"DDC", "VG":"01_0200_25_8202_0001_00", register 101 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_8202_0001_01"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_7e04_0001_00", register 102 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_7e04_0001_06"  
T_cycle: 30 s  

"SRC":"DDC", "VG":"01_0200_25_7e05_0001_00", register 103 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_7e05_0001_54"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_3b02_0002_00", register 106 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_3b02_0002_00dc"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_3a02_0002_00", register 107 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_3a02_0002_00d2"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_3902_0002_00", register 108 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_3902_0002_00a0"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_5802_0002_00", register 109 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_5802_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_6b02_0002_00", register 110 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_6b02_0002_02bc"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_6a02_0002_00", register 111 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_6a02_0002_0258"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_6902_0002_00", register 112 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_6902_0002_01c2"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_2c02_0002_00", register 113 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_2c02_0002_0190"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_27_f902_0002_00", register 114 - SG - SG1  
"SRC":"SYS", "VG":"05_0100_27_f902_0002_0001"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0200_25_5902_0002_00", register 115 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_5902_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0200_25_0702_0002_00", register 116 - SG - SG1  
"SRC":"SYS", "VG":"02_0200_25_0702_0002_0232"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_26_1f02_0002_00", register 117 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1f02_0002_8000"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_25_6002_0002_00", register 118 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6002_0002_0235"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_25_6102_0002_00", register 119 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6102_0002_0213"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_26_1e00_0001_00", register 124 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1e00_0001_03"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_26_b102_0002_00", register 125 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_b102_0002_0058"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_25_9c01_0002_00", register 126 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_9c01_0002_01b7"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_25_9d01_0002_00", register 127 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_9d01_0002_0000"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0300_25_6902_0001_00", register 130 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_6902_0001_01"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0300_25_4902_0001_00", register 131 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_4902_0001_00"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0300_25_3902_0002_00", register 132 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_3902_0002_0190"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0300_25_3802_0002_00", register 133 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_3802_0002_0190"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0300_25_2c02_0002_00", register 134 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_2c02_0002_0190"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0300_25_2902_0002_00", register 135 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_2902_0002_0242"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0300_25_5702_0002_00", register 136 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_5702_0002_8000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0300_25_5102_0001_00", register 137 - SG - SG1  
"SRC":"SYS", "VG":"02_0300_25_5102_0001_00"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_26_e402_0001_00", register 140 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_e402_0001_02"  
T_cycle: 60 s  
  
"SRC":"DDC", "VG":"01_0100_26_e602_0002_00", register 141 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_e602_0002_0032"  
T_cycle: 60 s  
  
"SRC":"DDC", "VG":"01_0100_26_fe02_0002_00", register 142 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_fe02_0002_8000"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_26_1c00_0002_00", register 143 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1c00_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_26_1d00_0002_00", register 144 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1d00_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_26_1200_0002_00", register 145 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1200_0002_01bd"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_26_1300_0002_00", register 146 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_26_1300_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_25_6202_0001_00", register 150 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6202_0001_14"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_25_6203_0001_00", register 151 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6203_0001_37"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0100_25_6302_0001_00", register 153 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6302_0001_1a"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_25_6303_0001_00", register 154 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6303_0001_02"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0100_25_6304_0001_00", register 155 - SG - SG1  
"SRC":"SYS", "VG":"02_0100_25_6304_0001_17"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0901_26_3900_0001_00", register 160 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_3900_0001_00"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0700_25_4100_0001_00", register 161 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0700_25_4100_0001_00"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0700_25_4500_0002_00", register 163 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0700_25_4500_0002_0050"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0700_25_3200_0002_00", register 164 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0700_25_3200_0002_00aa"  
T_cycle: 10 s  

"SRC":"DDC", "VG":"01_0901_26_1302_0002_00", register 165 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_1302_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0901_26_1402_0002_00", register 166 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_1402_0002_007a"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0700_25_3302_0002_00", register 167 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0700_25_3302_0002_00ad"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0700_25_3700_0002_00", register 168 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0700_25_3700_0002_00c4"  
T_cycle: 10 s  
  
"SRC":"DDC", "VG":"01_0901_26_3102_0004_00", register 170_171 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_3102_0002_0000"  
T_cycle: 30 s  
  
"SRC":"DDC", "VG":"01_0901_26_2802_0004_00", register 172_173 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_2802_0004_0000058e"  
T_cycle: 60 s  
  
"SRC":"DDC", "VG":"01_0901_26_2602_0004_00", register 174_175 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_2602_0004_00000436"  
T_cycle: 60 s  
  
"SRC":"DDC", "VG":"01_0901_26_2702_0004_00", register 176_177 - WTC - WTC1  
"SRC":"SYS", "VG":"02_0901_26_2702_0004_00000158"  
T_cycle: 60 s  

    
### CURL examples for reading/writing
**reading**:  
Reading the **heating water buffer tank temperature_top** ("**Pufferspeicher Temperatur oben**") (CanApiJson adress/group pattern: **25_6002** --> see mapping table)  
(Modbus TCP register: 118):  
  
**reading with CURL in Windows console/terminal:**  
`curl.exe --http1.1 -H "Connection: keep-alive" -H "User-Agent:" -H "Accept:" -H "Referer: http://192.168.178.124/" -H "Content-Type:" -u admin:Admin123 -d "{\"ID\":\"12345678\",\"SRC\":\"DDC\",\"CAPI\":{\"NN\":1,\"N01\":{\"VG\":\"010100256002000200\"}}}" http://192.168.178.124/ajax/CanApiJson.json`  
-->this is the request  
  
**response:**  
`{"ID":"00000001","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"02010025600200020212"}}}`  
  
--> As you can see, ID: 1 is also possible (not only the default ID: 12345678)  
--> {"VG":"........212 = 212Hex = 535 decimal = 53,5 deg. celsius  
--> working perfectly.  

  
**writing & confirm/response writing:**  
Writing the: **"Pre-flow set temperature - Comfort"** (**"Vorlaufsolltemperatur Komfort"**) (CanApiJson adress/group pattern: **25_6b02** --> see mapping table)  
(Modbus TCP register: 110 on the Gateway WEM-Modbus side):  
  
I did enable just the register 110 in the **Weishaupt Gateway WEM-Modbus data protocol converter** web interface.  
Then, I used PowerHud modbustester software  
--->IP-address: the WEM-Modbus data protocol converter|Port: 501|Slave ID: 1|Register Start: **110(Vorlaufsolltemperatur Komfort)**|Count: 1  
and changed the mode from **03-Read Holding Registers** to: **06-Write Single Register**.  
After this, I changed the default value from 700 (→ 70,0 °C = **02BC** [HEX]) to 655 (→ 65,5 °C = **028F** [HEX]).  
  
In Wireshark I could sniff/dump these Weishaupt CanApiJson frame packages:  
  
**Weishaupt Gateway WEM-Modbus data protocol converter** ----> **Weishaupt SG (Systemgerät)**:  
`{"ID":"12345678","SRC":"DDC","CAPI":{"NN":1,"N01":{"VG":"**03**0200256b020002**028F**"}}}`  
  
**Weishaupt SG (Systemgerät)** ----> **Weishaupt Gateway WEM-Modbus data protocol converter**:  
`{"ID":"12345678","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"**04**0200256b020002**028F**"}}}`
  
You can see the payload of: **028F** (→ 65,5 °C = **028F** [HEX]) and you can see the new commands: **03**/**04**  
**03** seems to be: request writing  
**03** seems to be: response/confirm writing  
 
**05** seems to be: error / something went wrong  
try this:  
`curl.exe --http1.1 -H "Connection: keep-alive" -H "User-Agent:" -H "Accept:" -H "Referer: http://192.168.178.124/" -H "Content-Type:" -u admin:Admin123 -d "{\"ID\":\"12345678\",\"SRC\":\"DDC\",\"CAPI\":{\"NN\":1,\"N01\":{\"VG\":\"010200256002000200\"}}}" http://192.168.178.124/ajax/CanApiJson.json`  
and you will get:  
`{"ID":"12345678","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"**05**020025600200020001"}}}`  
  
--> 010 **2** 00256002000200 in the request frame was wrong  
--> 010 **1** 00256002000200 in the request frame would be right  
  
**writing with CURL:**  
`curl.exe --http1.1 -H "Connection: keep-alive" -H "User-Agent:" -H "Accept:" -H "Referer: http://192.168.178.124/" -H "Content-Type:" -u admin:Admin123 -d "{\"ID\":\"12345678\",\"SRC\":\"DDC\",\"CAPI\":{\"NN\":1,\"N01\":{\"VG\":\"030200256b020002028a\"}}}" http://192.168.178.124/ajax/CanApiJson.json`  
  
**response: (from the Weishaupt SG)**  
`{"ID":"12345678","SRC":"SYS","CAPI":{"NN":1,"N01":{"VG":"040200256b020002028a"}}}`  
  
---> changing **"Vorlaufsolltemperatur Komfort"** to 028a (650 = 65,0 °C) was **successfully**.  
  
  
**Conclusion:**
  
With the help of the pattern/mapping table you can read/write everything from/to your **CanApiJson compatible** Weishaupt gas heating system, oil heating system, or heat pump.  
Now, everything is possible in: Home Assistant, Node Red, Linux Terminal command(curl), Windows terminal/shell/command prompt.  
  
  
**Old/draft:**  

The ID is everytime the same --> did Weishaupt forget to implent something unique?  

**After setting up and configure the Weishaupt Gateway WEM-Modbus, the device will send a package ("DDC") to the "Systemgerät":**  
  
POST /ajax/CanApiJson.json HTTP/1.1  
Host: <IP of the "Systemgerät">  
Authorization: Basic YWRtaW46QWRtaW4xMjM= (Base64)  
Referer: http://<IP of the "Systemgerät"> /  
Content-Length: 386  
Connection: keep-alive  

{"ID":"12345678","SRC":"DDC","CAPI":{"NN":10,"N01":{"VG":"01010027f902000200"},"N02":{"VG":"010100261f02000200"},"N03":{"VG":"010100259c01000200"},"N04":{"VG":"010100259d01000200"},"N05":{"VG":"01010026fe02000200"},"N06":{"VG":"010100256202000100"},"N07":{"VG":"010100256203000100"},"N08":{"VG":"010200252c02000200"},"N09":{"VG":"010200250702000200"},"N10":{"VG":"010300254902000100"}}}

**After this, the "Systemgerät" will send packages ("SYS") to the Weishaupt Gateway WEM-Modbus (polling ever 30 seconds):**  

{"ID":"12345678","SRC":"SYS","CAPI":{"NN":10,"N01":{"VG":"0203002529020002026e"},"N02":{"VG":"020300255102000100"},"N03":{"VG":"020700254100000100"},"N04":{"VG":"020700253200000200b6"},"N05":{"VG":"020700253302000200ab"},"N06":{"VG":"020700253700000200c2"},"N07":{"VG":"020901263900000100"},"N08":{"VG":"020201252c0200020190"},"N09":{"VG":"020201250702000201d0"},"N10":{"VG":"02040025010000028000"}}}

{"ID":"12345678","SRC":"SYS","CAPI":{"NN":7,"N01":{"VG":"020201255802000200d2"},"N02":{"VG":"020201256b02000202bc"},"N03":{"VG":"020201256a0200020258"},"N04":{"VG":"020201256902000201c2"},"N05":{"VG":"020201255902000201cc"},"N06":{"VG":"02040025020000020264"},"N07":{"VG":"020400252a02000400000000"}}}

{"ID":"12345678","SRC":"SYS","CAPI":{"NN":10,"N01":{"VG":"05010027f90200020001"},"N02":{"VG":"020100261f0200028000"},"N03":{"VG":"020100259c01000201cc"},"N04":{"VG":"020100259d0100020000"},"N05":{"VG":"02010026fe0200028000"},"N06":{"VG":"020100256202000113"},"N07":{"VG":"020100256203000134"},"N08":{"VG":"020200252c0200020190"},"N09":{"VG":"02020025070200020262"},"N10":{"VG":"020300254902000100"}}}


**Poosible declarations/meanings:**  
**DDC**:  
Direct Digital Control (commonly in HVAC/Building automation)  
Digital Data Conversion/Converter  
Direct Digital Control  
Data Device Corporation

**SRC**:  
Source  
Serial Reference Clock / Service Channel  

**CAPI**:  
CAN API 
CAN bus API 
controller area network API  
(surely not: Common ISDN Application Programming Interface)  
