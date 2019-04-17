---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: "AD FS 註冊 SSL 憑證"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>AD FS 註冊 SSL 憑證

>適用於：Windows Server 2016、Windows Server 2012 R2

Active Directory 同盟服務 \(AD FS\) 需要聯盟伺服器陣列中每個聯盟伺服器上的安全通訊端層 \(SSL\) 伺服器驗證憑證。 每個聯盟在伺服器上發電廠可相同的憑證。 您必須擁有憑證和它提供的私密金鑰。 例如，如果您有憑證，其私密金鑰.pfx 檔案中，您可以匯入檔案直接 Active Directory 同盟服務設定精靈。 這個 SSL 憑證必須包含下列動作：  
  
1.  主旨替代名稱與主體名稱必須包含您同盟服務名稱，例如 fs.contoso.com。  
  
2.  替代主體名稱必須包含值**enterpriseregistration**，例如後面由您的組織的使用者主體名稱 \(UPN\) 尾碼**enterpriseregistration.corp.contoso.com**。  
  
    > [!WARNING]  
    > 如果您想要讓裝置登記服務 \(DRS\) 的工作地點加入主題替代名稱指定。  
  
> [!IMPORTANT]  
> 如果您的組織都使用多個 UPN 尾碼，而且想要讓 DRS，SSL 憑證必須包含每個尾碼主題替代名稱的項目。  
  
## <a name="see-also"></a>也了
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署聯盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

