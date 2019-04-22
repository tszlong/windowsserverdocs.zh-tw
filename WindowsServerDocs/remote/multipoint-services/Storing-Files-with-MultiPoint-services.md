---
title: 使用 MultiPoint 服務儲存檔案
description: 了解在 MultiPoint 服務中的檔案儲存體
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817339"
---
# <a name="storing-files-with-multipoint-services"></a>使用 MultiPoint 服務儲存檔案
MultiPoint 服務支援儲存使用者檔案，以下列方式：  
  
-   **在硬碟上的作業系統磁碟分割。** 根據預設，MultiPoint 服務會儲存在與作業系統的硬碟上的使用者檔案。  
  
-   **硬碟上的獨立磁碟分割。** 當 MultiPoint 服務系統設定第一次時，您可以*分割區*髍迼枃碟。 也就是說，您可以設定磁碟機的區段，讓它的作用如同它是另一個磁碟機。 這可讓您更輕鬆地還原，或將作業系統升級而不會影響使用者檔案。 如需詳細資訊，請參閱 <<c0> [ 建立磁碟分割或邏輯磁碟機](https://go.microsoft.com/fwlink/?LinkId=182618)在 Windows Server 技術文件庫中。  
  
-   **其他內部或外部硬碟機上。** 您可以將額外的內部或外部硬碟磁碟機附加至 MultiPoint 服務儲存及備份資料。  
  
-   **在共用的網路資料夾中。** 若要提供使用者檔案從任何站台，您可以在網路上建立共用的資料夾。 這需要另一部電腦或伺服器以及執行 MultiPoint 服務的電腦。 這是建議的方法來儲存檔案，如果沒有可用的檔案伺服器。  
  
    適用於小型的 2-3 電腦沒有可用的檔案伺服器與執行 MultiPoint 服務系統，其中一部 MultiPoint 服務電腦可以做為檔案伺服器的所有 MultiPoint 服務電腦。 您接著會 MultiPoint 服務做為檔案伺服器上建立所有使用者的使用者的帳戶。  
  
