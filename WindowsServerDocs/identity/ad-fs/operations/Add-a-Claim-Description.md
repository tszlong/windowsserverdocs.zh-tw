---
description: 深入瞭解：新增宣告描述
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: 新增宣告描述
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: bdf57eddd1ad76c5bf9217e9aab97cfef9332b61
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044296"
---
# <a name="add-a-claim-description"></a>新增宣告描述


在帳戶夥伴組織中，系統管理員會建立宣告來代表使用者在群組或角色中的成員資格，或代表使用者的一些資料，例如使用者的員工識別碼。

在資源夥伴組織中，系統管理員會建立對應的宣告，以代表可辨識為資源使用者的群組和使用者。 由於帳戶夥伴組織中的連出宣告對應至資源夥伴組織中的傳入宣告，因此資源夥伴可以接受帳戶夥伴所提供的認證。

您可以使用下列程式來新增宣告。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

## <a name="to-add-a-claim-description"></a>新增宣告描述

1. 在 [伺服器管理員] 中按一下 [工具]，然後選取 [AD FS 管理]。

2. 展開 [ **服務** ]，以滑鼠右鍵按一下 [ **新增宣告描述**]。
   ![新增宣告描述](media/Add-a-Claim-Description/claimdesc1.png)

3. 在 [新增宣告描述] 對話方塊的 [ **顯示名稱**] 中，輸入識別此宣告之群組或角色的唯一名稱。

4. 新增 **簡短名稱**。

5. 在 [宣告 **識別碼**] 中，輸入與您將使用之宣告的群組或角色相關聯的 URI。

6. 在 [ **描述**] 底下，輸入最能描述此宣告用途的文字。

7. 視組織的需求而定，選取下列其中一個核取方塊，將此宣告發布至同盟中繼資料：


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. 按一下 [確定]。

![新增宣告描述](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)
