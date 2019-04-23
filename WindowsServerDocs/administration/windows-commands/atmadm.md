---
title: atmadm
description: 適用於 Windows 命令主題**atmadm** -監視連線及註冊的 atM 位址呼叫 Manager 非同步傳輸模式 (atM) 網路上。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831769"
---
# <a name="atmadm"></a>atmadm

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

監視連線及註冊的 atM 位址呼叫管理員非同步傳輸模式 (atM) 網路上。 您可以使用**atmadm** atM 介面卡上顯示的傳入和傳出呼叫的統計資料。 不含參數， **atmadm**會顯示統計資料監視作用中的 atM 連線的狀態。 

## <a name="syntax"></a>語法

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|/c|顯示呼叫這部電腦上安裝的 atM 網路介面卡的所有目前連接的資訊。|
|/a|顯示已註冊的 atM 網路服務存取點 (NSAP) 位址，每個配接器安裝在這部電腦。|
|/s|顯示統計資料來監視使用中的 atM 連線的狀態。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- **Atmadm /c**命令會產生類似下面的輸出：

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```
    
    下表中的每個元素的描述**atmadm /c**範例輸出。
    
    |資料類型|螢幕上顯示|描述|
    |--------|---------|--------|
    |連接資訊|輸入/輸出|呼叫的方向。  **在 ** atM 網路介面卡是從另一部裝置。  **Out** atM 網路介面卡為另一部裝置。|
    ||PMP|點-點呼叫。|
    ||P-P|點對點呼叫。|
    ||SVC|連線位於交換式虛擬電路。|
    ||PVC|連線位於永久的虛擬電路。|
    |VPI/VCI 資訊|VPI/VCI|虛擬路徑和虛擬通道的傳入或傳出呼叫。|
    |遠端位址/媒體參數|47000580FFE1000000F21A2E180000C110081500|呼叫 NSAP 位址 **(In)** 或呼叫 **(Out)** atM 裝置。|
    ||**Tx**|**Tx**參數包含下列三個項目：<br /><br />-預設值或指定位元速率 （UBR、 CBR，VBR 或類型 ABR）<br />-預設值或指定線路速度<br />-指定服務的資料單位 (SDU) 大小|
    ||**Rx**|**Rx**參數包含下列三個項目：<br /><br />-預設值或指定位元速率 （UBR、 CBR，VBR 或類型 ABR）<br />-預設值或指定線路速度<br />-指定 SDU 大小|
    
- **Atmadm/a**命令會產生類似下面的輸出：

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- **Atmadm /s**命令會產生類似下面的輸出：

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```
    
    下表中的每個元素的描述**atmadm /s**範例輸出。
    
    |呼叫管理員統計資料|描述|
    |-------------|--------|
    |目前作用中呼叫|目前使用這部電腦上安裝的 atM 介面卡上的呼叫。|
    |成功的連入呼叫總計|已成功收到來自此 atM 網路上其他裝置的呼叫。|
    |成功的連出呼叫總計|從這部電腦到此網路上其他 atM 裝置成功的呼叫。|
    |失敗的連入呼叫|無法連線到此電腦的傳入呼叫。|
    |失敗的連出電話|無法連線到網路上的另一個裝置的傳出呼叫。|
    |遠端呼叫已關閉|關閉網路上的遠端裝置的呼叫。|
    |關閉本機呼叫|此電腦已關閉的呼叫。|
    |訊號和 ILMI 傳送的封包|傳送到交換器的整合式的本機管理介面 (ILMI) 封包數目為這台電腦嘗試連線。|
    |訊號和 ILMI 封包|收到來自 atM 切換 ILMI 封包數目。|
    
## <a name="BKMK_Examples"></a>範例

若要顯示這部電腦上安裝的 atM 網路介面卡目前所有連線的呼叫資訊，請輸入：

```
atmadm /c
```

若要顯示的已註冊的 atM 網路服務存取點 (NSAP) 位址安裝在這部電腦每張介面卡，請輸入：

```
atmadm /a
```

若要顯示統計資料監視作用中的 atM 連線的狀態，請輸入：

```
atmadm /s
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
