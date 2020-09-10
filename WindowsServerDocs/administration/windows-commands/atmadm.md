---
title: atmadm
description: Atmadm 命令的參考文章，可監視在非同步傳輸模式 (atM) 網路上的 atM 呼叫管理員所註冊的連線和位址。
ms.topic: reference
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c77ab10ce7ec628d3a1c820bc644f4b117d82953
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633404"
---
# <a name="atmadm"></a>atmadm

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

監視在非同步傳輸模式 (atM) 網路的 atM 呼叫管理員所註冊的連線和位址。 您可以使用 **atmadm** 來顯示 atM 介面卡上的傳入和撥出電話的統計資料。 使用時不含參數， **atmadm** 會顯示監視使用中 atM 線上狀態的統計資料。

## <a name="syntax"></a>語法

```
atmadm [/c][/a][/s]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /C | 顯示這部電腦上已安裝之 atM 網路介面卡的所有目前連線的呼叫資訊。 |
| /a | 針對安裝在這部電腦上的每個介面卡，顯示已註冊的 atM 網路服務存取點 (NSAP) 位址。 |
| /s | 顯示監視使用中 atM 線上狀態的統計資料。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- **Atmadm/c**命令會產生類似下列的輸出：

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

    下表包含 **atmadm/c** 範例輸出中每個元素的描述。

    | 資料類型 | 螢幕顯示 | 描述 |
    | -------- | --------- | -------- |
    | 連接資訊 | 輸入/輸出 | 呼叫的方向。 **中** 的是從另一部裝置到 atM 網路介面卡。  **從 atM** 網路介面卡到另一部裝置。 |
    | Pmp | 點對 multipoint 呼叫。 |
    | P-P | 點對點呼叫。 |
    | SVC | 連接是在切換的虛擬電路上。 |
    | Pvc | 連接是在永久虛擬電路上。 |
    | VPI/VCI 資訊 | VPI/VCI | 傳入或撥出電話的虛擬路徑和虛擬通道。 |
    | 遠端位址/媒體參數 | 47000580FFE1000000F21A2E180000C110081500 | **在) 中呼叫 (** 的 NSAP 位址，或**從) ** atM 裝置呼叫 (。 |
    | Tx | **Tx**參數包含下列三個元素：<ul><li>預設值或指定的位速率類型 (UBR、CBR、VBR 或 ABR) </li><li>預設或指定的行速度</li><li>指定的服務資料單位 (SDU) 的大小。</li></ul> |
    | Rx | **Rx**參數包含下列三個元素：<ul><li>預設值或指定的位速率類型 (UBR、CBR、VBR 或 ABR) </li><li>預設或指定的行速度</li><li>指定的 SDU 大小。</li></ul> |

- **Atmadm/a**命令會產生類似下列的輸出：

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```

- **Atmadm/s**命令會產生類似下列的輸出：

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

    下表包含 **atmadm/s** 範例輸出中每個元素的描述。

    | 呼叫管理員統計資料 | 描述 |
    | ------------- | -------- |
    | 目前使用中的呼叫 | 在這部電腦上安裝的 atM 介面卡上目前作用中的呼叫。 |
    | 成功的連入呼叫總數 | 已成功從這個 atM 網路上的其他裝置接收呼叫。 |
    | 成功的撥出電話總數 | 此電腦已成功完成此網路上其他 atM 裝置的呼叫。 |
    | 不成功的連入呼叫 | 無法連線到這部電腦的連入呼叫。 |
    | 不成功的連出呼叫 | 無法連線到網路上其他裝置的連出呼叫。 |
    | 遠端關閉的呼叫 | 由網路上的遠端裝置所關閉的呼叫。 |
    | 在本機關閉的呼叫 | 此電腦已關閉的呼叫。 |
    | 傳送的信號和 ILMI 封包 | 整合式本機管理介面的數目 (ILMI) 封包傳送到此電腦嘗試連線的交換器。 |
    | 收到信號和 ILMI 封包 | 從 atM 交換器接收的 ILMI 封包數目。 |

## <a name="examples"></a>範例

若要顯示這部電腦上已安裝之 atM 網路介面卡所有目前連線的通話資訊，請輸入：

```
atmadm /c
```

若要顯示已註冊的 atM 網路服務存取點 (此電腦上所安裝之每個介面卡的 NSAP) 位址，請輸入：

```
atmadm /a
```

若要顯示監視使用中 atM 線上狀態的統計資料，請輸入：

```
atmadm /s
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
