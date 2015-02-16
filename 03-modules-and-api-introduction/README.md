# 第 3 章：Sensor 模組

第 3 章開始，將使用一些常見的 sensor 模組，來實做一些 IoT 的概念。以下先總覽幾個常用的 sensor 模組。所有模組皆可在 Grove Starter Kit for mbed 套件裡取得。

![圖 3.1：Seeed Studio 設計與生產的 Grove Starter Kit for mbed](1.3_grove-kit.jpg)

圖 3.1：Seeed Studio 設計與生產的 Grove Starter Kit for mbed

以下是 Grove Starter Kit for mbed 裡的 sensor 模組簡介。

### Grove - Button

按鈕模組包含一顆獨立的「瞬間開/關」按鈕。「瞬間」是意思是，當按鈕被按壓後，按鈕會立刻主動反彈。按下按鈕時，按鈕模組會使 GPIO 腳位得到一個 High 信號，反彈時則輸出 LOW 信號。

範例：

使用 Button 控制 LED ，按鈕被按壓後 LED 亮，反彈時則 LED 不亮。
```
#include "mbed.h"

DigitalOut myled(LED1);
DigitalIn button(p21);

int main()
{
    while(1){

        if (button)
            myled = 1 ;          
        else
            myled = 0;   

         wait(0.7); // simple debouncing      
    }

}

```
### Grove - Buzzer

蜂鳴器（Buzzer）模組的主要元件，是一個壓電式蜂鳴器。可以將蜂鳴器連接到 LPC1786 的數位輸出（Digital Output），當輸出為 HIGH 時發出聲響。

亦可將蜂鳴器連接到類比輸出，此時可讓蜂鳴器產生各種音效。

範例：

使用 Button 控制蜂鳴器，按鈕被按壓後蜂鳴器發出聲響，反彈時則蜂鳴器無發出聲響。
```
#include "mbed.h"

DigitalOut Buzzer(p20);
DigitalIn button(p21);

int main()
{
    while(1){

        if (button)
            Buzzer = 1 ;          
        else
            Buzzer = 0;   

         wait(0.7); // simple debouncing      
    }

}

```

### Grove - Rotary Angle Sensor 

旋轉角度感測器模組提供類比輸出介於 0 - Vcc，可偵測角度為 0 - 300 度。

範例：

使用 AnalogIn API 讀取旋轉角度。
```
AnalogIn RotaryAngle(A3);

int main()
{
    int value = 0 ;
    value = RotaryAngle;
}
```

### Grove - Temperature Sensor

溫度感測器模組使用溫敏電阻偵測環境溫度。當環境溫度上升時，溫敏電阻值將會下降。我們可以利用這個特性去計算出環境溫度。
感測器可偵測的範圍是 -40ºC - 125ºC，誤差為 ±1.5ºC。

範例：

使用 AnalogIn API 讀取 Temperature Sensor 的資料，依 Temperature Sensor 的 datasheet 轉換出溫度。

```
#include "mbed.h"

AnalogIn devic(p15);

int a;
float temperature;
int B=3975;                                                         //B value of the thermistor
float resistance;

int main()
{
    while(1) {

        a = devic*675; // multiply ain by 675 if the Grove shield is set to 5V or 1023 if set to 3.3V
        resistance=(float)(1023-a)*10000/a;                         //get the resistance of the sensor;
        temperature=1/(log(resistance/10000)/B+1/298.15)-273.15;    //convert to temperature via datasheet ;
        
        wait(0.8);
        
    }
}

```

### Grove - Light Sensor

光源感測器模組包含一個光敏電阻。當環境光源的強度上升時，光敏電阻值將會下降。也就是說，光亮時輸出訊號為 HIGH，黑暗時為 LOW。

範例：

使用 Light Sensor 控制 LED ，光亮時 LED 不亮，無光亮時則 LED 亮。
```
#include "mbed.h"

DigitalOut myled(LED1);
DigitalIn lighter(p21);

int main()
{
    while(1){

        if(lighter == 1)
            myled = 0 ;          
        else
            myled = 1;   

         wait(0.7); // simple debouncing      
    }

}

```
### Grove - Infrared Emitter

