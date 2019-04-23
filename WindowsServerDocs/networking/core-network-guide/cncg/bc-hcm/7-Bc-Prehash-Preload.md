---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選用)
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839389"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>預先雜湊和預先載入託管快取伺服器上的內容\(選擇性\)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用本節中的程序預先雜湊內容伺服器上，將內容新增至資料的封裝，然後預先載入託管快取伺服器上的內容。 

這些程序是選擇性的因為您並不需要 prehash 和預先載入內容在託管快取伺服器上。 

如果您未預先載入內容，資料會加入至託管快取會自動用戶端會下載它透過 WAN 連線。

>[!IMPORTANT]
>雖然這些程序是共同選擇性，如果您決定 prehash 和預先載入內容，在您的託管快取伺服器，則需要執行兩個程序。

- [建立 Web 內容伺服器資料套件，以及檔案內容&#40;選擇性&#41;](8-Bc-Data-Packages.md)
  
- [在託管快取伺服器上的資料封裝匯入&#40;選擇性&#41;](9-Bc-Import-Data.md)

若要繼續進行本指南，請參閱[建立內容伺服器資料套件適用於 Web 和檔案內容&#40;選擇性&#41;](8-Bc-Data-Packages.md)。