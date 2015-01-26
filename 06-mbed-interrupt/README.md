# 第 6 章：ARM mbed 的中斷應用

本章節針對上一章節涵蓋的技術作進一步的解說。上一章節我們定義一個 REST API，讓 Client side（此為 Browser）以 HTTP GET request 的方式取得 Server side（此為 Dust sensor）感測到的數值。這種把裝置視為網路上的某項資源，然後透過 URL 存取該資源的形式，正是第 4 章所提到的 WoT（Physical Web）觀念的初步實作。

## DigitalIn

LPC1768 開發版編號 p5～p30 的 mbed pin 都可以當成 DigitalIn 介面使用，也就是小卡片上標示為藍色的部份。當需要讀取某一支數位輸入 pin 的值時，會使用此介面。

## InterruptIn

當某一支數位輸入 pin 的值發生改變時，InterruptIn 介面會觸發一個中斷事件。

除了 mbed pin p19 和 p20，其它任何編號的 mbed pin 皆可以當成 InterruptIn。

## Timer

Timer 介面被用來新增、啟動、停止、讀取一個計時器，用來測量一小段時間，通常是數毫到數秒不等。任何計時器都能被立刻新增、啟動、停止。

## Ticker

Ticker 介面被用來設置週期性的中斷，該週期之內重複地執行某個功能（呼叫某個函數）。

## Grove - Dust Sensor 特性介紹

當空氣中的顆粒物進入 Dust sensor 時，顆粒物本身的大小、單位時間內進入的數量會影響感測器的電壓狀態，造成一小段時間維持在低電壓。所以在我們指定的某一段測量時間裡，藉由每一次低電壓脈衝佔用的時間，可以計算出空氣中顆粒物的濃度，檢測出空氣品質。此感測器可檢測出直徑 > 1μm 的顆粒。

## 程式碼解說

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


