---
description: 深入瞭解：設定服務通訊憑證
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: 設定服務通訊憑證
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 4d5991240e33d6775f7b7744775bfc4608a15f9b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049076"
---
# <a name="set-a-service-communications-certificate"></a>設定服務通訊憑證


Active Directory 同盟服務 AD FS 中的同盟伺服器 \( \) 使用服務通訊憑證來保護 web 服務流量，以 \( 安全通訊端層 \) 與 Web 用戶端或同盟伺服器 proxy 的 SSL 通訊。

> [!NOTE]
> 服務通訊憑證與 SSL 憑證不同。 若要變更 AD FS SSL 憑證，您將需要使用 Powershell。 遵循 [本文](../operations/manage-ssl-certificates-ad-fs-wap.md)中的指導方針。


您可以使用下列程式，利用 AD FS 管理] 嵌入式管理單元來變更服務通訊憑證 \- 。

> [!NOTE]
> AD FS 管理嵌入式管理單元會 \- 將同盟伺服器的伺服器驗證憑證視為服務通訊憑證。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ 使用適當帳戶和群組成員資格的詳細資料：LinkId \= 83477 \) 。

### <a name="to-set-a-service-communications-certificate"></a>設定服務通訊憑證

1.  在 [ **開始** ] 畫面上，輸入 **AD FS 管理**]，然後按 enter。

2.  在主控台樹中，按兩下 \- [ **服務**]，然後按一下 [ **憑證**]。

3.  在 [ **動作** ] 窗格中，按一下 [ **設定服務通訊憑證** ] 連結。

4.  在 [ **選取服務通訊憑證** ] 對話方塊中，流覽至您要設定為服務通訊憑證的憑證檔案，選取憑證檔案，然後按一下 [ **開啟**]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)
