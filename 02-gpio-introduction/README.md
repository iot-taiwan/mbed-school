# 第 2 章：學習 ARM mbed GPIO 控制
GPIO 全名為 General Purpose Input/Output，是通用型輸入/輸出，所有與 sersor 的介面都是透過 GPIO 來控制，也就是透過 I/O PIN 腳來執行訊號的輸入或輸出，所以首先我們應該要學會如何使用 GPIO 這是很基礎也很重要的。

## 準備工作

### LPC1768 的 GPIO 腳位控制

本章將使用 ARM mbed 的 *DigitalOut* 巨集來控制 GPIO 腳位。以下是幾個基本觀念：

1. 選擇 GPIO Pinout。選擇 LPC1786 的 GPIO p21 來控制 LED 燈。參考 LPC1786 的 Pinout 圖，學習如何將 LED 連接至開發板。

2. 使用麵包板進行實驗。

3. 將 GPIO p21 定義為輸出：

```
DigitalOut myled(p21);
```

撰寫程式時，只要將 *mbed.h* 引入，即可使用 *DigitalOut* 巨集。

### 電子零件介紹

準備材料如下：

1. ARM mbed LPC 1768 開發版
2. 麵包板 x 1
3. 紅色導線 x 1，黑色導線 x 1
4. LED x 1

![圖 2.1：本章節使用的零件](http://i.imgur.com/5iD2ehn.jpg)

圖 2.1：本章節使用的零件

## LED 通電測試
我們先簡單的以 LED 來做通電測試，使用 LPC1768 開發版的 3.3 Regulated Out (VOUT) 的電源來讓 LED 通電。

依照以下步驟進行 LED 通電測試：

1. 將 LPC 1768 開發版插上麵包板
2. LED 有兩支針腳，一長一短，長的接正極，短的接負極
3. 紅色導線一端接到 LPC 1768 開發版 3.3v Regulated Out 的針腳，另一端接到麵包板上的正極
4. 黑色導線一端接到 LPC 1768 開發版 GND 的針腳，另一端接到麵包板上的負極
5. LED 針腳長端插上麵包板正極，短端插上麵包板負極

![圖 2.2：將 LPC 1768 開發版插上麵包板](http://i.imgur.com/2Nx0FSh.jpg)

圖 2.2：將 LPC 1768 開發版插上麵包板

![圖 2.3：LED 有兩支針腳，一長一短，長的接正極，短的接負極](http://i.imgur.com/GzFrp2W.jpg)

圖 2.3：LED 有兩支針腳，一長一短，長的接正極，短的接負極

![圖 2.4：紅色導線一端接到 LPC 1768 開發版 3.3v Regulated Out 的針腳，另一端接到麵包板上的正極](http://i.imgur.com/mJZWmMH.jpg)

圖 2.4：紅色導線一端接到 LPC 1768 開發版 3.3v Regulated Out 的針腳，另一端接到麵包板上的正極

![圖 2.5：黑色導線一端接到 LPC 1768 開發版 GND 的針腳，另一端接到麵包板上的負極](http://i.imgur.com/aszlXqG.jpg)

圖 2.5：黑色導線一端接到 LPC 1768 開發版 GND 的針腳，另一端接到麵包板上的負極

![圖 2.6：LED 針腳長端插上麵包板正極，短端插上麵包板負極](http://i.imgur.com/3Duw0rK.jpg)

圖 2.6：LED 針腳長端插上麵包板正極，短端插上麵包板負極

![圖 2.7：LED 點亮，測試成功](http://i.imgur.com/Iom5KME.jpg)

圖 2.7：LED 點亮，測試成功

## 實習：以 DigitalOut 控制 LED 

![圖 2.8：將 LPC 1768 開發版插上麵包板](http://i.imgur.com/2Nx0FSh.jpg)

圖 2.8：將 LPC 1768 開發版插上麵包板

![圖 2.9：紅色導線一端接到 LPC 1768 開發版 GND 針腳，另一端接到麵包板上的正極](http://i.imgur.com/LT0xdWQ.jpg)

圖 2.9：紅色導線一端接到 LPC 1768 開發版 GND 針腳，另一端接到麵包板上的正極

![圖 2.10：LED 針腳長端插上 PwmOut 21~26 任意一個針腳，針腳短端插上麵包板正極](http://i.imgur.com/99qiOYr.jpg)

圖 2.10：LED 針腳長端插上 PwmOut 21~26 任意一個針腳，針腳短端插上麵包板正極

4. 撰寫程式碼

撰寫第一個 ARM mbed 程式碼。首先，必須引入 *mbed.h* 標頭檔，接著將連接 LED 的 GPIO Pin 腳定義為數位輸出。ARM mbed 提供一個 *wait()* API，用來暫停程式碼的執行，可利用此 API 練習撰寫「LED 閃爍」程式。

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
