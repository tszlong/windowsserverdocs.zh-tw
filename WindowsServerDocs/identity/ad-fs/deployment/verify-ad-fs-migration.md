---
title: 移轉 AD FS 2.0 同盟伺服器
description: 提供移轉到 Windows Server 2012 R2 的 AD FS 伺服器的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 38e44ab2f803d8ec8940dbba7574a9f37389112a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444458"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>確認 AD FS 2.0 移轉到 Windows Server 2012 R2

一旦您完成相同的伺服器移轉到 Windows Server 2012 R2 Active Directory 同盟服務 (AD FS) 伺服器陣列時，您可以使用下列程序來驗證您的伺服器陣列中的同盟伺服器正常運作;也就是在相同網路上的任何用戶端可以連線到您 federtation 伺服器。  
  
本機電腦上 **Users**、**Backup Operators**、**Power Users**、**Administrators** 或具有同等權限的成員資格，至少需要完成此程序。
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>確認該同盟伺服器運作正常  
  
1.  開啟瀏覽器視窗並在 [網址] 列中，輸入同盟伺服器名稱，然後將它與附加`federationmetadata/2007-06/federationmetadata.xml`瀏覽到 federation service 中繼資料端點。 比方說， `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` 。  
  
如果在瀏覽器視窗終可看到同盟伺服器中繼資料，不會出現任何 SSL 錯誤或警告，則您的同盟伺服器便是運作正常。  
  
2. 您也可以瀏覽到 AD FS 登入頁面 (Federation Service 名稱後面加上 `adfs/ls/idpinitiatedsignon.htm`，例如，`https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`)。  這會顯示 AD FS 登入頁面，您可以使用網域系統管理員認證登入。  
  
> [!IMPORTANT]
>  確定透過將您的 Federation Service 名稱 (例如，`https://fs.contoso.com`) 新增至瀏覽器內部網路區域的方式，將您的瀏覽器設定為信任同盟伺服器角色。  
  
## <a name="next-steps"></a>後續步驟
 [將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [準備移轉 AD FS 同盟伺服器](prepare-migrate-ad-fs-server-r2.md)  
 [移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [移轉 AD FS 同盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
