---
title: 在託管快取伺服器上匯入資料套件 (選用)
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 41bb7f5f0637bd4b1f1bfa3c0451debff58a3e5a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318436"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>在託管快取伺服器上匯入資料封裝 \(選擇性\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用這個程式匯入資料封裝，並在託管快取伺服器上預先載入內容。

這個程式是選擇性的，因為您不需要在託管快取伺服器上預先雜湊和預先載入內容。

如果您未預先\-載入內容，則會自動將資料新增至託管快取，因為用戶端會透過 WAN 連線進行下載。

您必須是 Administrators 群組的成員，才能執行此程式。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>在託管快取伺服器上匯入資料封裝  

1. 在伺服器電腦上，以系統管理員許可權開啟 Windows PowerShell。

2. 輸入下列命令，將– Path 參數的值取代為您儲存資料封裝的資料夾位置，然後按 ENTER。

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. 如果您要預先載入內容的託管快取伺服器不只一個，請在每部託管快取伺服器上執行此程式。

若要繼續進行本指南，請參閱[設定服務連接點的用戶端自動託管](10-Bc-Client-By-Scp.md)快取探索。
