##  Humix - start with Raspberry Pi 3 (1) Humix Think 設定
### Humix 主架構  ---- 分為兩大部份(think、sense)

>Humix-ng <br>
|--- Humix Think (以IBM Bluemix為基礎建立Humix的"大腦") <br>
|--- Humix Sense (為Humix的"小腦"，接收RPi的資訊並傳送至"大腦") <br>
    
#### Think & Sense
- Humix Think 其實就是一個位於Bluemix上的app,此app可結合Bluemix中的各個 API (模組化應用程式介面,ex. Speech to Text ... ),擁有儲存資料、翻譯語音等功能
- Humix Sense 則是在RPi 中接收"外界"資訊，並與"Think"做連結，將所接收到的資訊傳送到Bluemix上的app(Humix Think)

> **< Note >** <br> 
> 因為"Humix Think"是個app，所以一個think可搭配多個sence，也就是說，一個app可接收多個機器人(Humix)資訊的意思!! <br>

#### 軟體設定
-   Enable Humix Think  
    ##### step1.  申請/登入 Bluemix account (本機端) <br>
        登入bluemix後需更改"Region"。將Region設定在"美國南部"！！ <br>
<img border="0" height="303" src="https://1.bp.blogspot.com/-wnsU8Sj6xyI/Vw81z3pRSlI/AAAAAAAAABs/PtqygkrMWAowDsHq5ZqtZ5cmM_WLuc7-gCLcB/s400/IBM%2BBluemix%2B-region2.png" width="400" />

    ##### step2.  Humix Think 設定 (本機端) <br>
因為 humix think 主要運作於IBM Bluemix平台上，所以想要 enable humix的"大腦"，這個步驟要在本機端做操作。 <br>
a. 將"humix-ng" 從github clone到本機端後做相關設定。
<pre>git clone https://github.com/project-humix/humix-ng.git </pre>
b.  進入"think"資料夾， 更改 manifest.yml 檔案設定
進入"think"資料夾
<pre>cd humix-ng/think </pre>
    更改 manifest.yml 檔案設定，設定屬於自己的 application name ( the name must be unique ) (紅色框框為需要更改的部份) <br>
<pre>vim manifest.yml </pre>
 <img border="0" height="255" src="https://4.bp.blogspot.com/-DG2AZWai6XI/Vw9RbQ6jfBI/AAAAAAAAACA/Z-qpv-dcEncJl_QmZy2swW_GR8kqD83RACKgB/s400/humix-ng-think_manifest.png" width="400" />
    - example : <br>
    假設要創造一個名字叫"humix-pi2"的app作為Humix think的話,name以及host的設定就要設定為"humix-pi2",而services則是app選用的各個API,可以依照個人需求做新增及減少的動作。 <br>
 * example manifest.yml : <br>
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

c. < install cf-cli client > 
安裝 cf-cli後 ，可利用command line登入bluemix帳號及將創好的app發佈到bluemix上.
下載地址 : https://github.com/cloudfoundry/cli
