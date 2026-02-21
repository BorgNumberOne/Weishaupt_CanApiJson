## Weishaupt CanApiJson
Weishaupt CanApiJson - CAN-BUS-like protocol via JSON between:  
"**Systemgerät**" (48301122172, 48301122242, 48301122512, 48301122522) and  
"**Gateway WEM-Modbus**" (Weishaupt 48300002722)

### Weishaupt Systemgerät - hardware names (possible):  
WEM-Systemgerät 2.5 -- Weishaupt Ersatzteil komplett mit SD-Karte - ersetzt 48301122172, 48301122242, 48301122512  
Weishaupt WEM-Systemgerät 2.6 kpl. mit SD-Card 48301122522

### MAC address(es) (possible):
00:23:7e:??:??:?? [Elster_??:??:??]

### Host name and address(es):
hostname: WEM-SG  
http://wem-sg (sg = Systemgerät)  
http://admin:Admin123@wem-sg  
http://wem-sg/script/einstellung.js  
http://wem-sg/script/Form_eth_log.js  
http://wem-sg/script/ajax.js  
http://admin:Admin123@wem-sg/ajax/CanApiJson.json -->JSON content changes every 30 seconds (polling).  
http://wem-sg/sd/systable.csv --> could be figured out with the help of network package sniffing between "Systemgerät" and "Gateway WEM-Modbus"  

### Gateway WEM-Modbus (Weishaupt 48300002722)  

### MAC addresses (possible):  
d0:76:50:??:??:?? [TAPKOTechnol_??:??:??]  

### Host name and address(es):  
hostname: MOD-WHGW-?????? (?????? = second part of the MAC address)  
http://MOD-WHGW-??????:8080

**After setting up the Weishaupt Gateway WEM-Modbus, the device will send a package ("DDC") to the "Systemgerät":**  

POST /ajax/CanApiJson.json HTTP/1.1  
Host: <IP of the "Systemgerät">  
Authorization: Basic YWRtaW46QWRtaW4xMjM= (Base64)  
Referer: http://<IP of the "Systemgerät"> /  
Content-Length: 386  
Connection: keep-alive  

{"ID":"12345678","SRC":"DDC","CAPI":{"NN":10,"N01":{"VG":"01010027f902000200"},"N02":{"VG":"010100261f02000200"},"N03":{"VG":"010100259c01000200"},"N04":{"VG":"010100259d01000200"},"N05":{"VG":"01010026fe02000200"},"N06":{"VG":"010100256202000100"},"N07":{"VG":"010100256203000100"},"N08":{"VG":"010200252c02000200"},"N09":{"VG":"010200250702000200"},"N10":{"VG":"010300254902000100"}}}

**After this, the "Systemgerät" will send packages to de Gateway WEM-Modbus:** (polling ever 30 seconds)  
{"ID":"12345678","SRC":"SYS","CAPI":{"NN":10,"N01":{"VG":"0203002529020002026e"},"N02":{"VG":"020300255102000100"},"N03":{"VG":"020700254100000100"},"N04":{"VG":"020700253200000200b6"},"N05":{"VG":"020700253302000200ab"},"N06":{"VG":"020700253700000200c2"},"N07":{"VG":"020901263900000100"},"N08":{"VG":"020201252c0200020190"},"N09":{"VG":"020201250702000201d0"},"N10":{"VG":"02040025010000028000"}}}

{"ID":"12345678","SRC":"SYS","CAPI":{"NN":7,"N01":{"VG":"020201255802000200d2"},"N02":{"VG":"020201256b02000202bc"},"N03":{"VG":"020201256a0200020258"},"N04":{"VG":"020201256902000201c2"},"N05":{"VG":"020201255902000201cc"},"N06":{"VG":"02040025020000020264"},"N07":{"VG":"020400252a02000400000000"}}}

{"ID":"12345678","SRC":"SYS","CAPI":{"NN":10,"N01":{"VG":"05010027f90200020001"},"N02":{"VG":"020100261f0200028000"},"N03":{"VG":"020100259c01000201cc"},"N04":{"VG":"020100259d0100020000"},"N05":{"VG":"02010026fe0200028000"},"N06":{"VG":"020100256202000113"},"N07":{"VG":"020100256203000134"},"N08":{"VG":"020200252c0200020190"},"N09":{"VG":"02020025070200020262"},"N10":{"VG":"020300254902000100"}}}

