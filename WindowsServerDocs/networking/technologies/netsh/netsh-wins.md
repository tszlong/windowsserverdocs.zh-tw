---
title: 網路殼層 (Netsh) 範例批次檔
description: 您可以利用本主題了解如何建立批次檔，以使用 Netsh 在 Windows Server 2016 中執行多項工作。
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3f03a1c98463b1c6135bdf5ae62dafd91cf60b75
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946864"
---
# <a name="network-shell-netsh-example-batch-file"></a>網路殼層 (Netsh) 範例批次檔

適用於：Windows Server 2016

您可以利用本主題了解如何建立批次檔，以使用 Netsh 在 Windows Server 2016 中執行多項工作。 在此範例批次檔中，會使用 **netsh wins** 內容。

## <a name="example-batch-file-overview"></a>範例批次檔概觀

您可以在批次檔和其他指令碼中，將 Netsh 命令用於 Windows 網際網路名稱服務 \(WINS\)，以將工作自動化。 下列批次檔範例將示範如何將 Netsh 命令用於 WINS，以執行各種相關工作。

在此範例批次檔中，WINS\-A 是 IP 位址為 192.168.125.30 的 WINS 伺服器，而 WINS\-B 是 IP 位址為192.168.0.189 的 WINS 伺服器。

此範例批次檔會完成下列工作。

- 將 IP 位址為 192.168.0.205 的動態名稱記錄 MY\_RECORD \[04h\] 新增至 WINS\-A
- 將 WINS\-B 設定為 WINS\-A 的推送/提取複寫協力電腦
- 連線至 WINS\-B，然後將 WINS\-A 設定為 WINS\-B 的推送/提取複寫協力電腦
- 起始從 WINS\-A 到 WINS\-B 的推送複寫
- 連線至 WINS\-B，以確認已成功複寫新的記錄 MY\_RECORD

## <a name="netsh-example-batch-file"></a>Netsh 範例批次檔

在下列範例批次檔中，包含註解的程式列前面會加上 "rem"，以進行備註。 Netsh 會忽略註解。

```
rem: Begin example batch file.
rem two WINS servers:
rem (WINS-A) 192.168.125.30
rem (WINS-B) 192.168.0.189

rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}

rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2

rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2

rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0

rem 5. Connect to (WINS-B), and check that the record MY_RECORD [04h] was replicated successfully.
netsh wins server 192.168.0.189 show name Name=MY_RECORD EndChar=04

rem 6. End example batch file.
```

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>範例批次檔中使用的 Netsh WINS 命令

下一節列出此範例程序中使用的 **netsh wins** 命令。

- **server**。 將目前的 WINS 命令列內容移至依名稱或 IP 位址指定的伺服器。
- **add name**。 在 WINS 伺服器上註冊名稱。
- **add partner**。 在 WINS 伺服器上新增複寫協力電腦。
- **init push**。 起始推送觸發程序，並將其傳送至 WINS 伺服器。
- **show name**。 顯示 WINS 伺服器資料庫中特定記錄的詳細資訊。

## <a name="additional-references"></a>其他參考資料

- [網路殼層 (Netsh)](netsh.md)
