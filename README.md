# FTP-Proxy

* 運行方式
```
gcc ftp_proxy.c -o proxy ./proxy <ProxyIP> <ProxyPort> <Rate>
```

* Proxy 是在 FTP Client 與 FTP Server 間進行數據轉送的媒介，而我們要控制 Server 下 載到 Client 與 Client 上傳到 Server 的傳輸速度
* 關於速度控制的部分，我們將封包傳輸 時間、封包大小以及想控制的速度傳入 rate_control function 中
* 因為系統過快，也就是 實際速度比期望傳輸速度還大時，我們就利用 usleep()這個函數，讓系統 delay 來配合上傳及下載的速度
* 當上傳檔案時，藉由 Read from Client 可以得出已知傳輸時間以及封包 大小，並根據公式 “特定速度下所花費時間= 封包大小/速度“，我們可以計算出目前速度與當前速度時間的分別時間，相減之後即是想要 delay 的時間差
* 但測試後發現單單只讓系統 usleep 這個值是不夠的，因此我們決定再乘上一特定係數來降低平均速率，而這係數也是我們自己測試出來最符合的值

* 流程圖
![](https://imgur.com/wCx4rV5)

* 速度控制示意圖
![](https://imgur.com/uGb1RxG)
