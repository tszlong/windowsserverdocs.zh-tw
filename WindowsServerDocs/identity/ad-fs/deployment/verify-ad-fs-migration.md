---
title: 遷移 AD FS 2.0 同盟伺服器
description: 提供將 AD FS 伺服器遷移至 Windows Server 2012 R2 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 024f7a586c58dcf5f0d6f9c3aa291e6bef8d838b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408184"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>確認 AD FS 2.0 遷移至 Windows Server 2012 R2

當您完成 Active Directory 同盟服務（AD FS）伺服器陣列到 Windows Server 2012 R2 的相同伺服器遷移之後，您可以使用下列程式來確認伺服器陣列中的同盟伺服器是否可運作;也就是說，相同網路上的任何用戶端都可以連接到您的 federtation 伺服器。  
  
本機電腦上 **Users**、**Backup Operators**、**Power Users**、**Administrators** 或具有同等權限的成員資格，至少需要完成此程序。
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>確認該同盟伺服器運作正常  
  
1.  開啟瀏覽器視窗，然後在網址列中輸入同盟伺服器名稱，再附加 `federationmetadata/2007-06/federationmetadata.xml` 以流覽至 federation service 中繼資料端點。 例如，`https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml`。  
  
如果在瀏覽器視窗終可看到同盟伺服器中繼資料，不會出現任何 SSL 錯誤或警告，則您的同盟伺服器便是運作正常。  
  
2. 您也可以瀏覽到 AD FS 登入頁面 (Federation Service 名稱後面加上 `adfs/ls/idpinitiatedsignon.htm`，例如，`https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`)。  這會顯示 AD FS 登入頁面，您可以使用網域系統管理員認證登入。  
  
> [!IMPORTANT]
>  請務必將您的同盟服務名稱（例如 `https://fs.contoso.com`）新增至瀏覽器的近端內部網路區域，以將您的瀏覽器設定設為信任同盟伺服器角色。  
  
## <a name="next-steps"></a>後續步驟
 [將 Active Directory 同盟服務角色服務遷移至 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [準備遷移 AD FS 同盟伺服器](prepare-migrate-ad-fs-server-r2.md)  
 [遷移 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [遷移 AD FS 同盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
