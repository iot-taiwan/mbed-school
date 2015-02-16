# 第八章 WoT

WoT 是 IoT 的 Application Layer，並且是使用 Web 技術來打造 application。也就是說，IoT + Web-enabled technologies 就是 WoT。對 WoT 來說，最重要的觀念，就是以 URL 來表示 IoT 裝置；為 IoT 加入 URL 的觀念，就是 Google 提出的 Physical Web 計畫。

雖然 WoT 都是使用目前已經存在的軟體技術，但許多觀念都要重新思考，例如：Software Architecture、Application Framework 與 Composition Layer。

本章節重點為 AutomationJS，這是利用 Virtual DOM 技術，來進行 UI Boundary Composition 的專案。

AutomationJS 是一個輕量級的 Composition Layer 實作，並且使用 Backbone 做為 Model-View 的基礎；未來也將接軌 HTML 5 的新技術標準－Shadow DOM。

有了 AutomationJS 後，差不多就解決了 WoT 的一個關鍵技術問題。本章將實做完整 WoT 整合，整合內容如下：

Block #1：Dust Sensor + WebSocket Client 及 Temperature Sensor + WebSocket Client
Block #2：Websocket channel server
```
https://www.mokoversity.com/wotcity
```
Block #3 ：AutomationJS
Frontend
Backbone with Virtual DOM

##  Dust Sensor + WebSocket Client 及 Temperature Sensor + WebSocket Client

##  Websocket channel server

### WoT.City

Websocket channel for ARM mbed.

## How to use it

ARM mbed 裝置利用下方的 URL，建立一個與 server 溝通的連線，這裏的 server 指得是 Websocket server。接著，我們就可以透過網際網路將資料送到 Webssocket server。

```
ws://wot.city/object/[name]/send
```

我們必須為這個連線指定一個物件名稱，例如：

```
ws://wot.city/object/mbedtaiwan/send
```

打開瀏覽器，網頁就能利用以下的 URL 建立一個跟 Websocket server 的連線，並且觀看從 Websocket server 接收過來的資料。

```
ws://wot.city/object/[name]/viewer
```

同樣地，我們必須要指定一個物件名稱，例如：

```
ws://wot.city/object/mbedtaiwan/viewer
```

一個 ARM mbed 物件擁有兩種特殊的資源可以使用，分別是 send 和 viewer。
站在 Websocket server 的角度，每個 Websocket client 使用 send 把資料串流透過 Websocket 連線傳送到 server。
viewer 則是 server 把資料串流透過 Websocket server 傳送給 client。

## AutomationJS

前端使用 Backbone.js 實做 Virtual DOM。

結果如下圖：

![dashboard](http://i.imgur.com/BFturz1.png)
