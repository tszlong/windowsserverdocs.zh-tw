---
title: "移轉 AD FS 2.0 聯盟伺服器"
description: "AD FS 伺服器移轉到 Windows Server 2012 R2 提供相關資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>檢查 AD FS 2.0 移轉到 Windows Server 2012 R2

一旦您完成您以 Windows Server 2012 R2 的 Active Directory 同盟服務 (AD FS) 陣列伺服器移轉相同，您可以使用下列程序驗證聯盟伺服器您的作業。是的在相同網路的任何 client 可以瑞曲之戰 federtation 伺服器。  
  
成員資格**使用者**，**備份電信業者**、**進階使用者**、**系統管理員**或相當於、 在本機電腦上的最低需求完成此程序。
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>若要確認聯盟伺服器操作  
  
1.  打開瀏覽器視窗並在 [位址列中，輸入聯盟伺服器名稱，然後將它與附加`federationmetadata/2007-06/federationmetadata.xml`以瀏覽至同盟服務中繼資料端點。 例如， `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` 。  
  
如果您的瀏覽器視窗中，您可以看到聯盟伺服器中繼資料，而不需要任何 SSL 錯誤或警告，聯盟伺服器是操作。  
  
2.  您也可以登入頁面 AD FS 瀏覽 (加上同盟服務姓名`adfs/ls/idpinitiatedsignon.htm`，例如， `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`)。  這會顯示，您可以使用登入網域系統管理員認證登入頁面 AD FS。  
  
> [!IMPORTANT]
>  請確定您的瀏覽器設定來新增您同盟服務名稱信任聯盟伺服器角色 (例如， `https://fs.contoso.com`) 以瀏覽器的本機該處。  
  
## <a name="next-steps"></a>後續步驟
 [Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)  
 [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [移轉 AD FS 聯盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
