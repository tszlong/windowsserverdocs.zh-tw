---
title: 在託管快取伺服器上匯入資料套件 (選用)
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887999"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>在託管快取伺服器上的資料封裝匯入\(選擇性\)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用此程序匯入資料套件，並在託管快取伺服器上預先載入內容。

此程序是選擇性的因為您不需要 prehash 和預先載入內容在託管快取伺服器上。

如果您這樣做不預先\-內容，資料加入至託管快取會自動為用戶端會下載它透過 WAN 連線的負載。

您必須是 Administrators 群組的成員才能執行此程序。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>若要匯入資料在託管快取伺服器上的封裝  

1. 伺服器電腦上，開啟 Windows PowerShell 系統管理員權限。

2. 輸入下列命令，取代值的資料夾位置已儲存您的資料套件，然後再按 ENTER 鍵 – Path 參數。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果您有多部託管快取伺服器，您要預先載入內容時，請在每個託管快取伺服器上執行此程序。

若要繼續進行本指南，請參閱[設定用戶端自動託管快取探索透過服務連線點](10-Bc-Client-By-Scp.md)。
