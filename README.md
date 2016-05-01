##  Humix - start with Raspberry Pi 3 (1) Humix Think 設定
### Humix 主架構  ---- 分為兩大部份(think、sense)

>Humix-ng <br>
|--- Humix Think (以IBM Bluemix為基礎建立Humix的"大腦") <br>
|--- Humix Sense (為Humix的"小腦"，接收RPi的資訊並傳送至"大腦") <br>
    
#### Think & Sense
- Humix Think 其實就是一個位於Bluemix上的app,此app可結合Bluemix中的各個 API (模組化應用程式介面,ex. Speech to Text ... ),擁有儲存資料、翻譯語音等功能
- Humix Sense 則是在RPi 中接收"外界"資訊，並與"Think"做連結，將所接收到的資訊傳送到Bluemix上的app(Humix Think)

> < Note > <br> 
> 因為"Humix Think"是個app，所以一個think可搭配多個sence，也就是說，一個app可接收多個機器人(Humix)資訊的意思!! <br>

#### 軟體設定
-   Enable Humix Think  
    step1.  申請/登入 Bluemix account (本機端) <br>
        登入bluemix後需更改"Region"。將Region設定在"美國南部"！！ <br>
<tr><a href="https://1.bp.blogspot.com/-wnsU8Sj6xyI/Vw81z3pRSlI/AAAAAAAAABs/PtqygkrMWAowDsHq5ZqtZ5cmM_WLuc7-gCLcB/s1600/IBM%2BBluemix%2B-region2.png" imageanchor="1" ><img border="0" height="303" src="https://1.bp.blogspot.com/-wnsU8Sj6xyI/Vw81z3pRSlI/AAAAAAAAABs/PtqygkrMWAowDsHq5ZqtZ5cmM_WLuc7-gCLcB/s400/IBM%2BBluemix%2B-region2.png" width="400" /></a></tr>
