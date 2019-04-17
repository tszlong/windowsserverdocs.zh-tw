---
title: 網路 Shell (Netsh) 範例批次檔案
description: 您可以使用本主題以了解如何建立執行多工作，使用 Windows Server 2016 Netsh 批次檔。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh-example-batch-file"></a>網路殼層 \(Netsh\) 範例批次檔案

適用於：Windows Server 2016

您可以使用本主題以了解如何建立 Windows Server 2016 中使用 Netsh 執行多工作批次檔。 在此範例批次檔案中，**netsh wins**使用操作。

## <a name="example-batch-file-overview"></a>範例「批次檔案概觀

您可以使用 Netsh 命令 Windows 網際網路名稱服務 \(WINS\) 在「批次檔案及其他指令碼自動化工作。 批次檔案下例示範如何使用 Netsh 命令 WINS 來執行各種不同的相關的工作。

在「批次檔此範例，WINS\-A WINS 伺服器的 IP 位址 192.168.125.30，WINS\-B 且 WINS 伺服器的 IP 位址 192.168.0.189。

範例「批次檔案達成下列工作。

- 新增名稱動態筆 192.168.0.205 的 IP 位址、MY\_RECORD \[04h\]，以 WINS\-A
- 設定為 WINS\-A 的推播日拉複寫協力廠商的 WINS\-B
- 連接至 WINS\-B，並將設定為推播日拉複寫協力廠商的 WINS\-B WINS\-A。
- 起始 WINS\-B 推播複寫 WINS\-A 從
- 若要確認已成功複寫新記錄 MY\_RECORD，WINS\-B 連接

## <a name="netsh-example-batch-file"></a>Netsh 範例批次檔案

以下的範例批次檔案，包含意見行的前面加上「rem，」的。 Netsh 忽略意見。

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>範例「批次檔案中使用 WINS Netsh 命令

下列區段清單**netsh wins**在此範例程序中所使用的命令。

- **伺服器**。 將目前的 WINS command\ 列操作切換到依其名稱或 IP 位址所指定的伺服器。
- **新增名稱**。 暫存器 WINS 伺服器上的名稱。
- **新增協力廠商**。 新增複寫合作夥伴 WINS 伺服器上。
- **初始化推播**。 初始並 WINS 伺服器來傳送推播觸發程序。
- **顯示名稱**。 顯示的特定 WINS 伺服器資料庫中的詳細的資訊。  

如需詳細資訊，請查看[網路 Shell (Netsh)](netsh.md)。
