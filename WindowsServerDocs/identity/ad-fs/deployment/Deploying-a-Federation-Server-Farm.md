---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: "Windows Server 2012 R2 AD FS 部署指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>部署聯盟伺服器陣列

>適用於：Windows Server 2016、Windows Server 2012 R2

部署聯盟伺服器陣列，才能完成此訂單中的檢查清單中的工作。 當參考連結的概念主題將您、您檢視的概念主題，讓您可以繼續檢查清單中的其餘的工作後，回到此檢查清單。  
  
![部署聯盟的伺服器陣列](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 部署聯盟伺服器陣列**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|當您準備部署 Active Directory 同盟服務 \(AD FS\) 檢視重要的概念和考量。 **注意：**|![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在 Windows Server 2012 R2 AD FS 程式設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[了解主要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||如果您選擇使用您 AD FS 設定存放區的 Microsoft SQL Server，請確定要部署的功能 SQL Server 執行個體。|[SQL Server](https://technet.microsoft.com/sqlserver)**警告：**在 Windows Server 2012 R2 如果您想要建立 AD FS 發電廠 SQL Server 用來儲存您設定的資料，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012。|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|若要 Active Directory domain 加入您的電腦。|![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[加入網域的電腦](Join-a-Computer-to-a-Domain.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|AD fs 註冊安全通訊端層 \(SSL\) 憑證。|![部署聯盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[適用於 AD FS 註冊 SSL 憑證](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|安裝 AD FS 角色服務。|![部署聯盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[安裝 AD FS 角色服務](Install-the-AD-FS-Role-Service.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|設定聯盟伺服器。|![部署聯盟的伺服器陣列](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定聯盟伺服器](Configure-a-Federation-Server.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|步驟：的裝置登記服務 \(DRS\) 設定聯盟伺服器。|![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定聯盟伺服器裝置登記服務與](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|新增主機 \(A\) 和別名 \(CNAME\) 資源記錄同盟服務的公司網域名稱系統 \(DNS\) 和 DRS。|![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定 DRS 和同盟服務的公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![部署聯盟的伺服器陣列](media/icon_checkboxo.gif)|請確認聯盟伺服器操作。|![部署聯盟的伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[確認聯盟伺服器是操作](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>也了  
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

