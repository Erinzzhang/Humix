##  Humix - start with Raspberry Pi 3 (1) Humix Think 設定
### Humix 主架構  ---- 分為兩大部份(think、sense)

#### Humix-ng <br>
    |--- Humix Think (以IBM Bluemix為基礎建立Humix的"大腦") <br>
    |--- Humix Sense (為Humix的"小腦"，接收RPi的資訊並傳送至"大腦") <br>
    
#### Think & Sense
- Humix Think 其實就是一個位於Bluemix上的app,此app可結合Bluemix中的各個 API (模組化應用程式介面,ex. Speech to Text ... ),擁有儲存資料、翻譯語音等功能
- Humix Sense 則是在RPi 中接收"外界"資訊，並與"Think"做連結，將所接收到的資訊傳送到Bluemix上的app(Humix Think)

