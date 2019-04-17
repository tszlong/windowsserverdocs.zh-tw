---
title: "新增健康提醒"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>新增健康提醒

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

健康增益集提供定義警示、健康檢查，和修復的網路問題。 健康增益集組成 xml 檔案加註代碼或評估健康資訊的特定功能的資料。 建立開發人員和系統管理員伺服器 client 電腦上安裝健康增益集。  
  
 請參考[Windows Server 方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)如建立健康增益集的詳細資訊。  
  
## <a name="installing-health-add-in-files"></a>安裝健康增益集檔案  
 開發人員已經建立 xml 檔案之後，您必須將檔案複本置於伺服器 client 電腦上的適當位置。  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>若要安裝在伺服器上的 xml 檔案  
  
1.  在**%ProgramFiles%\Windows Server\Bin\Feature 定義**資料夾，建立新資料夾名為**MyHealthAddIn**。 您可以命名此資料夾任何。 建議的資料夾名稱會功能名稱相同。  
  
2.  將 Definition.xml 和 Definition.xml.config 檔案複製到新的資料夾。  
  
3.  如果您建立或條件為二進位檔，您也應該複製這些檔案來**%ProgramFiles%\Windows Server\Bin**。  
  
 Client 電腦執行排定的工作每 6 個小時的提取 XML 檔案的適當位置。 您可以手動執行的工作強制伺服器 client 電腦之間同步處理。  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>若要安裝 client 電腦 xml 檔案  
  
1.  打開工作排程器。  
  
2.  執行**HealthDefintionUpdateTask**在 [工作排程器。  
  
    > [!NOTE]
    >  這項工作不會安裝二進位檔案。 您必須手動二進位將檔案複製到**%ProgramFiles%\Windows Server\Bin**上的資料夾。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)