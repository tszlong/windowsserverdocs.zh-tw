---
title: 網路 Shell (Netsh) 範例批次檔
description: 您可以使用本主題以了解如何建立執行 Windows Server 2016 中使用 Netsh 的多個工作批次檔。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880169"
---
# <a name="network-shell-netsh-example-batch-file"></a>網路殼層\(Netsh\)範例批次檔

適用於：Windows Server 2016

您可以使用本主題以了解如何建立 Windows Server 2016 中使用 Netsh 來執行多個工作批次檔。 在這個範例批次檔中， **netsh wins**會使用內容。

## <a name="example-batch-file-overview"></a>範例批次檔概觀

您可以使用 Windows 網際網路名稱服務的 Netsh 命令\(WINS\)批次檔和其他自動化工作的指令碼中。 下列批次檔範例示範如何使用 WINS 的 Netsh 命令執行的各種相關的工作。

在這個範例批次檔，贏得\-A 是 WINS 伺服器 IP 位址 192.168.125.30 與 WINS\-B 是 WINS 伺服器 IP 位址 192.168.0.189。

範例批次檔會完成下列工作。

- 加入動態的名稱記錄 IP 位址 192.168.0.205，MY\_記錄\[04 h\]，到 WINS\-A
- 設定 WINS\-B 為推播/提取複寫夥伴的 WINS\-A
- 連接到 WINS\-B，然後按一下 設定 WINS\-WINS 的推播/提取複寫協力電腦 A\-B
- 起始從 WINS 推入複寫\-A 到 WINS\-B
- 連接到 WINS\-B 以確認新的記錄，MY\_記錄中，已成功複寫

## <a name="netsh-example-batch-file"></a>Netsh 範例批次檔

在下列範例的批次檔，包含註解的行前面加上"rem，"的備註。 Netsh 會忽略註解。

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>中的範例批次檔使用 Netsh WINS 命令

下列區段會列出**netsh wins**會在此範例程序的命令。

- **伺服器**。 將目前的 WINS 命令\-列內容，以指定其名稱或 IP 位址的伺服器。
- **新增名稱**。 登錄至 WINS 伺服器的名稱。
- **新增夥伴**。 新增複寫協力電腦上的 WINS 伺服器。
- **init 推播**。 初始並傳送至 WINS 伺服器的推入觸發程序。
- **顯示名稱**。 會顯示在 WINS 伺服器資料庫中的特定記錄的詳細的資訊。  

如需詳細資訊，請參閱 <<c0> [ 網路殼層 (Netsh)](netsh.md)。
