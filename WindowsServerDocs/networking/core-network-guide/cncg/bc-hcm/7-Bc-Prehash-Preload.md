---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選用)
description: 瞭解如何在內容伺服器上 prehash 內容、將內容新增至資料套件，然後在託管快取伺服器上預先載入內容。
manager: brianlic
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 912b5066b5dc0b3cac9fb40c01f0904e6c63477e
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965540"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash 和預先載入託管快取伺服器上的內容（ \( 選擇性）\)

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本節中的程式，在內容伺服器上 prehash 內容、將內容新增至資料套件，然後預先載入託管快取伺服器上的內容。

這些程式是選擇性的，因為您不需要在託管快取伺服器上 prehash 和預先載入內容。

如果您未預先載入內容，當用戶端透過 WAN 連線下載資料時，系統會自動將資料新增至託管快取。

>[!IMPORTANT]
>雖然這些程式都是選擇性的，但如果您決定要在託管快取伺服器上 prehash 和預先載入內容，則必須執行這兩個程式。

- [建立 Web 和檔案內容的內容伺服器資料封裝 &#40;選用&#41;](8-Bc-Data-Packages.md)

- [在託管快取伺服器上匯入資料封裝 &#40;選擇性&#41;](9-Bc-Import-Data.md)

若要繼續使用本指南，請參閱 [建立 Web 和檔案內容的內容伺服器資料封裝 &#40;選擇性&#41;](8-Bc-Data-Packages.md)。