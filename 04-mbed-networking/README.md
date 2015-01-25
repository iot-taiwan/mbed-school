# 第 4 章：ARM mbed 網路組態

## 準備工作

在 IoT 的技發發展藍圖裡，描述了 IoT 的 4 個發展階段，其中第 4 個階段就是 WoT。而目前正好處於第 4 個 IoT 發展階段。Google 在 2014 年底發起的 Physical Web 計畫，是一個非常先期的研究計畫，就是為了 IoT 的新階段預做準備 [1]。

IoT 的第 4 個階段，將聚焦在 Advanced Sensor Fusion 與 Physical-World Web 層面，這二個層面簡單來說，就是 WoT。根據維期百科上的定義，WoT 是 IoT 的 Application Layer，並且是使用 Web 技術來打造 application。也就是說，IoT + Web-enabled technologies 就是 WoT。

對 WoT 來說，最重要的觀念，就是以 URL 來表示 IoT 裝置；為 IoT 加入 URL 的觀念，就是 Google 提出的 Physical Web 計畫。所以說，WoT 與 Physical Web 是一體兩面的觀念，都是 IoT 正進入的新發展階段。

目前有許多 ARM mbed 的開發板，這些開發板並不是「另一個 Arduino」硬體，而是更能符合 WoT 理念的 RESTful device。同樣的硬體，不同的觀念、技術框架與商業思維，能帶來不同的產品思維與商業模式。所以，ARM mbed 與 WoT 帶來的，將是一場新的革命與機會。

從第 5 章開始，mbed Taiwan 將開始帶給大家不一樣的 IoT 技術視野。

[1]: http://www.jollen.org/blog/2015/01/arm-mbed-1-physical-web.html

## 使用 DHCP 取得 IP 位址


```
#include "mbed.h"
  
struct netif netif_data;

int main() { 
    struct ip_addr  ipaddr;
    struct ip_addr  netmask;
    struct ip_addr  gateway;
        
    /* LPC1768 host name*/
    char *hostname = "lpc1768_jollen";
 
    printf("DHCP: Start networking...\r\n");
    
    /* Start Networking using DHCP */
    IP4_ADDR(&netmask, 255,255,255,255);
    IP4_ADDR(&gateway, 0,0,0,0);
    IP4_ADDR(&ipaddr, 0,0,0,0);
 
    /* Initialise after configuration */
    lwip_init();
    netif->hwaddr_len = ETHARP_HWADDR_LEN;
    device_address((char *)netif->hwaddr);
 
    /* debug MAC address*/
    printf("mbed HW address: %02x:%02x:%02x:%02x:%02x:%02x\r\n",
           (char*) netif->hwaddr[0],
           (char*) netif->hwaddr[1],
           (char*) netif->hwaddr[2],
           (char*) netif->hwaddr[3],
           (char*) netif->hwaddr[4],
           (char*) netif->hwaddr[5]);
 
    netif = netif_add(netif, &ipaddr, &netmask, &gateway, NULL, device_init, ip_input);
    netif->hostname = hostname;
    netif_set_default(netif);


    /* DHCP */
    dhcp_start(netif);
 
    /* Initialise all needed timers */
    tickARP.attach_us( &etharp_tmr,  ARP_TMR_INTERVAL  * 1000);
    tickFast.attach_us(&tcp_fasttmr, TCP_FAST_INTERVAL * 1000);
    tickSlow.attach_us(&tcp_slowtmr, TCP_SLOW_INTERVAL * 1000);
    dns_tick.attach_us(&dns_tmr, DNS_TMR_INTERVAL * 1000);
    dhcp_coarse.attach_us(&dhcp_coarse_tmr, DHCP_COARSE_TIMER_MSECS * 1000);
    dhcp_fine.attach_us(&dhcp_fine_tmr, DHCP_FINE_TIMER_MSECS * 1000);
  
    while (!netif_is_up(netif)) {
        device_poll();
    }
 
    printf("DHCP: local IP is %s\r\n", inet_ntoa(*(struct in_addr*)&(netif->ip_addr))); 
}
```

## 使用 Static IP 位址

```
#include "mbed.h"
#include "EthernetInterface.h"

int main(void) {
    eth = new EthernetInterface;
    eth->init("192.168.21.81", "255.255.255.0", "192.168.21.2" );

    if (eth->connect()) 
        return -1;

    printf("IP Address is %s\r\n", eth->getIPAddress());
}
```

## MQTT Client

```
http://developer.mbed.org/cookbook/mbed_Client_for_MQTT
```
