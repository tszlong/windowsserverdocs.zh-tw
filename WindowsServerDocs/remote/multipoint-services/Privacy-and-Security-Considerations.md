---
title: 隱私權和安全性考量
description: 提供關於 MultiPoint 服務的隱私權和安全性深入解析
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00eb89e5-aaf5-450e-94d8-3ef759dc5e26
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6580d0974e0e27a6ecd2706890801fbeb9c45d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848529"
---
# <a name="privacy-and-security-considerations"></a>隱私權和安全性考量
因為 MultiPoint 服務系統的設計屬於共用運算環境，所以請考量下列隱私權和安全運算議題。  
  
## <a name="privacy-in-a-multipoint-services-system"></a>MultiPoint 服務系統中的隱私權  
MultiPoint 服務可共用或不公開使用者文件，這對您、您的 MultiPoint 服務系統中的其他「系統管理使用者」、「MultiPoint 儀表板使用者」或「標準使用者」來說可能是新功能。 在 MultiPoint 管理員中，您可以看到畫面活動所有作用中的標準使用者桌面上。 當標準使用者登入 MultiPoint 服務系統時，系統會通知使用者必須接受此監視行為才能繼續。 如需如何共用內容或不公開內容的詳細資訊，請參閱[管理使用者檔案](Manage-User-Files.md)。  
  
## <a name="security-in-a-multipoint-services-system"></a>MultiPoint 服務系統中的安全性  
身為 MultiPoint 服務系統的管理使用者，您應熟悉 Windows 中的安全性和安全運算功能。 其中包括針對防火牆、防毒保護以及間諜軟體和其他惡意程式碼防護的 Windows 自動更新和支援。   
  
共用運算資源 (例如 MultiPoint 服務系統) 由於系統可能有大量使用者，加上站台硬體裝置和線路存取容易，因此可能很容易受到安全性威脅的攻擊。 惡意使用者可能會嘗試在 MultiPoint 服務站台集線器插入鍵盤記錄木馬程式或類似設計。 如果您發現 MultiPoint 服務系統的任何連接埠連接到無法辨識的裝置，請視為可疑並依照組織的安全性呈報方針處理。  
  
## <a name="see-also"></a>另請參閱  
[管理使用者檔案](Manage-User-Files.md)  
[管理 MultiPoint 服務系統](Managing-Your-MultiPoint-Services-System.md)