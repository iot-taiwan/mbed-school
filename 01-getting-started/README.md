# 第 1 章：ARM mbed 起步走

## 進入 Workspace

ARM mbed 的開發，必須使用 mbed.org 官方的 on-line compiler。將 LPC1786 與電腦連接後，進入 mbed.org 申請開發者帳號；進入 Compiler 環境後，可以看到圖 1.1 的 workspace。

![圖 1.1：mbed 的 workspace](1.1_workspace.png)

## "Hello, World"

依照以下步驟建立第一個 "Hello, World"：

1. 在 workspace 裡點選 *New -> New Program*
2. *Platform* 選取 mbed LPC1786
3. *Template* 選取 Blinky LED Hello World
4. *Program Name* 填寫自訂名稱
5. 按 *OK*

![圖 1.2：New Program](1.2_new-program.png)
圖 1.2：New Program

mbed 提供許多 Program 與 Library 範本，這裡使用的是一個 LED 閃爍的程式碼範本。接著進行編譯與 firmware 更新：

1. 在 workspace 裡點選 *Compile -> Compile All*
2. 編譯成功後，會自動下載一個副檔名為 *.bin* 的 firmware
3. 將下載回來的 firmware 拷貝到 LPC1768 開發板

使用 Mac 環境的開發者，使用 USB 將 LPC1768 與電腦連接後，會看到一個名為 *MBED* 的資料夾，只要將 firmware 檔案拉到此資料裡即可，如圖 1.3。

4. 拔除並重接 USB 線，讓 LPC1786 重新關機。
5. 完成。可以看到 LPC1786 的 LED1 開始閃爍。

![圖 1.3：Drag and Drop 更新 firmware](1.3_drag-drop.png)
圖 1.3：Drag and Drop 更新 firmware

