---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: "需要新增描述宣告"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0e388ef656d3b690da62b077cb9f9e678a771e64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-claim-description"></a>需要新增描述宣告

>適用於：Windows Server 2016、Windows Server 2012 R2

Account 合作夥伴組織中的系統管理員建立宣告代表使用者的成員資格群組中的角色或代表部分的使用者，例如使用者的員工驗證數字的相關資料。

在資源合作夥伴組織中，系統管理員建立對應宣告來表示群組和被視為資源使用者的使用者。 因為資源合作夥伴資源合作夥伴組織，連入宣告 account 合作夥伴公司地圖中傳出宣告都能接受 account 協力廠商提供的認證。 

您可以使用下列程序新增理賠要求。

資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。

## <a name="to-add-a-claim-description"></a>若要新增宣告描述

1. 在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。 

2.  展開**服務**和按右鍵**新增宣告描述**。
![新增宣告描述](media\Add-a-Claim-Description\claimdesc1.png)

3.  在 [新增取得描述對話方塊中，在**顯示名稱**，輸入群組或本理賠要求的角色所辨識的唯一名稱。

4.  新增**簡短名稱**。

5.  在**取得識別碼**，輸入 URI 群組或角色宣告將會使用您的相關聯的。

6.  在**描述**，輸入文字的最佳描述此宣告的用途。

7.  根據您的組織的需求，選取下列核取方塊，視需要此宣告發行至聯盟中繼資料：


    - 若要發行讓合作夥伴知道這台，可接受此宣告此宣告，請按一下 [**將此宣告聯盟中繼資料中發行為宣告類型可接受此同盟服務**。
    - 若要發行讓合作夥伴注意，此伺服器可以發出此宣告此宣告，請按一下**將此宣告聯盟中繼資料中發行為此同盟服務可以傳送宣告類型**。

8.  按一下**[確定]**。

![新增宣告描述](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>也了  
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md) 
