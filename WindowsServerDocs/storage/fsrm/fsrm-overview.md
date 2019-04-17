---
title: "檔案伺服器資源管理員 (FSRM) 概觀"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: "檔案伺服器資源管理員 (FSRM) 是可讓您在 Windows Server 檔案伺服器上管理和分類資料的工具。"
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>檔案伺服器資源管理員 (FSRM) 概觀

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

檔案伺服器資源管理員 (FSRM) 是 Windows Server 中的角色服務，可讓您管理和分類儲存在檔案伺服器上的資料。 檔案伺服器資源管理員包含下列功能：
  
-   **檔案分類基礎結構**：檔案分類基礎結構透過自動化分類程序，讓您深入了解資料，以便更有效地管理資料。 您可以分類檔案，並以該分類為基礎套用原則。 這些原則的範例包括限制檔案存取、檔案加密以及檔案到期的動態存取控制。 您可以使用檔案分類規則自動分類檔案，或修改所選檔案或資料夾的屬性來手動分類檔案。  
  
-   **檔案管理工作**：檔案管理工作可讓您根據檔案的分類來套用條件性原則或動作。 檔案管理工作的條件包括檔案位置、分類屬性、檔案建立日期、檔案上次修改日期，或檔案上次存取時間。 檔案管理工作可以採取的動作包括可使檔案到期、加密檔案或執行自訂命令的功能。  
  
-   **配額管理**：配額可讓您限制磁碟區或資料夾允許使用的空間，而且可以自動套用到磁碟區上建立的新資料夾。 您也可以定義能夠套用至新磁碟區或資料夾的配額範本。  
  
-   **檔案檢測管理**：檔案檢測協助您控制使用者可在檔案伺服器上儲存的檔案類型。 您可以限制可以儲存在共用檔案上的副檔名。 例如，您可以建立一個檔案檢測，不允許在檔案伺服器的個人共用資料夾中儲存副檔名為 MP3 的檔案。  
  
-   **存放裝置報告**：存放裝置報告可用來協助您識別磁碟使用量的趨勢，以及資料的分類方式。 您也可以監視選取的使用者群組是否嘗試儲存未授權的檔案。  
  
 您可以使用檔案伺服器資源管理員 Microsoft Management Console (MMC) 或 Windows PowerShell，來設定和管理檔案伺服器資源管理員所包含的功能。  
  
> [!IMPORTANT]
>  檔案伺服器資源管理員只支援以 NTFS 檔案系統格式化的磁碟區。 不支援彈性檔案系統。  
  
## <a name="practical-applications"></a>實際用途  
 檔案伺服器資源管理員有一些實際用途，包括：  
  
-   搭配動態存取控制案例使用檔案分類基礎結構，可建立一個根據檔案伺服器檔案分類方式授與檔案及資料夾存取權的原則。  
  
-   建立檔案分類規則，用來將任何包含至少 10 個身分證號碼的檔案，標記成具有個人識別資訊的檔案。  
  
-   將過去 10 年未曾修改過的任何檔案視為已到期檔案。  
  
-   為每個使用者的主目錄建立 200 MB 的配額，並在使用者即將使用達 180 MB 時通知他們。  
  
-   不允許在個人共用資料夾儲存任何音樂檔。  
  
-   將報告排程在每個星期天午夜執行，並產生一份過去兩天內最新存取的檔案清單。 這麼做可以幫助您判斷週末存放活動，並據此規劃您的伺服器停機時間。  

## <a name="see-also"></a>請參閱

- [檔案伺服器資源管理員的新功能](https://technet.microsoft.com/library/dn383587.aspx)
- [動態存取控制](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 