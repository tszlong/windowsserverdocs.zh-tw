---
title: （選擇性）裝載快取伺服器上的資料套件匯入
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0bd8f12ab76c8e2bf03ba79ce46a4cbea2f4dc5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>匯入的資料套件上裝載快取伺服器 \(Optional\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程序匯入的資料的套件，並 content 預先載入您裝載快取的伺服器。

此程序是選擇性的因為您不需要 content prehash 和預先載入您裝載快取的伺服器上。

如果您無法載入 pre\ content，資料會裝載快取自動新增為戶端 WAN 連接到下載。

您必須是系統管理員才能執行此程序群組成員。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>若要匯入裝載快取伺服器上的資料套件  

1. 伺服器在電腦上，請以系統管理員權限開放 Windows PowerShell。

2. 輸入下列命令，更換值-Path 參數使用的資料夾位置您儲存您的資料的套件，然後按 ENTER 鍵。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果您有多個您想要 content 預先載入的裝載快取伺服器，請在每個裝載快取的伺服器上執行此程序。

若要繼續使用此快速入門，請查看[設定 Client 自動裝載快取探索服務連接點的](10-Bc-Client-By-Scp.md)。
