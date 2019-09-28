---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 使用群組原則將憑證散發到用戶端電腦
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359616"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則將憑證散發到用戶端電腦


您可以使用下列程式，向下推送適當的安全通訊端層 \(SSL @ no__t-1 憑證 \(or 對應至帳戶同盟伺服器、資源同盟伺服器和之受根信任 @ no__t-3 的對等憑證使用群組原則，將 Web 服務器加入帳戶夥伴樹系中的每部用戶端電腦。  
  
若要完成此程式，至少需要**Domain admins**或**Enterprise Admins**的成員資格或同等許可權（在 ACTIVE DIRECTORY DOMAIN SERVICES 中 @no__t 2AD DS @ no__t-3）。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(HTTP：\/ \/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>若要使用群組原則將憑證散發到用戶端電腦  
  
1.  在帳戶夥伴組織樹系中的網域控制站上，啟動**群組原則 Management** snap @ no__t-1in。  
  
2.  尋找現有的群組原則物件 \(GPO @ no__t-1，或建立新的 GPO 以包含憑證設定。 確定 GPO 與網域、網站或組織單位相關聯 \(OU @ no__t-1，其中適當的使用者和電腦帳戶位於其中。  
  
3.  Right @ no__t-0click GPO，然後按一下 [**編輯**]。  
  
4.  在主控台樹中，開啟 [**電腦設定] @ no__t-1Policies @ no__t-2Windows Settings @ no__t-3Security Settings @ no__t-4Public 金鑰原則**，right @ no__t-5Click**受信任的根憑證授權**單位，然後按一下 [匯**入]** .  
  
5.  在 [**歡迎使用憑證匯入嚮導]** 頁面上，按 **[下一步]** 。  
  
6.  在 [**要匯入**的檔案] 頁面上，輸入適當憑證檔案的路徑 \(for 範例中，\\ @ no__t-3fs1 @ no__t-4c $ \\fs1 .cer @ no__t-6，然後按 **[下一步]** 。  
  
7.  在 [**憑證存放區**] 頁面上，按一下 **[將所有憑證放入下列存放區**]，然後按 **[下一步]** 。  
  
8.  在 [**正在完成憑證匯入嚮導]** 頁面上，確認您提供的資訊正確無誤，然後按一下 **[完成]** 。  
  
9. 重複步驟2到6，為伺服器陣列中的每部同盟伺服器新增額外的憑證。  
