---
title: nbtstat
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e670b1490f1c4c54b8cf377d48755849faa16f8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437127"
---
# <a name="nbtstat"></a>nbtstat

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示 NetBIOS over TCP/IP (NetBT) 通訊協定統計資料、 本機電腦和遠端電腦的 NetBIOS 名稱資料表和 NetBIOS 名稱的快取。 **nbtstat**可讓重新整理的 NetBIOS 名稱快取以及註冊使用 Windows 網際網路名稱服務 (WINS) 的名稱。 不含參數， **nbtstat**顯示說明。 

## <a name="syntax"></a>語法

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>參數

|    參數    |                                                                                                                         描述                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    顯示遠端電腦的 NetBIOS 名稱資料表所在*remoteName*是遠端電腦的 NetBIOS 電腦名稱。 NetBIOS 名稱表格是對應至該電腦上執行的 NetBIOS 應用程式清單中的 NetBIOS 名稱。     |
| /A <IPaddress>  |                                                           顯示遠端電腦的 IP 位址 （在小數點十進位表示法） 所指定的遠端電腦的 NetBIOS 名稱資料表。                                                            |
|       /c        |                                                                        顯示內容的 netbios 命名快取、 NetBIOS 名稱的資料表和其已解析的 IP 位址。                                                                         |
|       /n        |                                            顯示本機電腦的 NetBIOS 名稱資料表。 狀態**註冊**表示已經註冊名稱，透過廣播或 WINS 伺服器。                                             |
|       /r        |      顯示 NetBIOS 名稱解析的統計資料。 在執行 Windows XP 或 Windows Server 2003 設定為使用 WINS 的電腦，此參數會傳回已解決的名稱的數目和已註冊使用廣播和 WINS。       |
|       /R        |                                                                      清除的 NetBIOS 名稱快取內容，然後再重新載入 # 預先 tagged 項目**Lmhosts**檔案。                                                                      |
|       /RR       |                                                                           釋出，然後更新 WINS 伺服器已註冊在本機電腦的 NetBIOS 名稱。                                                                            |
|       /s        |                                                                          會顯示 NetBIOS 用戶端和伺服器工作階段，嘗試將轉換的目的地 IP 位址的名稱。                                                                           |
|       /S        |                                                                          會顯示 NetBIOS 用戶端和伺服器工作階段，根據目的地 IP 位址只列出在遠端電腦。                                                                          |
|   <Interval>    | 會重新顯示所選的統計資料，暫停中指定的秒數*間隔*每部顯示器之間。 按下 CTRL + C 來停止重新統計資料。 如果省略這個參數，則**nbtstat**列印目前的組態資訊一次。 |
|       /?        |                                                                                                            在命令提示字元顯示說明。                                                                                                             |

## <a name="remarks"></a>備註

-   **nbtstat**命令列參數會區分大小寫。

-   下表描述所產生的資料行標題**nbtstat**:

    |朝向|描述|
    |------|--------|
    |Input|已接收的位元組數目。|
    |輸出|傳送的位元組數目。|
    |輸入/輸出|連接是否從 （輸出） 的電腦或從本機電腦的另一部電腦 （輸入）。|
    |生命週期|名稱資料表快取項目會存留在之前的剩餘時間則在清除。|
    |區域名稱|與連接相關聯的本機 NetBIOS 名稱。|
    |遠端主機|名稱或 IP 位址與遠端的電腦相關聯。|
    |<03>|NetBIOS 名稱的最後一個位元組轉換為十六進位。 每個 NetBIOS 名稱是 16 個字元。 此最後一個位元組會經常有特殊意義，因為相同的名稱可能會出現數次的電腦上，只有不同的最後一個位元組。 例如，< 20 > 是以 ASCII 文字的空間。|
    |type|名稱類型。 名稱可以是唯一的名稱或群組名稱。|
    |狀態|是否在遠端電腦上的 NetBIOS 服務正在執行 （登錄） 或重複的電腦名稱已註冊相同的服務 （衝突）。|
    |State|NetBIOS 連線的狀態。|

-   下表說明可能的 NetBIOS 連線狀態：

    |State|描述|
    |-----|--------|
    |已連線|已建立工作階段。|
    |相關聯|「 連線 」 端點已建立並相關聯的 IP 位址。|
    |接聽|此端點可供輸入的連線。|
    |閒置|此端點已經開啟，但不能接收的連線。|
    |連接|工作階段是在連接階段中，而且正在解析目的地的名稱到 IP 位址對應。|
    |接受|傳入的工作階段目前接受，並將在短時間內連線。|
    |重新連線|工作階段會嘗試重新連線 （它第一次嘗試連接失敗）。|
    |輸出|工作階段是在連接階段中，目前正在建立 TCP 連線。|
    |輸入|傳入的工作階段是在連接階段中。|
    |中斷連線|工作階段正在中斷連接。|
    |Disconnected|在本機電腦發出中斷連接，它正在等待從遠端系統的確認。|

-   此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。

## <a name="BKMK_Examples"></a>範例
若要顯示電腦的 NetBIOS 名稱 CORP07 的遠端電腦的 NetBIOS 名稱表格，請輸入：

```
nbtstat /a CORP07
```

若要顯示指派 10.0.0.99 的 IP 位址的遠端電腦的 NetBIOS 名稱表格，請輸入：

```
nbtstat /A 10.0.0.99
```

若要顯示本機電腦的 NetBIOS 名稱資料表，請輸入：

```
nbtstat /n
```

若要顯示本機電腦 NetBIOS 名稱快取的內容，請輸入：

```
nbtstat /c
```

若要清除的 NetBIOS 名稱快取，然後重新載入 # 預先 tagged 的項目，在本機的 Lmhosts 檔案中，輸入：

```
nbtstat /R
```

若要釋放登錄至 WINS 伺服器的 NetBIOS 名稱，並重新註冊它們，請輸入：

```
nbtstat /RR
```

若要顯示的 IP 位址每隔五秒 NetBIOS 工作階段統計資料，請輸入：

```
nbtstat /S 5
```

## <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)


