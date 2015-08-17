## 源知净化器测试结果

### 设备端结果

#### 一、添加绑定流程
功能性测试正常，但因为该设备壳体为金属，加测smartlink成功率。
测试两组
    
    手机型号	测试次数	成功	失败	 配网时间
    小米2s	5	    5	 0	 12.10.15.17.16
    酷派	    5	    3	 2	 17.11.27.25.9

#### 二、注册登录等辅助模块
略

#### 三、设备指令测试

##### 开机（正常）  

    {"sn":21,"cmd":"upload","mac":"ACCF232C827A","data":["power::on"]}
    {"sn":22,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::on"]}
    {"sn":23,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":24,"cmd":"upload","mac":"ACCF232C827A","data":["mode::work"]}

##### 关机（正常）

    {"sn":26,"cmd":"upload","mac":"ACCF232C827A","data":["power::of"]}
    {"sn":27,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::of"]}

##### 定时开机 （正常）

    {"sn":28,"cmd":"upload","mac":"ACCF232C827A","data":["timeron::01"]}

##### 定时关机 （正常）

    {"sn":41,"cmd":"upload","mac":"ACCF232C827A","data":["timerof::01"]}

##### 定时开机下开机 （正常）

    {"sn":31,"cmd":"upload","mac":"ACCF232C827A","data":["power::on"]}
    {"sn":32,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":33,"cmd":"upload","mac":"ACCF232C827A","data":["mode::work"]}
    {"sn":35,"cmd":"upload","mac":"ACCF232C827A","data":["timeron::00"]}
    {"sn":34,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::on"]}        

 
##### 定时关机下关机 （正常）

    {"sn":43,"cmd":"upload","mac":"ACCF232C827A","data":["power::of"]}
    {"sn":44,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::of"]}
    {"sn":45,"cmd":"upload","mac":"ACCF232C827A","data":["timerof::00"]}

##### 开灯 （正常）

        {"sn":51,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::on"]}

##### 关灯 （正常）

        {"sn":51,"cmd":"upload","mac":"ACCF232C827A","data":["iaqled::of"]}

##### 自动模式 （此处 lv3状态无价值）

    {"sn":113,"cmd":"upload","mac":"ACCF232C827A","data":["mode::auto"]}
    {"sn":114,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":115,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv2"]}

##### 极速模式 （此处，lv1状态无价值，模式重复上报）

    {"sn":10,"cmd":"upload","mac":"ACCF232C827A","data":["mode::fast"]}
    {"sn":11,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv1"]}
    {"sn":12,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv5"]}
    {"sn":13,"cmd":"upload","mac":"ACCF232C827A","data":["mode::fast"]}

##### 工作模式 （此处，风速改变但未上报，重复登录）
    {"sn":16,"cmd":"upload","mac":"ACCF232C827A","data":["mode::work"]}
    {"sn":0,"cmd":"login","mac":"ACCF232C827A","pv":"1.0.7","hfver":"1.3","data":["vid::3","pid::40A3E","pkey::12345","dsn::12345678901234","usrver::V5.02"]}
    

##### 静音模式 （此处，此处 lv3状态无价值，模式重复上报）

    {"sn":5,"cmd":"upload","mac":"ACCF232C827A","data":["mode::mute"]}
    {"sn":6,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":7,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv1"]}
    {"sn":8,"cmd":"upload","mac":"ACCF232C827A","data":["mode::mute"]}


##### 风速测试 （重复上报，重复登录，模式上报三个问题）

风速1

    {"sn":10,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv1"]}

风速2 

    {"sn":2,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv2"]}
    {"sn":3,"cmd":"upload","mac":"ACCF232C827A","data":["mode::none"]}
    {"sn":4,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv2"]}
    {"sn":5,"cmd":"upload","mac":"ACCF232C827A","data":["mode::none"]}

风速3 （重复登录）

    {"sn":6,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":0,"cmd":"login","mac":"ACCF232C827A","pv":"1.0.7","hfver":"1.3","data":["vid::3","pid::40A3E","pkey::12345","dsn::12345678901234","usrver::V5.02"]}

风速4 （重复登录）

    {"sn":5,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv4"]}
    {"sn":6,"cmd":"upload","mac":"ACCF232C827A","data":["mode::none"]}
    {"sn":0,"cmd":"login","mac":"ACCF232C827A","pv":"1.0.7","hfver":"1.3","data":["vid::3","pid::40A3E","pkey::12345","dsn::12345678901234","usrver::V5.02"]}

风速5 （重复登录）

    {"sn":2,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv5"]}
     {"sn":0,"cmd":"login","mac":"ACCF232C827A","pv":"1.0.7","hfver":"1.3","data":["vid::3","pid::40A3E","pkey::12345","dsn::12345678901234","usrver::V5.02"]}

风速3 （为何上报模式，重复登录）

    {"sn":3,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":4,"cmd":"upload","mac":"ACCF232C827A","data":["mode::work"]}
    {"sn":5,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv3"]}
    {"sn":6,"cmd":"upload","mac":"ACCF232C827A","data":["mode::work"]}
    {"sn":0,"cmd":"login","mac":"ACCF232C827A","pv":"1.0.7","hfver":"1.3","data":["vid::3","pid::40A3E","pkey::12345","dsn::12345678901234","usrver::V5.02"]}


### <font style="color:red">频繁操作模式，风速，开关条件下 导致wifi模块掉线，然后上线</font>
连续操作模式，风速，开关，大概1s一次，即当设备刚做出变化时再做一次操作，此时发现wifi模块会掉一次线，分析应该是wifi模块掉电了一次，导致tcp连接断开，紧接着wifi模块又继电，然后设备上线。

以下为连续设置静音模式，自动模式设备上报的推送结果。

    {"sn":7,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv1"]}
    {"sn":8,"cmd":"upload","mac":"ACCF232C827A","data":["mode::mute"]}
    {"sn":9,"cmd":"upload","mac":"ACCF232C827A","data":["mode::auto"]}
    {"sn":10,"cmd":"upload","mac":"ACCF232C827A","data":["windspeed::lv1"]}
    {"sn":"8178416","mac":"ACCF232C827A","device_online":0}
    {"sn":"8178427","mac":"ACCF232C827A","device_online":1}

### 设备与App配合方面的问题

- 除了自动模式，其他模式在设备上无显示
- 定时设备上的步长值为0.5小时，而App上的步长值为1小时。


### 结论

1、显示问题：app与设备模式的显示不一致的问题，视情况而定，可忽略。
2、配合问题：步长值不一致的问题，建议统一。
3、状态上报问题：部分状态上报无意义，部分状态上报重复，建议改善。
4、重复登录动作：控制风速时会发生重复上报登录，建议分析一下原因，纠正。
5、wifi模块掉线：此问题非常严重，会导致app界面卡顿一下，以及状态与设备可能不同步，必须改善。

