---
title: 網路 Shell (Netsh) 範例批次檔
description: 您可以使用本主題來瞭解如何建立批次檔，以在 Windows Server 2016 中使用 Netsh 執行多項工作。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 86fbe66978f7c09a332bba16a27a13fa029cb5a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401912"
---
# <a name="network-shell-netsh-example-batch-file"></a>網路命令介面 \(Netsh @ no__t-1 範例批次檔

適用於：Windows Server 2016

您可以使用本主題來瞭解如何使用 Windows Server 2016 中的 Netsh，建立執行多項工作的批次檔。 在此範例批次檔中，會使用**netsh wins**內容。

## <a name="example-batch-file-overview"></a>範例批次檔總覽

您可以在批次檔和其他腳本中，使用 Windows 網際網路名稱服務的 Netsh 命令 \(WINS @ no__t-1 來自動化工作。 下列批次檔範例示範如何使用適用于 WINS 的 Netsh 命令來執行各種相關的工作。

在此範例批次檔中，WINS @ no__t-0A 是具有 IP 位址192.168.125.30 的 WINS 伺服器，而 WINS @ no__t-1B 是 IP 位址為192.168.0.189 的 WINS 伺服器。

範例批次檔會完成下列工作。

- 將具有 IP 位址192.168.0.205、MY @ no__t-0RECORD \[04h @ no__t-2 的動態名稱記錄新增至 WINS @ no__t-3A
- 將 WINS @ no__t-0B 設定為 WINS @ no__t-1A 的推送/提取複寫協力電腦
- 連接到 WINS @ no__t-0B，然後將 WINS @ no__t-1A 設定為 WINS @ no__t-2B 的推送/提取複寫協力電腦
- 起始從 WINS @ no__t-0A 到 WINS @ no__t-1B 的推送複寫
- 連接到 WINS @ no__t-0B，以確認已成功複寫新的記錄（MY @ no__t-1RECORD）

## <a name="netsh-example-batch-file"></a>Netsh 範例批次檔

在下列範例批次檔中，包含批註的程式列前面會加上 "rem"，以取得批註。 Netsh 會忽略批註。

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
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>範例批次檔中使用的 Netsh WINS 命令

下一節列出此範例程式中使用的**netsh wins**命令。

- **伺服器**。 將目前的 WINS 命令 @ no__t-0line 內容移至其名稱或 IP 位址所指定的伺服器。
- **新增名稱**。 在 WINS 伺服器上註冊名稱。
- **新增合作夥伴**。 在 WINS 伺服器上新增複寫協力電腦。
- **init push**。 起始推播觸發程式，並將其傳送至 WINS 伺服器。
- **顯示名稱**。 顯示 WINS 伺服器資料庫中特定記錄的詳細資訊。  

如需詳細資訊，請參閱[Network Shell （Netsh）](netsh.md)。
