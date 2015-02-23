# 第 2 章：學習 ARM mbed GPIO 控制
GPIO 全名為 General Purpose Input/Output，是通用型輸入/輸出，所有與 sensor 溝通的介面都是透過 GPIO 來控制，也就是透過 I/O pin 來執行訊號的輸入或輸出，所以首先我們應該要學會如何使用 GPIO，這是很基礎也很重要的。

## 準備工作
### LPC1786 pinout
![圖 2.1：LPC1786 pinout](http://i.imgur.com/5vpCFjM.png)
### ARM mbed pin 名稱定義

ARM mbed 具體的 pin 名稱，如第一章用的 LED1、LED2、LED3、LED4 是在那裡定義的呢？ ARM mbed 的 pin 定義放在 *PinNames.h*，內容如下：

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
### LPC1768 的 GPIO 控制

本章將使用 ARM mbed 的 *DigitalOut* 巨集來控制 GPIO。以下是幾個基本觀念：

1. 選擇 GPIO Pinout。選擇 LPC1786 的 GPIO p21 來控制 LED。參考 LPC1786 的 Pinout 圖，學習如何將 LED 連接至開發板。

2. 使用麵包板進行實驗。

3. 將 GPIO p21 定義為輸出：

```
DigitalOut myled(p21);
```

撰寫程式時，只要將 *mbed.h* 引入，即可使用 *DigitalOut* 巨集。

### ARM mbed *DigitalOut* API 說明

我們在前面第一章 LED 的範例使用了 ARM mbed 的 *DigitalOut* API。在 LPC1768 開發版上，*DigitalOut* 可控制 P5-P30 pin。在 LPC1768 開發版的 pin 說明卡上，P5-P30 就是藍色標籤的 pin。
 
當 *DigitalOut* 的傳入值為 0 時，表示設定 pin 為 off；反之，傳入值為 1 時為 on。GPIO 有關的 API 整理如下：

* DigitalOut(PinName pin) 宣告 DistigalOut 的 pin 的連結

* void write(int value) 設定 pin 值

* int read() 讀取 pin 值

* DigitalOut& operator = (int value) 設定 pin 的值

* DigitalOut& operator = (DigitalOut& rhs) 設定 pin 的值為另一個 pin

* operator int() 讀取 pin 的簡易寫法

範例：

```
DigtialOut  myled(LED1); 
myled = 1;  // 也可以寫成 myled.write(1);
```

### 電子零件介紹

接下我們將做簡單的 LED 通電測試與撰寫第一個 ARM mbed 程式碼：「LED 閃爍」程式

準備材料如下：

1. ARM mbed LPC 1768 開發版
2. 麵包板 x 1
3. 紅色跳線 x 1，黑色跳線 x 1
4. LED x 1

![圖 2.1：本章節使用的零件](http://i.imgur.com/5iD2ehn.jpg)

圖 2.1：本章節使用的零件

## LED 通電測試
我們先簡單的以 LED 來做通電測試，使用 LPC1768 開發版的 3.3 Regulated Out (VOUT) 正極、 GND 負極的電源來讓 LED 通電。

依照以下步驟進行 LED 通電測試：

1. 將 LPC 1768 開發版插上麵包板
2. LED 有兩支針腳，一長一短，長的接正極，短的接負極
3. 紅色跳線一端接到 LPC 1768 開發版 3.3v Regulated Out 的 pin，另一端接到麵包板上的正極
4. 黑色跳線一端接到 LPC 1768 開發版 GND 的 pin，另一端接到麵包板上的負極
5. LED pin 長端插上麵包板正極，短端插上麵包板負極

![圖 2.2：將 LPC 1768 開發版插上麵包板](http://i.imgur.com/2Nx0FSh.jpg)

圖 2.2：將 LPC 1768 開發版插上麵包板

![圖 2.3：LED 有兩支 pin，一長一短，長的接正極，短的接負極](http://i.imgur.com/GzFrp2W.jpg)

圖 2.3：LED 有兩支 pin，一長一短，長的接正極，短的接負極

![圖 2.4：紅色跳線一端接到 LPC 1768 開發版 3.3v Regulated Out 的針腳，另一端接到麵包板上的正極](http://i.imgur.com/mJZWmMH.jpg)

圖 2.4：紅色跳線一端接到 LPC 1768 開發版 3.3v Regulated Out 的針腳，另一端接到麵包板上的正極

![圖 2.5：黑色跳線一端接到 LPC 1768 開發版 GND 的 pin，另一端接到麵包板上的負極](http://i.imgur.com/aszlXqG.jpg)

圖 2.5：黑色跳線一端接到 LPC 1768 開發版 GND 的 pin，另一端接到麵包板上的負極

![圖 2.6：LED 針腳 pin 插上麵包板正極，短端插上麵包板負極](http://i.imgur.com/3Duw0rK.jpg)

圖 2.6：LED 針腳 pin 插上麵包板正極，短端插上麵包板負極

![圖 2.7：LED 點亮，測試成功](http://i.imgur.com/Iom5KME.jpg)

圖 2.7：LED 點亮，測試成功

## 實習：以 *DigitalOut* 控制 LED 
接下來我們開始進行第一個 GPIO 控制，以 ARM mbed 的 *DigitalOut* 控制 pin 輸出高電平或者低電平來控制 LED。

依照以下步驟來以 *DigitalOut* 控制 LED：

1. 將 LPC1768 開發版插上麵包板
2. 紅色跳線一端接到 LPC 1768 開發版 GND pibn，另一端接到麵包板上的正極
3. LED pin 長端插上 p21~p26 任意一個 pin，pin 短端插上麵包板正極
4. 撰寫程式碼

![圖 2.8：將 LPC 1768 開發版插上麵包板](http://i.imgur.com/2Nx0FSh.jpg)

圖 2.8：將 LPC 1768 開發版插上麵包板

![圖 2.9：紅色跳線一端接到 LPC 1768 開發版 GND 針腳，另一端接到麵包板上的正極](http://i.imgur.com/LT0xdWQ.jpg)

圖 2.9：紅色跳線一端接到 LPC 1768 開發版 GND 針腳，另一端接到麵包板上的正極

![圖 2.10：LED 針腳長端插上 p21~p26 任意一個針腳，針腳短端插上麵包板正極](http://i.imgur.com/99qiOYr.jpg)

圖 2.10：LED 針腳長端插上 p21~p26 任意一個針腳，針腳短端插上麵包板正極

### 撰寫程式碼

撰寫第一個 ARM mbed 程式碼：「LED 閃爍」程式。首先，必須引入 *mbed.h* 標頭檔，接著將連接 LED 的 GPIO Pin 腳定義為數位輸出。ARM mbed 提供一個 *wait()* API，用來暫停程式碼的執行，可利用此 API 練習撰寫「LED 閃爍」程式。

程式碼如下：

```
#include "mbed.h"

DigitalOut myled(p21);

int main() {
    wait(0.8);
    
    while(1) {    
        myled = 1;
        wait(0.2);
        myled = 0;
        wait(0.2);
    }
}

```

本範例程式下載網址：

```
https://developer.mbed.org/users/mbedschool/code/ch2_mbed_led_control/
```

可在 mbed 的 online workspace 尋找 *mbedschool* 帳號，即可找到本書的所有範例。

![圖 2.11：測試成功](http://i.imgur.com/GV3hZjz.jpg)

圖 2.11：測試成功

## 延伸練習

動手做勞作，發揮想像力，即使只是簡單的 LED 閃爍程式，也能做出有趣的小東西。

![圖 2.12：LED 瓢蟲](http://i.imgur.com/zo38TF0.jpg)

圖 2.12：LED 瓢蟲
