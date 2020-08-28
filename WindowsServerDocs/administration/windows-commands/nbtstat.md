---
title: nbtstat
description: Nbtstat 命令的參考文章，會顯示 NetBIOS over TCP/IP 的 (NetBT) 通訊協定統計資料、本機電腦和遠端電腦的 NetBIOS 名稱表格，以及 NetBIOS 名稱快取。
ms.topic: reference
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 273aaadcf537864512d6dc7aedff0c9872c3cd05
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037796"
---
# <a name="nbtstat"></a>nbtstat

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示 NetBIOS over TCP/IP (NetBT) 通訊協定統計資料、本機電腦和遠端電腦的 NetBIOS 名稱表格，以及 NetBIOS 名稱快取。 此命令也可讓您重新整理 NetBIOS 名稱快取，以及在 Windows 網際網路名稱服務中註冊的名稱 (WINS) 。 使用時不含參數，此命令會顯示說明資訊。

只有在 [網路連線] 的網路介面卡內容中，將 [ (TCP/IP) 通訊協定安裝為元件時，才能使用此命令。

## <a name="syntax"></a>語法

```
nbtstat [/a <remotename>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<interval>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /a `<remotename>` | 顯示遠端電腦的 NetBIOS 名稱表，其中 *remotename* 是遠端電腦的 netbios 電腦名稱稱。 NetBIOS 名稱表是與在該電腦上執行的 NetBIOS 應用程式對應的 NetBIOS 名稱清單。 |
| /A `<IPaddress>` | 顯示遠端電腦的 NetBIOS 名稱表，由 IP 位址指定， (以小數點十進位標記法) 遠端電腦。 |
| /C | 顯示 NetBIOS 名稱快取的內容、NetBIOS 名稱的表格及其已解析的 IP 位址。 |
| /n | 顯示本機電腦的 NetBIOS 名稱表。 [ **已註冊** ] 的狀態表示此名稱是由廣播或 WINS 伺服器註冊。 |
| /r | 顯示 NetBIOS 名稱解析統計資料。 |
| /R | 清除 NetBIOS 名稱快取的內容，然後從 **Lmhosts** 檔案重載預先標記的專案。 |
| /RR | 釋放並重新整理向 WINS 伺服器註冊之本機電腦的 NetBIOS 名稱。 |
| /s | 顯示 NetBIOS 用戶端和伺服器會話，並嘗試將目的地 IP 位址轉換成名稱。 |
| /S | 顯示 NetBIOS 用戶端和伺服器會話，只列出目的地 IP 位址的遠端電腦。 |
| `<interval>` | 顯示選取的統計資料，並暫停每個顯示 *間隔* 內指定的秒數。 按 CTRL + C 停止顯示統計資料。 如果省略此參數， **nbtstat** 只會列印目前的設定資訊一次。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Nbtstat**命令列參數會區分大小寫。

- **Nbtstat**命令所產生的資料行標題包括：

    | 朝向 | 描述 |
    | ------- | ----------- |
    | 輸入 | 收到的位元組數目。 |
    | 輸出 | 已傳送的位元組數。 |
    | 輸入/輸出 | 連接是從電腦 (輸出) 或從另一部電腦到本機電腦 (輸入) 。 |
    | 存留期 | 名稱資料表快取專案在清除之前將存留的剩餘時間。 |
    | 本機名稱 | 與連接相關聯的本機 NetBIOS 名稱。 |
    | 遠端主機 | 與遠端電腦相關聯的名稱或 IP 位址。 |
    | `<03>` | NetBIOS 名稱的最後一個位元組已轉換為十六進位。 每個 NetBIOS 名稱長度都是16個字元。 最後一個位元組的重要性通常很高，因為在電腦上可能會出現數次相同的名稱，但只有最後一個位元組不同。 例如， `<20>` 是 ASCII 文字中的一個空格。 |
    | type | 名稱的類型。 名稱可以是唯一的名稱或組名。 |
    | 狀態 | 遠端電腦上的 NetBIOS 服務是否正在執行 (註冊) 或重複的電腦名稱稱已註冊相同的服務 (衝突) 。 |
    | State | NetBIOS 連接的狀態。 |

- 可能的 NetBIOS 連接狀態包括：

    | State | 描述 |
    | ------- | ----------- |
    | 連線 | 已建立會話。 |
    | 接聽 | 此端點可供輸入連接使用。 |
    | 閒置 | 此端點已開啟，但無法接收連接。 |
    | Connecting | 會話處於連接階段，而目的地的名稱到 IP 位址對應正在解析中。 |
    | 接受 | 目前已接受輸入會話，且即將連接。 |
    | 重新 | 會話正在嘗試重新連接， (它在第一次嘗試) 時無法連線。 |
    | 輸出 | 會話正在連接階段，而且目前正在建立 TCP 連接。 |
    | 輸入 | 傳入會話處於連接階段。 |
    | 正在中斷連線 | 會話正在進行中斷連線。 |
    | 已中斷連接 | 本機電腦已發出中斷連線，而且正在等候遠端系統確認。 |

### <a name="examples"></a>範例

若要顯示 NetBIOS 電腦名稱稱為 *CORP07*之遠端電腦的 netbios 名稱表，請輸入：

```
nbtstat /a CORP07
```

若要顯示指派 IP 位址為 *10.0.0.99*之遠端電腦的 NetBIOS 名稱表，請輸入：

```
nbtstat /A 10.0.0.99
```

若要顯示本機電腦的 NetBIOS 名稱表，請輸入：

```
nbtstat /n
```

若要顯示本機電腦 NetBIOS 名稱快取的內容，請輸入：

```
nbtstat /c
```

若要清除 NetBIOS 名稱快取並重載本機 *Lmhosts* 檔案中預先標記的專案，請輸入：

```
nbtstat /R
```

若要釋放登錄于 WINS 伺服器的 NetBIOS 名稱，然後重新註冊它們，請輸入：

```
nbtstat /RR
```

若要每隔五秒鐘依 IP 位址顯示 NetBIOS 會話統計資料，請輸入：

```
nbtstat /S 5
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
