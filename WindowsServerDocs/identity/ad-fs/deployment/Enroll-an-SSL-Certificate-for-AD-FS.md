---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: 註冊 AD FS 的 SSL 憑證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e80927f2670614d2949f4e67cc158319f05c5fa0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192149"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>註冊 AD FS 的 SSL 憑證

Active Directory Federation Services \(AD FS\)需要安全通訊端層憑證\(SSL\)同盟伺服器陣列中每部同盟伺服器上的伺服器驗證。 伺服陣列中每部同盟伺服器上，可以使用相同的憑證。 您必須準備好憑證和私密金鑰。 例如，如果 .pfx 檔案內有憑證和私密金鑰，您可以直接將該檔案匯入 Active Directory Federation Services 設定精靈。 此 SSL 憑證必須包含下列內容：  
  
1.  主體名稱和主體別名必須包含同盟服務名稱，例如 fs.contoso.com。  
  
2.  主體別名必須包含值**enterpriseregistration** ，且後面跟著 「 使用者主體名稱\(UPN\)組織的尾碼，比方說， **enterpriseregistration.corp.contoso.com**。  
  
    > [!WARNING]  
    > 指定主體替代名稱，如果您打算啟用 Device Registration Service \(DRS\) Workplace join。  
  
> [!IMPORTANT]  
> 如果您的組織使用多個 UPN 尾碼，而且您打算啟用 DRS，SSL 憑證必須包含每個尾碼的主體替代名稱的項目。  
  
## <a name="see-also"></a>另請參閱
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

