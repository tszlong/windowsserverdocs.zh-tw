---
title: 安裝增益集
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833069"
---
# <a name="install-add-ins"></a>安裝增益集

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以在所有伺服器或用戶端電腦上加入增益集，只要在建立映像之前先安裝增益集即可。 之後，這些增益集就會自動加到所有使用該映像來複寫的電腦上。 您可以執行 .wssx 檔案以安裝增益集或者按照 [SDK 文件](https://go.microsoft.com/fwlink/?LinkID=248648)中各類型增益集的指南安裝個別增益集。 如果使用 .wssx 檔案進行安裝，可透過增益集管理員來解除安裝增益集。 如果您安裝個別檔案，則無法透過增益集管理員管理增益集。  
  
> [!NOTE]
>  任何在伺服器上安裝或下載之軟體 (包括 [儀表板] 索引標籤及健康情況通知) 的當地語系化介面，均應該符合伺服器上所安裝之作業系統的語言。  
  
#### <a name="to-install-an-add-in"></a>若要安裝增益集  
  
1.  (選用) 如果您要使用 .wssx 檔案安裝增益集，請完成下列步驟：  
  
    1.  儲存 < 名稱\>.wssx 檔案在參照電腦上的。  
  
    2.  按兩下 .wssx 檔案以開啟 [增益集安裝精靈]。  
  
    3.  遵循精靈的指示以完成安裝。  
  
2.  (選用) 將個別增益集檔案安裝在適當位置中，如 SDK 中針對每個增益集類型的定義。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)