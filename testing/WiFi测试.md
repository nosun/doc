��D�ڡ�
1) ��ѯ������Ϣ 0x01
�ظ�
pid::1, pkey::123
AA 00 11 01 70 69 64 3A 3A 32 00 70 6B 65 79 3A 3A 31 32 33 0A 

pid:1, pkey:123
AA 00 0F 01 70 69 64 3A 32 00 70 6B 65 79 3A 31 32 33 0A

pid:1, pkey::123
AA 00 10 01 70 69 64 3A 32 00 70 6B 65 79 3A 3A 31 32 33 0A

pid:1, pkey:123, usrver:1
AA 00 18 01 70 69 64 3A 32 00 70 6B 65 79 3A 31 32 33 00 75 73 72 76 65 72 3A 31 0A

pid::1, pkey::123, usr::1
AA 00 18 01 70 69 64 3A 3A 32 00 70 6B 65 79 3A 3A 31 32 33 00 75 73 72 3A 3A 31 0A

2) �ϱ�״̬ 0x03
switch::on, windspeed::high
AA 00 1B 03 73 77 69 74 63 68 3A 3A 6F 6E 00 77 69 6E 64 73 70 65 65 64 3A 3A 68 69 67 68 0A 

pw::1, pm::123, tm::3388
AA 00 17 03 70 77 3A 3A 31 00 70 6D 3A 3A 31 32 33 00 74 6D 3A 3A 33 33 38 38 0A 

pw:1, pm:123, tm:3388
AA 00 14 03 70 77 3A 31 00 70 6D 3A 31 32 33 00 74 6D 3A 33 33 38 38 0A 

3)����SmartLink

AA 00 01 07 0A 

AA 00 02 07 C8 0A 	��ʱ����Smartlinkģʽ
AA 00 02 07 C9 0A 	�����쳣

4)��ѯ����

AA 00 01 05 0A 

��T/L�ڡ�
1) �����豸
HF-A11ASSISTHREAD

2) ��¼
{"sn":1020,"cmd":"login"}
{"sn":0,"cmd":"login","ret":200,"heart":3}
{"sn":0,"cmd":"login","ret":200,"heart":30,["serverip"]:"ipaddr:port"}
{"sn":0,"cmd":"login","ret":200,"heart":3,"serverip":"192.168.1.135:9999"}

3) ״̬��ѯ
{"sn":1021,"cmd":"info"}

4) ����
{"sn":1022,"cmd":"heartbeat"}
{"sn":1022,"cmd":"heartbeat",["period"]:30}
{"sn":1022,"cmd":"heartbeat","heart":3}

5) �·�ָ��
{"sn":1023,"cmd":"download","data":["p::1"]}
{"sn":1023,"cmd":"download","data":["pw:1","pm:123","mm:1234"]}

6) ������
{"sn":1024,"cmd":"linknum"}

7) �̼�����
{"sn":1025,"cmd":"updatewifi","url":"http://yun.skyware.com.cn/download/firmware/LPB100_v1.1.6_W.bin"}

{"sn":1,"cmd":"updatewifi","url":"http://yun.skyware.com.cn/download/firmware/LPB100_2_v1.0.9_W.bin"}

8) AP����
{"sn":1026,"cmd":"config","wsssid":"XXX","wskey":"WPA2PSK,AES,*****"}

9) �ϴ�����
{"sn": 1027,"cmd": "silence",["switch"]: "on/off",["period"]: 30}
{"sn":1027,"cmd":"silence","switch":"on"}

10) upload�ظ�
{"sn":1024,"cmd":"upload","ret":200}

��S�ڡ�

MQTT����
{\"sn\":1567,\"cmd\":\"upload\",\"mac\":\"ACCF234DB278\",\"data\":[\"pw::0\",\"lc::1\",\"uv::0\",\"io::0\",\"mo::0\",\"fa::3\",\"tm::000\",\"fi::58556\",\"pm::0101\",\"th::2937\"]}

��ATָ�
SOCKB
AT+SOCKB=TCP,9999,d1.skyware.com.cn
AT+SOCKB=TCP,9999,182.92.148.183
AT+SOCKB=TCP,9999,172.27.35.6
AT+SOCKB=TCP,9999,192.168.1.133

SMARTLINK
AT+SMTLK

����
AT+UPURL=http://yun.skyware.com.cn/download/firmware/,LPB100_2_v1.0.4_W.bin