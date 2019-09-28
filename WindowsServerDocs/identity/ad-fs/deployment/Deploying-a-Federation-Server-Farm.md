---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Windows Server 2012 R2 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5507cd567114d17c6500655ee210b70bd9ea1ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408418"
---
# <a name="deploying-a-federation-server-farm"></a>部署同盟伺服器陣列


若要部署同盟伺服器陣列，請依序完成此檢查清單中的工作。 當參考連結將您導向到觀念性主題時，請在檢閱觀念性主題之後返回此檢查清單，然後再繼續此檢查清單中其餘的工作。  
  
@no__t 0deploying 同盟伺服器陣列 @ no__t-1Checklist：部署同盟伺服器陣列 @ no__t-0  
  
||工作|參考資料|  
|-|--------|-------------|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|當您準備部署 Active Directory 同盟服務 \(AD FS @ no__t-1 時，請參閱重要的概念和考慮。 **注意：**|@no__t 0deploying 同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS Windows server 2012 R2 中的設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />@no__t 0deploying 同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[瞭解重要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||如果您決定要將 Microsoft SQL Server 用於 AD FS 設定存放區，請確實部署可運作的 SQL Server 執行個體。|[SQL Server](https://technet.microsoft.com/sqlserver) **警告：** 在 Windows Server 2012 R2 中，如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|將您的電腦加入 Active Directory 網域中。|@no__t 0deploying 同盟伺服器陣列將](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|註冊安全通訊端層 \(SSL @ no__t-1 AD FS 的憑證。|@no__t 0deploying 同盟伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[註冊 SSL 憑證以供 AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|安裝 AD FS 角色服務。|@no__t 0deploying 同盟伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[安裝 AD FS 角色服務](Install-the-AD-FS-Role-Service.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|設定同盟伺服器。|@no__t 0deploying 聯盟伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定同盟伺服器](Configure-a-Federation-Server.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|選擇性步驟：使用裝置註冊服務設定同盟伺服器 \(DRS @ no__t-1。|@no__t 0deploying 的同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用裝置註冊服務設定同盟伺服器](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|將主機 \(A @ no__t-1 和 alias \(CNAME @ no__t-3 資源記錄新增至公司網域名稱系統 \(DNS @ no__t-5 （適用于 federation service 和 DRS）。|@no__t 0deploying 同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定同盟服務和 DRS 的公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|確認該同盟伺服器運作正常。|@no__t 0deploying 的同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[驗證同盟伺服器是否可運作](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>另請參閱  
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

