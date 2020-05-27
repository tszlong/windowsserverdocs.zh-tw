---
title: nbtstat
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0589fcd094d60fd5c3d9bc8798d273c49fb042b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820898"
---
# <a name="nbtstat"></a>nbtstat

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示 NetBIOS over TCP/IP （NetBT）通訊協定統計資料、本機電腦和遠端電腦的 NetBIOS 名稱表，以及 NetBIOS 名稱快取。 **nbtstat**允許重新整理 NetBIOS 名稱快取，以及使用 Windows 網際網路名稱服務（WINS）註冊的名稱。 使用時不含參數， **nbtstat**會顯示說明。

## <a name="syntax"></a>語法

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

#### <a name="parameters"></a>參數

|    參數    |                                                                                                                         說明                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a<remoteName> |    顯示遠端電腦的 NetBIOS 名稱資料表，其中*remoteName*是遠端電腦的 netbios 電腦名稱稱。 NetBIOS 名稱資料表是對應于該電腦上執行之 NetBIOS 應用程式的 NetBIOS 名稱清單。     |
| /A<IPaddress>  |                                                           顯示遠端電腦的 NetBIOS 名稱表，由遠端電腦的 IP 位址（小數點十進位標記法）所指定。                                                            |
|       /C        |                                                                        顯示 NetBIOS 名稱快取的內容、NetBIOS 名稱的資料表及其已解析的 IP 位址。                                                                         |
|       /n        |                                            顯示本機電腦的 NetBIOS 名稱表。 [**已註冊**] 狀態表示此名稱是透過廣播或 WINS 伺服器註冊。                                             |
|       /r        |      顯示 NetBIOS 名稱解析統計資料。 在執行 Windows XP 或 Windows Server 2003 且設定為使用 WINS 的電腦上，此參數會傳回已解析並使用 [廣播] 和 [WINS] 註冊的名稱數目。       |
|       /R        |                                                                      清除 NetBIOS 名稱快取的內容，然後重載**Lmhosts**檔案中 #PRE 標記的專案。                                                                      |
|       /RR       |                                                                           發行並重新整理已向 WINS 伺服器註冊之本機電腦的 NetBIOS 名稱。                                                                            |
|       /s        |                                                                          顯示 NetBIOS 用戶端和伺服器會話，並嘗試將目的地 IP 位址轉換成名稱。                                                                           |
|       /S        |                                                                          顯示 NetBIOS 用戶端和伺服器會話，僅根據目的地 IP 位址列出遠端電腦。                                                                          |
|   <Interval>    | 重新顯示選取的統計資料，並暫停每個顯示器之間*間隔*中所指定的秒數。 按 CTRL + C 停止重新重新加入統計資料。 如果省略此參數， **nbtstat**只會列印一次目前的設定資訊。 |
|       /?        |                                                                                                            在命令提示字元顯示說明。                                                                                                             |

## <a name="remarks"></a>備註

-   **nbtstat**命令列參數會區分大小寫。

-   下表描述由**nbtstat**產生的資料行標題：

    |朝向|說明|
    |------|--------|
    |輸入|收到的位元組數目。|
    |輸出|已傳送的位元組數。|
    |輸入/輸出|連線是從電腦（輸出）還是從另一部電腦連接到本機電腦（輸入）。|
    |存留期|名稱資料表快取專案在清除前存留的剩餘時間。|
    |本機名稱|與連接相關聯的本機 NetBIOS 名稱。|
    |遠端主機|與遠端電腦相關聯的名稱或 IP 位址。|
    |<03>|轉換成十六進位之 NetBIOS 名稱的最後一個位元組。 每個 NetBIOS 名稱長度為16個字元。 最後一個位元組通常具有特殊的重要性，因為相同的名稱可能會出現在電腦上數次，但只有最後一個位元組不同。 例如，<20> 是 ASCII 文字中的空格。|
    |類型|名稱的類型。 名稱可以是唯一的名稱或組名。|
    |狀態|遠端電腦上的 NetBIOS 服務是否正在執行（已註冊）或重複的電腦名稱稱已註冊相同的服務（衝突）。|
    |州|NetBIOS 連接的狀態。|

-   下表描述可能的 NetBIOS 連接狀態：

    |州|說明|
    |-----|--------|
    |連線|已建立會話。|
    |與|已建立連接端點，並與 IP 位址相關聯。|
    |接聽|此端點適用于輸入連線。|
    |閒置|已開啟此端點，但無法接收連接。|
    |Connecting|會話在連線階段中，而目的地的名稱與 IP 位址對應已被解析。|
    |接受|目前已接受傳入會話，很快就會連接。|
    |正在|會話嘗試重新連線（第一次嘗試連接失敗）。|
    |輸出|會話正在連接階段，而且目前正在建立 TCP 連線。|
    |輸入|輸入會話在連接階段。|
    |正在中斷連線|會話正在中斷連接的過程中。|
    |已中斷連接|本機電腦已發出中斷連線，而且正在等候遠端系統的確認。|

-   只有當網際網路通訊協定（TCP/IP）通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。

## <a name="examples"></a>範例
若要顯示 NetBIOS 電腦名稱稱為 CORP07 之遠端電腦的 NetBIOS 名稱資料表，請輸入：

```
nbtstat /a CORP07
```

若要顯示指派給 IP 位址為10.0.0.99 的遠端電腦的 NetBIOS 名稱表，請輸入：

```
nbtstat /A 10.0.0.99
```

若要顯示本機電腦的 NetBIOS 名稱表，請輸入：

```
nbtstat /n
```

若要顯示本機電腦的 NetBIOS 名稱快取內容，請輸入：

```
nbtstat /c
```

若要清除 NetBIOS 名稱快取，並在本機 Lmhosts 檔案中重載 #PRE 標記的專案，請輸入：

```
nbtstat /R
```

若要釋放向 WINS 伺服器註冊的 NetBIOS 名稱並重新註冊，請輸入：

```
nbtstat /RR
```

若要每隔五秒以 IP 位址顯示 NetBIOS 會話統計資料，請輸入：

```
nbtstat /S 5
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)


