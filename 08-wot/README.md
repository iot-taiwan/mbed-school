# 第八章 WoT

WoT 是 IoT 的 Application Layer，並且是使用 Web 技術來打造 application。也就是說，IoT + Web-enabled technologies 就是 WoT。對 WoT 來說，最重要的觀念，就是以 URL 來表示 IoT 裝置；為 IoT 加入 URL 的觀念，就是 Google 提出的 Physical Web 計畫。

雖然 WoT 都是使用目前已經存在的軟體技術，但許多觀念都要重新思考，例如：Software Architecture、Application Framework 與 Composition Layer。

一個重新定義的 application 框架，或是 frontend 的 Composition layer 設計，可能會是 WoT 的關鍵技術。
本章節重點為 AutomationJS，這是利用 Virtual DOM 技術，來進行 UI Boundary Composition 的專案。

AutomationJS 是一個輕量級的 Composition Layer 實作，並且使用 Backbone 做為 Model-View 的基礎；未來也將接軌 HTML 5 的新技術標準－Shadow DOM。

有了 AutomationJS 後，差不多就解決了 WoT 的一個關鍵技術問題。本章將以一個實例，實一個符合 WoT 概念的 IoT 應用程式。重點技術如下：

* 使用 Dust Sensor、Temperature Sensor
* 實作 WebSocket Client
* 使用 WebSocket channel server
* 使用 AutomationJS 實作 Data Composition Layer
* 使用 AutomationJS 實作 Virtual DOM 技術

前面的章節，介紹了 ARM mbed 官方的 WebSocket channel server。本章將改用由 Mokoversity 自行開發的平臺：WoT.City。相關資訊可參考：

```
https://www.mokoversity.com/wotcity
```

## Dust Sensor 與 WebSocket Client

撰寫 ARM mbed 程式碼，以下範例結合第五章 Dust Sensor 及第七章 WebSocket Client。

程式碼如下：

```
#include "mbed.h"
#include "EthernetInterface.h"
#include "Websocket.h"

EthernetInterface *eth;
InterruptIn probe(p8);

Timer timer;
Ticker ticker;
DigitalOut led1(LED1);
Websocket ws("ws://wot.city/object/dust/send");
uint32_t low_time = 0;

void down();
void up();

void tick()
{
    char low_time_buf[256];
    char buf[256];
    sprintf( low_time_buf, "%f", low_time / (30.0 * 1000000));
    strcpy(buf, "{"); 
    strcat(buf, "\"lowpulseoccupancytime\":");
    strcat(buf, low_time_buf);
    strcat(buf, "}");
    ws.send(buf);
    low_time = 0;
   
}
 
void down()
{
    probe.rise(&up);
    led1 = 0;
    timer.start();
}

void up()
{
    led1 = 1;
    timer.stop();
    probe.fall(&down);
    
    low_time += timer.read_us();
    timer.reset();
}
int main() {
   
    probe.fall(&down);
    ticker.attach(tick, 30);

    eth = new EthernetInterface;
    eth->init(); //DHCP

    if (eth->connect()) return -1;
        
    
}
```

## Temperature Sensor 與 WebSocket Client

撰寫 ARM mbed 程式碼，以下範例結合第三章 Temperature Sensor 及第七章 WebSocket Client。

完整程式碼如下：

```
/* Grove - Temprature Sensor demo v1.0
*  This sensor detects the enviroment temprature,
*  Uses a Arch-Pro board with Grove Base Shield
*  Connect the Grove Temperature sensor to A0 and
*  connect a Grove 4 digit LED display to the UART connector
*  The temperature will be displayed in Celcius
*  Modified by David Bottrill from the original Arduino code
*  Reference: http://www.seeedstudio.com
*/

#include "mbed.h"
#include "DigitDisplay.h"
#include "WiflyInterface.h"
#include "Websocket.h"

EthernetInterface *eth;
DigitalOut myled(LED2);

DigitDisplay display(P4_29, P4_28);
AnalogIn ain(P0_23);

int a;
float temperature;
int B=3975;                                                         //B value of the thermistor
float resistance;
WiflyInterface wifly(p13, p14, p19, p26, "WWNet", "mmmmmmmm", WPA);
int main()
{
    wifly.init(); //Use DHCP
    //wifly.init("192.168.21.45","255.255.255.0","192.168.21.2");
    while (!wifly.connect());
    
    Websocket ws("ws://wot.city/object/temperature/send");
    while (!ws.connect());
    
    
    while(1) {
        char data[256];
        // multiply ain by 675 if the Grove shield is set to 5V or 1023 if set to 3.3V
        a=ain*675;
        resistance=(float)(1023-a)*10000/a;                         //get the resistance of the sensor;
        temperature=1/(log(resistance/10000)/B+1/298.15)-273.15;    //convert to temperature via datasheet ;
        
        wait(0.8);
        sprintf( data , "{ \"temperature\": %f }",temperature);
        ws.send(data);
    }
}
```
## WoT.City

第 7 章章提到 Websocket channel server 扮演封裝 IoT 物件的角色，對 Websocket server 來說，只要能定義好「channel」的結構，就能封裝數以萬計、千萬計的 IoT 物件。「抽像上來看，ARM mbed 仍然是 server 端」，就是這樣的觀念。

Wot.City 為 Mokoversity 開發，專為 ARM mbed 使用的 Websocket channel Server。

ARM mbed 裝置可使用以下的 URL，建立一個與 WebSocket Server 的連線：

```
ws://wot.city/object/[name]/send
```

*[name]* 的部份是 ARM mbed 裝置的名稱（IoT Object），為 IoT 裝置指定一個物件名稱，例如：

```
ws://wot.city/object/mbedtaiwan/send
```

接著，我們就可以透過網際網路將資料送到 WebSocket server。

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

使用 Websocket channel server 來封裝 IoT objects，讓 IoT devices 成為抽象化的 Websocket server。

## AutomationJS

前端使用 Backbone.js 實做 Virtual DOM。下圖為 Dust Sensor 及 Temperature Sensor 透過 Websocket 傳送 Sensor 數值到  Websocket channel server ，再由 Frontend 接收 Websocket channel server 的即時資料（經由 server push）後，顯示在網頁上。 Virtual DOM 讓下圖二個 Sensor 資料呈現區塊都是獨立的，當其中一個 Sensor 資料有變動時，只會更新單一區塊，而不是更新整個畫面。

![dashboard](http://i.imgur.com/BFturz1.png)
