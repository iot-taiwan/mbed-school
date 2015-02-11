# 第 4 章：ARM mbed 網路組態

對 WoT 來說，最重要的觀念，就是以 URL 來表示 IoT 裝置；為 IoT 加入 URL 的觀念，就是 Google 提出的 Physical Web 計畫。所以說，WoT 與 Physical Web 是一體兩面的觀念，都是 IoT 正進入的新發展階段。

本章將開始介紹 ARM mbed 的網路功能。

## 準備工作

LPC1768 本身沒有 PHY 功能，因此必須使用 mbed shield 子板，將 LPC1786 加上 ethernet 功能；也可以改用 Seeed Studio 設計與生產的 Arch Pro 開發板。

## 使用 DHCP 取得 IP 位址

使用 DHCP 進行網路組態。

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

先匯入 ARM mbed 提供 EthernetInterface Library 後， 引入 *EthernetInterface.h* 標頭檔，即可設定 Static IP 位址。
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
