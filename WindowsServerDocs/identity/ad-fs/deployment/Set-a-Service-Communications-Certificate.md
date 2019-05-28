---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: 設定服務通訊憑證
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 140e8e4204148dd8862385054554d7b8336856ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192007"
---
# <a name="set-a-service-communications-certificate"></a>設定服務通訊憑證


在 Active Directory Federation Services 中的同盟伺服器\(AD FS\)使用的服務通訊憑證來保護網頁服務流量的安全通訊端層\(SSL\)與 Web 之間的通訊用戶端或同盟伺服器 proxy。

> [!NOTE]  
> 服務通訊憑證不是將 SSL 憑證相同。 若要變更 AD FS SSL 憑證，您必須使用 Powershell。 請遵循本章[文章](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap)。


您可以使用下列程序來變更服務通訊憑證，使用 AD FS 管理嵌入式管理單元\-中。  

> [!NOTE]  
> AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。  

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   

### <a name="to-set-a-service-communications-certificate"></a>若要設定服務通訊憑證  

1.  在 **開始**畫面上，輸入**AD FS 管理**，然後按 ENTER 鍵。  

2.  在主控台樹狀目錄中，按兩下\-按一下 **服務**，然後按一下**憑證**。  

3.  在 **動作**窗格中，按一下**設定服務通訊憑證**連結。  

4.  在 **中選取服務通訊憑證**對話方塊方塊中，瀏覽至您想要將設定為服務通訊憑證，選取憑證檔案，然後按一下 憑證檔案**開啟**.  

## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  

[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
