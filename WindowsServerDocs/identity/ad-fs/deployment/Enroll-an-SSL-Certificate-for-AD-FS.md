---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: 註冊 AD FS 的 SSL 憑證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2d24e5593af2d12359d4e2acd5fc93df334730b8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962822"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>註冊 AD FS 的 SSL 憑證

Active Directory 同盟服務 \( AD FS \) 需要憑證，以在同盟伺服器陣列中的每部同盟伺服器上進行安全通訊端層 \( SSL \) 伺服器驗證。 伺服器陣列中的每部同盟伺服器都可以使用相同的憑證。 您必須準備好憑證和私密金鑰。 例如，如果 .pfx 檔案內有憑證和私密金鑰，您可以直接將該檔案匯入 Active Directory Federation Services 設定精靈。 此 SSL 憑證必須包含下列內容：

1.  [主體名稱] 和 [主體替代名稱] 必須包含您的同盟服務名稱，例如 fs.contoso.com。

2.  主體別名必須包含值**enterpriseregistration** ，後面接著您組織的使用者主體名稱 \( UPN \) 尾碼，例如**enterpriseregistration.corp.contoso.com**。

    > [!WARNING]
    > 如果您打算啟用裝置註冊服務 \( DRS 以進行 Workplace Join，請指定主體別名 \) 。

> [!IMPORTANT]
> 如果您的組織使用多個 UPN 尾碼，而且您打算啟用 DRS，則 SSL 憑證必須包含每個尾碼的主體別名專案。

## <a name="see-also"></a>另請參閱
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)



