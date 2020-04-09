---
title: 新增健康狀態警示
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2540c4af032d35f5be71338513030c59cb18953d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817622"
---
# <a name="add-health-alerts"></a>新增健康狀態警示

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

健康狀態增益集可提供警示、健康狀態檢查及網路問題修復的定義。 健康狀態增益集是由 XML 檔案所組成，而這些檔案會註解用於評估特定功能健康狀態資訊的程式碼或資料。 健康狀態增益集是由開發人員所建立，並由系統管理員安裝在伺服器和用戶端電腦上。  
  
 請參閱 [Windows Server 解決方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648) ，以取得有關建立健康狀態增益集的詳細資料。  
  
## <a name="installing-health-add-in-files"></a>安裝健康狀態增益集檔案  
 當開發人員建立 XML 檔案之後，您必須將一份檔案放在伺服器和用戶端電腦上的適當位置。  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>若要在伺服器上安裝 XML 檔案  
  
1. 在 **%ProgramFiles%\Windows Server\Bin\Feature Definitions** 資料夾中，建立名為 **MyHealthAddIn**的新資料夾。 您可以為此資料夾指定任何名稱。 建議讓此資料夾的名稱與功能名稱相同。  
  
2. 將 Definition.xml 和 Definition.xml.config 檔案複製到新資料夾。  
  
3. 如果建立了條件或動作的二進位檔案，您也必須將這些檔案複製到 **%ProgramFiles%\Windows Server\Bin**。  
  
   用戶端電腦會每隔 6 小時執行排定的工作，將 XML 檔案提取至適當的位置。 您可以手動執行此工作，以強制執行用戶端電腦和伺服器之間的同步處理。  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>若要在用戶端電腦上安裝 XML 檔案  
  
1.  開啟工作排程器。  
  
2.  在工作排程器中執行 **HealthDefintionUpdateTask**。  
  
    > [!NOTE]
    >  此工作不會安裝二進位檔案。 您必須手動將二進位檔案複製到用戶端電腦上的 **%ProgramFiles%\Windows Server\Bin** 資料夾。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)