---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: 將同盟伺服器新增至同盟伺服器陣列
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57c59241c36ba4ed657b83e7c691931f57978a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408528"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>將同盟伺服器新增至同盟伺服器陣列


安裝同盟服務角色服務並在電腦上設定所需的憑證之後，您就可以設定電腦成為同盟伺服器。 您可以使用下列程序，將電腦加入至新的同盟伺服器陣列。  
  
您可以使用 AD FS 同盟伺服器設定 Wizard 將電腦加入伺服器陣列。 當您使用此嚮導將電腦加入至現有的伺服器陣列時，電腦會設定為僅讀取\-AD FS 設定資料庫的複本，而且必須從主要同盟伺服器接收更新。  
  
> [!NOTE]  
> 若為同盟 Web 單一\-在 \(SSO\) 設計上登\-，您必須在帳戶夥伴組織中至少有一部同盟伺服器，且資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\(HTTP：\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>將同盟伺服器新增至同盟伺服器陣列  
  
1.  有兩種方式可以啟動 [AD FS 同盟伺服器設定向導]。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   同盟服務角色服務安裝完成後，請在中開啟 [AD FS 管理] 嵌入式\-管理單元，然後按一下 [**總覽**] 頁面或 [**動作**] 窗格中的 [ **AD FS 同盟伺服器設定]** 連結。  
  
    -   在安裝程式完成之後，請開啟 [Windows Explorer]，流覽至**C：\\Windows\\ADFS**資料夾，然後按兩下 [ **fsconfigwizard.exe**]\-。  
  
2.  在 [歡迎] 頁面上，確認已選取 [將同盟伺服器新增至現有的 Federation Service]，然後按一下 [下一步]。  
  
3.  如果您選取的 AD FS 資料庫已經存在，則會出現 [偵測**到現有的 AD FS 設定資料庫**] 頁面。 如果出現該頁面，按一下 **[刪除資料庫]** ，然後按一下 **[下一步]** 。  
  
    > [!CAUTION]  
    > 只有在您確定此 AD FS 資料庫中的資料不重要，或在生產同盟伺服器陣列中未使用時，才選取此選項。  
  
4.  在 **[指定主要同盟伺服器及服務帳戶]** 頁面的 **[主要同盟伺服器名稱]** 底下，輸入伺服器陣列中的主要同盟伺服器電腦名稱，然後按一下 **[瀏覽]** 。 在 **[瀏覽]** 對話方塊中，找出其他所有同盟伺服器在現有的同盟伺服器陣列中當作服務帳戶使用的網域帳戶，然後按一下 **[確定]** 。 輸入密碼並加以確認，然後按 **[下一步]** ：  
  
    > [!NOTE]  
    > 如需為同盟伺服器陣列指定服務帳戶的詳細資訊，請參閱[手動設定同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 同盟伺服器陣列中的每部同盟伺服器都必須指定相同的服務帳戶，伺服器陣列才能夠運作。 例如，如果建立的服務帳戶是 contoso\\ADFS2SVC，則您為同盟伺服器角色設定且將參與相同伺服器陣列的每部電腦，都必須在同盟伺服器設定 Wizard 的這個步驟中指定 contoso\\ADFS2SVC，讓伺服器陣列能夠運作。  
  
5.  在 [準備套用設定] 頁面上，檢閱詳細資料。 如果設定看起來是正確的，請按 **[下一步]** 開始使用這些設定來設定 AD FS。  
  
6.  在 [設定結果] 頁面上，檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

