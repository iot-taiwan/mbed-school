# 第 3 章：常用模組及 API 介紹

## 常用 Sensor 模組介紹

從第 3 章開始，將使用一些常見的 sensor 模組，來實做一些 IoT 的概念。以下先總覽幾個常用的 sensor 模組。

### Grove - Button

按鈕模組包含一顆獨立的「瞬間開/關」按鈕。「瞬間」是意思是，當按鈕被按壓後，按鈕會立刻主動反彈。按下按鈕時，按鈕模組會使 GPIO 腳位得到一個 High 信號，反彈時則輸出 LOW 信號。

### Grove - Buzzer

蜂鳴器（Buzzer）模組的主要元件，是一個壓電式蜂鳴器。可以將蜂鳴器連接到 LPC1786 的數位輸出（Digital Output），當輸出為 HIGH 時發出聲響。

亦可將蜂鳴器連接到類比輸出，此時可讓蜂鳴器產生各種音效。

### Grove - Rotary Angle Sensor 

簡介

旋轉角度感測器模組提供類比輸出介於 0 - Vcc，可偵測角度為 0 - 300 度。

### Grove - Temperature Sensor

簡介

溫度感測器模組使用溫敏電阻偵測環境溫度。當環境溫度上升時，溫敏電阻值將會下降。我們可以利用這個特性去計算出環境溫度。
感測器可偵測的範圍是 -40ºC - 125ºC，誤差為 ±1.5ºC。

### Grove - Light Sensor

簡介

光源感測器模組包含一個光敏電阻。當環境光源的強度上升時，光敏電阻值將會下降。也就是說，光亮時輸出訊號為 HIGH，黑暗時為 LOW。

### Grove - Infrared Emitter

簡介

紅外線發射器模組，透過紅外線 LED 發射紅外線，可發射 10 米的紅外線訊號，超過 10 米，紅外線接收器可能接收不到訊號。

### Grove - Infrared Receiver

簡介

紅外線接收器模組，用來接收紅外線信號，可接收 10 米的紅外線信號，通常與紅外線發射器一起使用。

### Grove - Chainable RGB LED

簡介

全彩可變色 LED 模組，可透過程式改變 LED 顏色。

### Grove - 3-Axis Digital Accelerometer

簡介

數位三軸加速器模組，通常使用在需要偵測方向、手勢、動作的應用，能準確地反映物體的運動性質。

### 4 Digit Display

簡介

數字顯示器為顯示數字的電子元件。藉由 7 個 LED 以不同組合來顯示數字，所以稱為 7 段顯示器。4 位數字顯示器即可顯示 4 個數字，可當時鐘。

## ARM mbed PIN 腳名稱定義

ARM mbed 具體的 PIN 腳名稱，如第一章用的 LED1、LED2、LED3、LED4 是在那裡定義的呢？ ARM mbed 的 PIN 腳定義放在 PinNames.h，定義如下：

