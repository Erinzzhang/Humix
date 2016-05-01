#  Humix - start with Raspberry Pi 3 

#### Humix 主架構  ---- 分為兩大部份(think、sense)

>Humix-ng <br>
|--- Humix Think (以IBM Bluemix為基礎建立Humix的"大腦") <br>
|--- Humix Sense (為Humix的"小腦"，接收RPi的資訊並傳送至"大腦") <br>
    
#### Think & Sense
- Humix Think 其實就是一個位於Bluemix上的app,此app可結合Bluemix中的各個 API (模組化應用程式介面,ex. Speech to Text ... ),擁有儲存資料、翻譯語音等功能
- Humix Sense 則是在RPi 中接收"外界"資訊，並與"Think"做連結，將所接收到的資訊傳送到Bluemix上的app(Humix Think)

> **< Note >** <br> 
> 因為"Humix Think"是個app，所以一個think可搭配多個sence，也就是說，一個app可接收多個機器人(Humix)資訊的意思!! <br>

##1.    Humix Think 設定

-   Enable Humix Think  <br>

##### step1.  申請/登入 Bluemix account (本機端) <br>
登入bluemix後需更改"Region"。將Region設定在"美國南部"！！ <br>
<img border="0" height="303" src="https://1.bp.blogspot.com/-wnsU8Sj6xyI/Vw81z3pRSlI/AAAAAAAAABs/PtqygkrMWAowDsHq5ZqtZ5cmM_WLuc7-gCLcB/s400/IBM%2BBluemix%2B-region2.png" width="400" /> <br>

##### step2.  Humix Think 設定 (本機端) <br>
因為 humix think 主要運作於IBM Bluemix平台上，所以想要 enable humix的"大腦"，這個步驟要在本機端做操作。 <br>
a. 將"humix-ng" 從github clone到本機端後做相關設定。
<pre>git clone https://github.com/project-humix/humix-ng.git </pre>
b.  進入"think"資料夾， 更改 manifest.yml 檔案設定
進入"think"資料夾
<pre>cd humix-ng/think </pre>
    更改 manifest.yml 檔案設定，設定屬於自己的 application name ( the name must be unique ) (紅色框框為需要更改的部份) <br>
<pre>vim manifest.yml </pre>
 <img border="0" height="255" src="https://4.bp.blogspot.com/-DG2AZWai6XI/Vw9RbQ6jfBI/AAAAAAAAACA/Z-qpv-dcEncJl_QmZy2swW_GR8kqD83RACKgB/s400/humix-ng-think_manifest.png" width="400" /> <br>
 <br>
    **example :** <br>
    假設要創造一個名字叫"humix-pi2"的app作為Humix think的話,name以及host的設定就要設定為"humix-pi2",而services則是app選用的各個API,可以依照個人需求做新增及減少的動作。 <br>
    example manifest.yml : <br>
``` 
applications:
- path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: humix-pi2
      host: humix-pi2
      disk_quota: 1024M
      services:
      - Humix-Cloudant-Service
      - Humix-Dialog-Service
      - Humix-NLC-Service
      - Humix-Speech-Service
      command: node --max-old-space-size=384 app.js --settings ./bluemix-settings.js -v 
 ```
> **< Note > 命名的限制！！**<br>
ex.  "humix-pi2" 
~~" humix_pi2 "~~  <br>
> <img border="0" height="261" src="https://4.bp.blogspot.com/-FnDlP-R_Pys/VxEkp35cuEI/AAAAAAAAAGQ/VK_fj1syLIcVxQPc9qwWPf-H_lXTbWi_gCLcB/s400/humix-name.png" width="400" />

c. < install cf-cli client >  <br>
安裝 cf-cli後 ，可利用command line登入bluemix帳號及將創好的app發佈到bluemix上. <br>
下載地址 : https://github.com/cloudfoundry/cli <br>

d. 執行 deployThink.sh <br>
<pre>./deployThink.sh</pre>
登入bluemix 帳號，選擇space (只有一個space時不用選)
```
    API endpoint: https://api.ng.bluemix.net
    Email> liuch@tw.ibm.com
    Password>

    Authenticating...
    OK
    Targeted org liuch@tw.ibm.com
    Select a space (or press enter to skip):
    1. dev
    2. demo
    3. personal
    
    Space> 1

    Targeted space dev
    
    API endpoint:   https://api.ng.bluemix.net (API version: 2.40.0)
    User:           liuch@tw.ibm.com
    Org:            liuch@tw.ibm.com
    Space:          dev
    Creating service instance Humix-Cloudant-Service in org liuch@tw.ibm.com / space dev as liuch@tw.ibm.com... 
```
e. 確認剛剛建立的"humix-think"可以運作 <br>
**http://< your_app_name >.mybluemix.net** <br>
如果成功了，可以看到下面的網頁畫面,同時，也可以先添加好Humix的SenceID <br>
<img border="0" height="227" src="https://3.bp.blogspot.com/-ntpV9i7u44g/VxEyXVlCufI/AAAAAAAAAG4/dSGYiqs_ZGIpSqAPBB2aHZlZyt9NkjKgwCLcB/s400/humix-pi2-addsense.png" width="400" />

