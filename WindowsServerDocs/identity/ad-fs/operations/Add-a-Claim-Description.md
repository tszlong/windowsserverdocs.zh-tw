---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: 新增宣告描述
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1023ca7da02d2a1f6af42f68892dc4c5c8f1a2bf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444375"
---
# <a name="add-a-claim-description"></a>新增宣告描述


在帳戶夥伴組織，系統管理員會建立宣告來代表使用者的群組或角色的成員資格，或代表使用者，例如，使用者的員工識別碼有關的一些資料。

在資源夥伴組織中，系統管理員會建立對應的宣告，來表示群組和可辨認為資源使用者的使用者。 因為資源夥伴中帳戶夥伴組織的對應資源夥伴組織中的連入宣告連出宣告是可以接受帳戶夥伴所提供的認證。 

您可以使用下列程序，以新增宣告。

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-add-a-claim-description"></a>若要新增宣告描述

1. 在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。 

2. 依序展開**服務**在以滑鼠右鍵按一下**新增宣告描述**。
   ![新增宣告描述](media/Add-a-Claim-Description/claimdesc1.png)

3. 在 新增宣告描述對話方塊中，於**顯示名稱**，輸入群組或角色，此宣告識別的唯一名稱。

4. 新增**簡短名稱**。

5. 在 **宣告識別碼**，輸入群組或角色，您將使用的宣告相關聯的 URI。

6. 底下**描述**，輸入最能描述此宣告的用途的文字。

7. 根據您組織的需求，選取下列核取方塊，視需要將此宣告發佈至同盟中繼資料：


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. 按一下 [確定]  。

![新增宣告描述](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
