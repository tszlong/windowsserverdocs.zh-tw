---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Windows Server 2012 R2 AD FS 部署指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb4d13d13771d76a306a32988c0faa03dd01db49
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855461"
---
# <a name="deploying-a-federation-server-farm"></a>部署同盟伺服器陣列


若要部署同盟伺服器陣列，請依序完成此檢查清單中的工作。 當參考連結將您導向到觀念性主題時，請在檢閱觀念性主題之後返回此檢查清單，然後再繼續此檢查清單中其餘的工作。  
  
![部署同盟伺服器陣列](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：部署同盟伺服器**陣列  
  
||工作|參考|  
|-|--------|-------------|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|當您準備部署 Active Directory 同盟服務 \(AD FS\)時，請參閱重要的概念和考慮。 **注意：**|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在 Windows server 2012 R2 中](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)部署同盟伺服器陣列 AD FS 設計指南<p>![部署同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[瞭解重要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||如果您決定要將 Microsoft SQL Server 用於 AD FS 設定存放區，請確實部署可運作的 SQL Server 執行個體。|[SQL Server](https://technet.microsoft.com/sqlserver) **警告：** 在 Windows Server 2012 R2 中，如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|將您的電腦加入 Active Directory 網域中。|![部署同盟伺服器陣列將](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|為 AD FS 註冊安全通訊端層 \(SSL\) 憑證。|![部署同盟伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[時，請為 AD FS 註冊 SSL 憑證](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|安裝 AD FS 角色服務。|![部署同盟伺服器陣列時](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[，請安裝 AD FS 角色服務](Install-the-AD-FS-Role-Service.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|設定同盟伺服器。|![部署同盟伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定同盟伺服器](Configure-a-Federation-Server.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|選擇性步驟：使用裝置註冊服務設定同盟伺服器 \(DRS\)。|![部署同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用裝置註冊服務設定同盟伺服器](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|將主機 \(\) 和別名 \(CNAME\) 資源記錄新增到公司網域名稱系統 \(federation service 和 DRS 的 DNS\)。|![部署同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定同盟服務和 DRS 的公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![部署同盟伺服器陣列](media/icon_checkboxo.gif)|確認該同盟伺服器運作正常。|![部署同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[時，請確認同盟伺服器可運作](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>另請參閱  
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

