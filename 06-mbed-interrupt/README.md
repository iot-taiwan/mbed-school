# 第 6 章：ARM mbed 中斷處理

本章針對上一章涵蓋的技術作進一步的解說。上一章我們定義一個 REST API，讓 Client 端（此為 Browser）以 HTTP
GET request 的方式呼叫 REST API，取得 Server 端（此為 Grove - Dust Sensor）感測到的數值。
這種把裝置視為網路上的某項資源，然後透過 URL 存取該資源的形式，正是第 4 章所提到的 WoT（Physical Web）觀念的初步實作。

此外，上一章也使用到一個常見的技術，稱為“中斷”。以下會逐一說明中斷的觀念和 ARM mbed 中斷系統。

## 中斷是什麼！？

根據維基百科，中斷（Interrupt）是指處理器接收到來自硬體或軟體的信號，提示發生了某個事件，應該被注意，這種情況就稱為中斷。

## ARM mbed 中斷系統

ARM mbed 中斷系統可以偵測 pin 值的變化、計時器時間的變化以及通訊狀態的變化，實作的類型一般可以分成兩種：

* 輪詢（Polling）
* 中斷（Interrupt）

以下透過程式碼範例說明這兩種實作類型的差異：

以輪詢的方式實作中斷事件的處理
```
#include "mbed.h"
 
DigitalIn enable(p5);
DigitalOut led(LED1);
 
int main() {
    while(1) {
        if(enable) {
            led = !led;
        }
        wait(0.25);
    }
}
```
從程式碼可看出透過不停檢查 p5 pin 的輸入變化來處理中斷事件

* DigitalIn
 * LPC1768 開發板編號 p5～p30 的 pin 都可以使用 DigitalIn 介面，
也就是小卡片上標示為藍色的部份。當需要讀取某一支數位輸入 pin 的數值時，會使用此介面。

以中斷的方式實作中斷事件的處理
```
#include "mbed.h"
 
InterruptIn button(p5);
DigitalOut led(LED1);
DigitalOut flash(LED4);
 
void flip() {
    led = !led;
}
 
int main() {
    button.rise(&flip);  // attach the address of the flip function to the rising edge
    while(1) {           // wait around, interrupts will interrupt this!
        flash = !flash;
        wait(0.25);
    }
}
```
從程式碼可看出當 p5 pin 的輸入發生變化時會觸發中斷

* InterruptIn
 * 當某一支數位輸入 pin 的數值發生改變時，InterruptIn 介面會觸發一個中斷事件。
 * 除了 LPC1768 開發版編號 p19 和 p20 的 pin，其它編號的 pin 皆可以使用 InterruptIn 介面。

## 基於計時器的中斷

此類中斷也是很常見的應用，一般分為兩類

* Ticker（repeating interrupt）
* Timeout（one-time interrupt）

### Ticker

Ticker 介面被用來設定重複性的中斷，在指定的週期內重複地執行某個功能（呼叫某個函數）。

### Timeout

Timeout 介面被用來設定一次性的中斷，在指定的週期內執行某個功能（呼叫某個函數）。

## 範例

最後我們透過上一章的空氣品質偵測器範例解說如何在某個時間範圍內，設定中斷事件觸發的條件，取得我們要的數值。

### 程式碼解說

```
void down() {
    probe.rise(&up); // 當電壓發生由 LOW 狀態轉移到 HIGH 狀態時，呼叫 “up” function
    timer.start(); // 開始計時
}

void up() {
    timer.stop(); // 結束計時
    probe.fall(&down); // 當電壓發生由 HIGH 狀態轉移到 LOW 狀態時，呼叫 “down” function
    
    low_time += timer.read_us(); // 取得已經歷的時間（毫秒）
    timer.reset(); // 將計時器歸零
}

int main() {
    ...
    probe.fall(&down) // 當電壓發生由 HIGH 狀態轉移到 LOW 狀態時，呼叫 “down” function
    ticker.attach(tick, 30); // 指定中斷事件觸發時要執行的動作（呼叫 “tick” function），以及特定時間區間（本範例為 30 秒）
}
```