紅外線發射器模組，透過紅外線 LED 發射紅外線，可發射 10 米的紅外線訊號，超過 10 米，紅外線接收器可能接收不到訊號。

### Grove - Infrared Receiver

紅外線接收器模組，用來接收紅外線信號，可接收 10 米的紅外線信號，通常與紅外線發射器一起使用。

範例：
使用 RemoteIR Library，引入 *DigitDisplay.h* 標頭檔，來實做紅外線接收信號。
```

#include "mbed.h"
#include "ReceiverIR.h"
ReceiverIR ir_rx(p15);

RemoteIR::Format format;
uint8_t buf[32];
int bitcount;
int main() {
    while(1)
    {    
        if (ir_rx.getState() == ReceiverIR::Received) {
            bitcount = ir_rx.getData(&format, buf, sizeof(buf) * 8);
        }
    }
}
```

### Grove - Chainable RGB LED

全彩可變色 LED 模組，可透過程式改變 LED 顏色。

### Grove - 3-Axis Digital Accelerometer

數位三軸加速器模組，通常使用在需要偵測方向、手勢、動作的應用，能準確地反映物體的運動性質。

### 4 Digit Display

數字顯示器為顯示數字的電子元件。藉由 7 個 LED 以不同組合來顯示數字，所以稱為 7 段顯示器。4 位數字顯示器即可顯示 4 個數字，可當時鐘。

範例：
使用 DigitDisplay Library ，引入 *DigitDisplay.h* 標頭檔，來實做 4 位數字顯示器。
```
#include "mbed.h"
#include "DigitDisplay.h"

DigitDisplay display(p27, p28);
/**
 * RX pin - p27
 * TX pin - p28
 */

int main()
{
   display.write(30);
}

```

# 實作：溫度感測器搭配四位數顯示器

實作簡易環境溫度感測器並將即時監控的溫度顯示在 LED 數字顯示器上。

## 電子零件介紹

* Arch Pro 開發板 
* Base Shield 
* Grove Temperature 
* Grove 4-Digit-Display 
* Grove - Universal 4 Pin 20cm Unbuckled Cable

![圖1：溫度感測器實作採用零件 ](http://i.imgur.com/kxaUJKu.jpg)
圖1：溫度感測器實作採用零件

## 溫度感測器實作步驟

* 將 Base Shield 接在 Arch Pro 上 
* 溫度感測器接 Universal 4 pin Cable 線，並接在 Base Shield 的 A0 
* 將四位數 LED 顯示器接 Universal 4 pin Cable 線，並接在 Base Shield 的 UART
* 記得將 Base Shield 切換至 5V VCC 

![圖2：完成圖](http://i.imgur.com/uhJPzho.jpg)
圖2：溫度感測器組裝完成圖

## 撰寫程式碼
使用 DigitDisplay  Library ，引入 *DigitDisplay.h* 標頭檔，來實做 4 位數字顯示器，將 Temperature Sensor 讀取到的值依 Temperature Sensor 的 datasheet 轉換後，顯示在 LED 數字顯示器上。
```
#include "mbed.h"
#include "DigitDisplay.h"

DigitalOut myled(LED2);

DigitDisplay display(P4_29, P4_28);
AnalogIn ain(P0_23);

int a;
float temperature;
int B=3975;                                                         //B value of the thermistor
float resistance;

int main()
{
    while(1) {
// multiply ain by 675 if the Grove shield is set to 5V or 1023 if set to 3.3V
        a=ain*675;
        resistance=(float)(1023-a)*10000/a;                         //get the resistance of the sensor;
        temperature=1/(log(resistance/10000)/B+1/298.15)-273.15;    //convert to temperature via datasheet ;
        myled = 1;
        wait(0.8);
        myled = 0;
        wait(0.8);
        display.write(temperature);
    }
}

```
## 即時溫度監控顯示畫面 

由程式碼定義 Arch Pro 的第二顆 LED 紅燈閃爍來簡易判斷是否有正常執行運作。
![圖3：溫度監控即時顯示 LED 實作](http://i.imgur.com/AKgq0Qf.jpg)
圖3：溫度監控即時顯示 LED 實作

## 參考資源
* http://developer.mbed.org/users/djbottrill/code/Grove_Thermometer/
