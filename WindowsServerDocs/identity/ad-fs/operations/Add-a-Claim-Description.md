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
ms.openlocfilehash: 454d261aa520778a6129ac9809f53894937b036a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190137"
---
# <a name="add-a-claim-description"></a>新增宣告描述


在帳戶夥伴組織，系統管理員會建立宣告來代表使用者的群組或角色的成員資格，或代表使用者，例如，使用者的員工識別碼有關的一些資料。

在資源夥伴組織中，系統管理員會建立對應的宣告，來表示群組和可辨認為資源使用者的使用者。 因為資源夥伴中帳戶夥伴組織的對應資源夥伴組織中的連入宣告連出宣告是可以接受帳戶夥伴所提供的認證。 

您可以使用下列程序，以新增宣告。

若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-add-a-claim-description"></a>若要新增宣告描述

1. 在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。 

2.  依序展開**服務**在以滑鼠右鍵按一下**新增宣告描述**。
![新增宣告描述](media\Add-a-Claim-Description\claimdesc1.png)

3.  在 新增宣告描述對話方塊中，於**顯示名稱**，輸入群組或角色，此宣告識別的唯一名稱。

4.  新增**簡短名稱**。

5.  在 **宣告識別碼**，輸入群組或角色，您將使用的宣告相關聯的 URI。

6.  底下**描述**，輸入最能描述此宣告的用途的文字。

7.  根據您組織的需求，選取下列核取方塊，視需要將此宣告發佈至同盟中繼資料：


    - 若要發行此宣告，讓合作夥伴知道此伺服器可以接受此宣告，請按一下**同盟中繼資料中發行此宣告為可接受此 Federation Service 的宣告型別**。
    - 若要發行此宣告，讓合作夥伴知道此伺服器可以發出此宣告，請按一下**同盟中繼資料中發行此宣告為此同盟服務可傳送的宣告型別**。

8.  按一下 [確定]  。

![新增宣告描述](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