成功將humix的"大腦"(app)送到bluemix平台之後，接著就是在RPi 3登場的時刻！<br>

## 2.   Humix Sense設定 
- Enable Humix Sense <br>

##### step1. 下載並解壓縮"humix.img"至SD card (RPi 3) <br>
下載humix的映像檔，並解壓縮燒錄至 SD card 中(映像檔包含RPi 3的作業系統Raspbian Jessie、humix-ng) <br>
下載地址：[humix.img](http://119.81.185.45/humix_image/20160330-humix-jessie-alpha.img.gz) <br>
##### step2. 連線登入RPi 3 (RPi 3) <br>
接著，利用USB-Serial-Cable ，連線登入Pi 3。 <br>
使用者"pi"，密碼為"raspberry" <br>
> < Note >第一次登入時，會發現有亂碼的產生，Why?!! <br>
 REF: 
- [常見問與答] 解決從序列埠登入到 Pi 3 的亂碼問題 <br>
https://www.raspberrypi.com.tw/10842/raspberry-pi-3-uart-overlay-workaround/   <br>
- RPi wi-fi 設定: 
[[基礎] 命令列設置無線網路](https://www.raspberrypi.com.tw/2152/setting-up-wifi-with-the-command-line/)

Connect Wi-fi in RPI
<pre>sudo vi /etc/wpa_supplicant/wpa_supplicant.conf</pre>
進入wpa_supplicant.conf後，添加設定如下
```
network={
  ssid="AP_name"             # 基地台名稱
  psk="AP_password"          # 密碼
  proto=RSN                  # WPA2 加密協定
  pairwise=CCMP              # WPA2 加密協定
  key_mgmt=WPA-PSK           # WPA2 金鑰管理協定
  auth_alg=OPEN              # WPA2 認證演算法
}
```
重新啟動無線網卡
<pre>sudo ifdown wlan0
sudo ifup wlan0 </pre>
查看RPi 的IP
<pre>ifconfig </pre>
利用[Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)設定ip，以 wi-fi 重新連線進入RPi 3 <br>

成功登入Pi 3完成wi-fi連線的設定後，就可以開始設定humix sense了！ <br>
##### step3. 更改 Humix Sense Config 設定 (RPi 3)
<pre>cd ~/humix-ng/sense/
vi config.js </pre>
**example:**
```
    module.exports = {
        thinkURL : 'http://humix-pi2.mybluemix.net/',
        senseId  : 'humix-pi2'
    }
```
> **< Note > 取得thinkURL,senseId** <br>
<img border="0" height="227" src="https://2.bp.blogspot.com/-Ws14YIG-g-w/VxE0B_3oB3I/AAAAAAAAAHI/8R00fLCjrvYRhdLCsXL0estBwwt8XiytQCLcB/s400/humix-pi2-config.png" width="400" /> <br>

##### step4. 更改 Watson Speech Recognition Credentials (紅色框框為需要更改的部份) 
<pre>cd ~/humix-ng/sense/modules/core/humix-dialog-module/lib/ 
vi config.js</pre> 
<img border="0" height="255" src="https://4.bp.blogspot.com/-DLIadhPYcgU/Vw95gLGNfkI/AAAAAAAAAC8/gSkUB4RErfASbhQ8Bx1KybxyiaS4EL0tACLcB/s400/IBM%2BBluemix%2B-watson.png" width="400" /> <br>
> **< Note > 到 bluemix 取得 username,passwd** <br>
<img border="0" height="195" src="https://1.bp.blogspot.com/-zrmCCnDEXGw/Vw99ew2egWI/AAAAAAAAADg/FCckacR_BfoIIUx4s1qEvPScVAi7IYBLwCLcB/s320/IBM%2BBluemix%2Bapp2.png" width="320" /> <br>
<img border="0" height="195" src="https://3.bp.blogspot.com/-vWl2kRxMyek/Vw97S0Yis6I/AAAAAAAAADM/Va-5-Jb8OdAsMpiPL26sySTLsXxs-y90ACLcB/s320/IBM%2BBluemix%2B-environment.png" width="320" /> <br>
<img border="0" height="195" src="https://3.bp.blogspot.com/-BQOqL-H3xNc/Vw9-Cvod2gI/AAAAAAAAADo/cNa0HT6Qp_4ektVlPy3iuxTy3_I43p0XACLcB/s320/humix-ng-think_pws.png" width="320" /> <br>

##### step5. 執行 Humix Sense. <br> 
設定好麥克風及喇叭後，到"sense"輸入 $ npm start 執行 Humix Sense 
<pre> cd ~/humix-ng/sense 
 npm start</pre>
 <img border="0" height="255" src="https://1.bp.blogspot.com/--SaSvdNwxAc/VxDCiCZr2YI/AAAAAAAAAEU/qii75kWgaG46QD--q2HGQ-ihNE-v-MefwCLcB/s400/pi--humix-ng-sense.png" width="400" /> <br>
 
 看到以上結果，代表sense成功連結到think(Humix 大腦)了！ <br>