```
typedef enum {
    // LPC Pin Names
    P0_0 = LPC_GPIO0_BASE,
          P0_1, P0_2, P0_3, P0_4, P0_5, P0_6, P0_7, P0_8, P0_9, P0_10, P0_11, P0_12, P0_13, P0_14, P0_15, P0_16, P0_17, P0_18, P0_19, P0_20, P0_21, P0_22, P0_23, P0_24, P0_25, P0_26, P0_27, P0_28, P0_29, P0_30, P0_31,
    P1_0, P1_1, P1_2, P1_3, P1_4, P1_5, P1_6, P1_7, P1_8, P1_9, P1_10, P1_11, P1_12, P1_13, P1_14, P1_15, P1_16, P1_17, P1_18, P1_19, P1_20, P1_21, P1_22, P1_23, P1_24, P1_25, P1_26, P1_27, P1_28, P1_29, P1_30, P1_31,
    P2_0, P2_1, P2_2, P2_3, P2_4, P2_5, P2_6, P2_7, P2_8, P2_9, P2_10, P2_11, P2_12, P2_13, P2_14, P2_15, P2_16, P2_17, P2_18, P2_19, P2_20, P2_21, P2_22, P2_23, P2_24, P2_25, P2_26, P2_27, P2_28, P2_29, P2_30, P2_31,
    P3_0, P3_1, P3_2, P3_3, P3_4, P3_5, P3_6, P3_7, P3_8, P3_9, P3_10, P3_11, P3_12, P3_13, P3_14, P3_15, P3_16, P3_17, P3_18, P3_19, P3_20, P3_21, P3_22, P3_23, P3_24, P3_25, P3_26, P3_27, P3_28, P3_29, P3_30, P3_31,
    P4_0, P4_1, P4_2, P4_3, P4_4, P4_5, P4_6, P4_7, P4_8, P4_9, P4_10, P4_11, P4_12, P4_13, P4_14, P4_15, P4_16, P4_17, P4_18, P4_19, P4_20, P4_21, P4_22, P4_23, P4_24, P4_25, P4_26, P4_27, P4_28, P4_29, P4_30, P4_31,
 
    // mbed DIP Pin Names
    p5 = P0_9,
    p6 = P0_8,
    p7 = P0_7,
    p8 = P0_6,
    p9 = P0_0,
    p10 = P0_1,
    p11 = P0_18,
    p12 = P0_17,
    p13 = P0_15,
    p14 = P0_16,
    p15 = P0_23,
    p16 = P0_24,
    p17 = P0_25,
    p18 = P0_26,
    p19 = P1_30,
    p20 = P1_31,
    p21 = P2_5,
    p22 = P2_4,
    p23 = P2_3,
    p24 = P2_2,
    p25 = P2_1,
    p26 = P2_0,
    p27 = P0_11,
    p28 = P0_10,
    p29 = P0_5,
    p30 = P0_4,
 
    // Other mbed Pin Names
#ifdef MCB1700
    LED1 = P1_28,
    LED2 = P1_29,
    LED3 = P1_31,
    LED4 = P2_2,
#else
    LED1 = P1_18,
    LED2 = P1_20,
    LED3 = P1_21,
    LED4 = P1_23,
#endif
    USBTX = P0_2,
    USBRX = P0_3,
 
    // Arch Pro Pin Names
    D0 = P4_29,
    D1 = P4_28,
    D2 = P0_4,
    D3 = P0_5,
    D4 = P2_2,
    D5 = P2_3,
    D6 = P2_4,
    D7 = P2_5,
    D8 = P0_0,
    D9 = P0_1,
    D10 = P0_6,
    D11 = P0_9,
    D12 = P0_8,
    D13 = P0_7,
    D14 = P0_27,
    D15 = P0_28,
 
    A0 = P0_23,
    A1 = P0_24,
    A2 = P0_25,
    A3 = P0_26,
    A4 = P1_30,
    A5 = P1_31,
 
    I2C_SCL = D15,
    I2C_SDA = D14,
 
    // Not connected
    NC = (int)0xFFFFFFFF
} PinName;
```

## ARM mbed API 說明

我們在前面二章範例 LED 燈的點亮使用了 ARM mbed 的 DigitalOut 的 API 功能
在 LPC768 的開發版上，DigitalOut 可使用在 LPC1768 開發版 PIN 腳說明上所有藍色標籤的 PIN 腳 （p5-p30），DigitalOut 的值 0 為 off，1 為 on，API 使用說明如下：

* DigitalOut(PinName pin) 宣告 DistigalOut 的 PIN 腳的連結

* void write(int value) 設定 PIN 腳值

* int read() 讀取 PIN 腳值

* DigitalOut& operator = (int value) 設定 PIN 腳的值

* DigitalOut& operator = (DigitalOut& rhs) 設定 PIN 腳的值為另一個 PIN 腳

* operator int() 讀取 PIN 腳的簡易寫法

範例：

```
DigtialOut  myled(LED1); 
myled = 1;  // 也可以寫成 myled.write(1);
```
 
