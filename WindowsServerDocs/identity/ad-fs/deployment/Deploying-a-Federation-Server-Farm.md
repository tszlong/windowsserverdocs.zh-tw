---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Windows Server 2012 R2 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0f8dca425f644952c36a289ec72651f6383e846
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192192"
---
# <a name="deploying-a-federation-server-farm"></a>部署同盟伺服器陣列


若要部署同盟伺服器陣列，請依序完成此檢查清單中的工作。 當參考連結將您導向到觀念性主題時，請在檢閱觀念性主題之後返回此檢查清單，然後再繼續此檢查清單中其餘的工作。  
  
![部署同盟的伺服器陣列](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：部署同盟伺服器陣列**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|檢閱重要的概念和考量，當您準備好要部署 Active Directory Federation Services \(AD FS\)。 **注意：** |![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Server 2012 R2 中 AD FS 設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||如果您決定要將 Microsoft SQL Server 用於 AD FS 設定存放區，請確實部署可運作的 SQL Server 執行個體。|[SQL Server](https://technet.microsoft.com/sqlserver) **警告：** 在 Windows Server 2012 R2 中，如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|將您的電腦加入 Active Directory 網域中。|![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[將電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|註冊安全通訊端層\(SSL\) AD FS 憑證。|![部署同盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Enroll an SSL Certificate for AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|安裝 AD FS 角色服務。|![部署同盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[安裝 AD FS 角色服務](Install-the-AD-FS-Role-Service.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|設定同盟伺服器。|![部署同盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定同盟伺服器](Configure-a-Federation-Server.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|選擇性步驟：使用裝置註冊服務設定同盟伺服器\(DRS\)。|![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用裝置註冊服務設定同盟伺服器](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|將主機新增\(A\)和別名\(CNAME\)資源記錄公司的網域名稱系統\(DNS\)同盟服務和 DRS。|![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定同盟服務和 DRS 的公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![部署同盟的伺服器陣列](media/icon_checkboxo.gif)|確認該同盟伺服器運作正常。|![部署同盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[確認同盟伺服器是否正確運作](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>另請參閱  
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

