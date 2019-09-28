---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: 設定服務通訊憑證
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d0464853c73f88ed76545921ffc8a4bf8551c800
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408322"
---
# <a name="set-a-service-communications-certificate"></a>設定服務通訊憑證


Active Directory 同盟服務中的同盟伺服器 \(AD FS @ no__t-1 使用服務通訊憑證來保護 Web 服務流量，以進行安全通訊端層 \(SSL @ no__t-3 與 Web 用戶端或同盟伺服器的通訊proxy.

> [!NOTE]  
> 服務通訊憑證與 SSL 憑證不同。 若要變更 AD FS SSL 憑證，您必須使用 Powershell。 請遵循這[篇文章](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap)中的指導方針。


您可以使用下列程式，利用 AD FS Management snap @ no__t-0in 來變更服務通訊憑證。  

> [!NOTE]  
> AD FS Management snap @ no__t-0in 指的是同盟伺服器的伺服器驗證憑證，做為服務通訊憑證。  

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(HTTP：\/ \/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   

### <a name="to-set-a-service-communications-certificate"></a>設定服務通訊憑證  

1.  在 **開始** 畫面上，輸入**AD FS 管理**，然後按 enter。  

2.  在主控台樹中，按兩下 @ no__t-0click **Service**，然後按一下 [**憑證**]。  

3.  在 [**動作**] 窗格中，按一下 [**設定服務通訊憑證**] 連結。  

4.  在 [**選取服務通訊憑證**] 對話方塊中，流覽至您想要設定為服務通訊憑證的憑證檔案，選取憑證檔案，然後按一下 [**開啟**]。  

## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  

[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
