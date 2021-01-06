---
title: 在託管快取伺服器上匯入資料套件 (選用)
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache 的指示。
manager: brianlic
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8812c3008c2563fb8b436740d9fc0116c87c0154
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950214"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>在託管快取伺服器上匯入資料封裝（ \( 選擇性）\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此程式來匯入資料套件，並在託管快取伺服器上預先載入內容。

此程式是選擇性的，因為您不需要在託管快取伺服器上 prehash 和預先載入內容。

如果您未預先 \- 載入內容，當用戶端透過 WAN 連線下載資料時，系統會自動將資料新增至託管快取。

您必須是 Administrators 群組的成員，才能執行此程式。

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>在託管快取伺服器上匯入資料封裝

1. 在伺服器電腦上，以系統管理員許可權開啟 Windows PowerShell。

2. 輸入下列命令，將– Path 參數的值取代為您儲存資料封裝所在的資料夾位置，然後按 ENTER。

    ```
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```

3. 如果您有多個要預先載入內容的託管快取伺服器，請在每一部託管快取伺服器上執行此程式。

若要繼續進行本指南，請參閱 [設定用戶端自動託管快取探索（依服務連接點](10-Bc-Client-By-Scp.md)）。
