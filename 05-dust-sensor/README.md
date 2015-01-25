# 第 5 章：DIY 空氣品質偵測器

## 準備工作

### LPC1768 的 GPIO Pinout 

### 電子零件介紹

## Dust Sensor 硬件原型製作

## Lightweight Web Server

## REST API 設計

本章所設計的空氣品質偵測器，將以 REST API 方式提供 sensor data。基本觀念：

1. 需移植一個 httpd 至 LPC1786
2. 需要製作一個 frontend
3. Frontend 以 data pull 方式調用空氣品質偵測器的 REST API
4. 當 REST API 被調用時，再即時讀取空氣指數，並以 JSON 格式返回數據

這個設計當有幾個技術議題，未來將會加強：

1. Sensor device 應以 data push 方式，即時推送 sensor data


### 取得空氣指數

REST API 定義：

```
GET /1/mbed/lpc1786/sensor/dust/sen12291p
```

JSON 回傳資料格式：

```
{
	success: true
	data: {

	}
}
```
