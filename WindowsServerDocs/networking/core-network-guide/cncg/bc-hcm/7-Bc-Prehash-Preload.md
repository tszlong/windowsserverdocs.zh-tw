---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選用)
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 45d84103f3b41b08beb207bee570b469a3caf3dc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964224"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>在託管快取伺服器上預先雜湊並預先載入內容（ \( 選擇性）\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本節中的程式來預先雜湊內容伺服器上的內容、將內容新增至資料封裝，然後在託管快取伺服器上預先載入內容。

這些程式是選擇性的，因為您不需要在託管快取伺服器上預先雜湊和預先載入內容。

如果您未預先載入內容，則會自動將資料新增至託管快取，因為用戶端會透過 WAN 連線進行下載。

>[!IMPORTANT]
>雖然這些程式是選擇性的，但如果您決定在託管快取伺服器上預先雜湊並預先載入內容，則需要執行這兩個程式。

- [建立 Web 和檔案內容的內容伺服器資料封裝 &#40;選擇性&#41;](8-Bc-Data-Packages.md)

- [在託管快取伺服器上匯入資料封裝 &#40;選擇性&#41;](9-Bc-Import-Data.md)

若要繼續進行本指南，請參閱[建立 Web 和檔案內容的內容伺服器資料封裝 &#40;選擇性&#41;](8-Bc-Data-Packages.md)。