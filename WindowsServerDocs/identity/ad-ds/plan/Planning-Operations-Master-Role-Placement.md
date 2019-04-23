---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: 規劃設置操作主機角色
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870889"
---
# <a name="planning-operations-master-role-placement"></a>規劃設置操作主機角色

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 網域服務 (AD DS) 支援目錄資料，這表示任何網域控制站可以接受的目錄變更，並將變更複寫到所有其他網域控制站的多重主機的複寫。 不過，某些變更，例如結構描述修改，並不適合使用多重主機的方式執行。 基於這個理由特定網域控制站，稱為 操作主機保留角色負責接受某些特定的變更要求。  
  
> [!NOTE]  
> 操作主機角色持有者必須能夠寫入 Active Directory 資料庫中的某些資訊。 由於 Active Directory 資料庫的唯讀網域控制站 (RODC) 上的唯讀**Rodc 無法做為操作主機角色持有者**。  
  
三個的操作主機角色 （也稱為彈性單一主機操作或 FSMO） 存在於每個網域：  
  
- 主要網域控制站 (PDC) 模擬器操作主機會處理所有密碼更新。  

- 相對識別碼 (RID) 操作主機會維護網域全域的 RID 集區，並配置以確保網域中建立的所有安全性主體都都有唯一識別碼的所有網域控制站的本機 Rid 集區。  
- 指定網域的基礎結構操作主機會維護從其網域中的群組成員的其他網域的安全性主體的清單。  

除了三個網域層級操作主機角色，兩個操作主機角色存在於每個樹系：  
  
- 架構操作主機會控管結構描述的變更。  
- 網域命名操作主機新增並移除與樹系的網域和其他目錄分割 （例如，網域名稱系統 (DNS) 應用程式磁碟分割）。  
  
將裝載網路可靠性很高，其中的領域中這些操作主機角色的網域控制站，並確定 PDC 模擬器] 和 [RID 主機一致地提供。  
  
建立指定網域的第一個網域控制站時，會自動指派作業主機角色持有者。 （架構主機和網域命名主機） 的兩個樹系層級角色會指派給第一個樹系中建立的網域控制站。 颾魤 ㄛ （RID 主機、 基礎結構主機和 PDC 模擬器） 的三個網域層級角色會指派給網域中建立的第一個網域控制站。  
  
> [!NOTE]  
> 自動操作主機角色持有者指派都只有在建立新的網域時，當目前的角色持有者會降級時進行復原。 角色擁有者的所有其他變更都必須由系統管理員。  
  
這些自動作業主機角色指派會導致極高的 CPU 使用量在樹系或網域中建立第一個網域控制站上。 若要避免這個問題，您的樹系或網域中的各種網域控制站主機角色指派 （傳輸） 作業。 將主機操作主機角色，其中網路與可靠的區域中的網域控制站，其中操作主機可以存取樹系中所有其他網域控制站。  
  
您也應該指定待命 （替代） 操作主機的所有作業主要角色。 待命操作主機是，您無法轉移操作主機角色萬一失敗，原始的角色持有者的網域控制站。 請確定待命操作主機是實際的操作主機的直接複寫協力電腦。  
  
## <a name="planning-the-pdc-emulator-placement"></a>規劃 PDC 模擬器的位置

PDC 模擬器處理用戶端密碼變更。 只有一個網域控制站做為樹系中的每個網域中的 PDC 模擬器。  
  
即使所有網域控制站都升級為 Windows 2000，Windows Server 2003 和 Windows Server 2008，並在 Windows 2000 原生功能等級的網域作業，PDC 模擬器接收優先執行的密碼變更的複寫在網域中其他網域控制站。 如果密碼最近有所變更，該變更將會複寫到網域中的每個網域控制站的時間。 如果在其他網域控制站，因為不正確的密碼，登入驗證失敗，該網域控制站將驗證要求轉送至 PDC 模擬器再決定是否要接受或拒絕登入嘗試。  
  
將 PDC 模擬器放在包含大量的轉送作業，如有需要的密碼，該網域中使用者的位置。 此外，請確定該位置也已連線至其他位置來複寫延遲降至最低。  
  
工作表可協助您記載您打算將 PDC 模擬器，以及代表每個位置中的每個網域的使用者數目的相關資訊，請參閱工作輔助工具的 Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))、 下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，和開啟設置網域控制站 (DSSTOPO_4.doc)。  
  
您需要參考位置，您需要將 PDC 模擬器，當您部署地區網域的相關資訊。 如需部署地區網域的詳細資訊，請參閱[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="requirements-for-infrastructure-master-placement"></a>基礎結構的主要位置的需求  

基礎結構主機更新會新增至群組，在它自己網域中其他網域中的安全性主體的名稱。 比方說，如果使用者從一個網域的第二個網域中的群組成員，而且第一個網域中變更使用者的名稱，第二個網域不是通知使用者的名稱，必須更新群組的成員資格 清單中。 因為在一個網域中的網域控制站不複寫到另一個網域中的網域控制站的安全性主體，第二個網域永遠不會察覺到基礎結構主機沒有變更。  
  
基礎結構主機監視器不斷群組成員資格，尋求其他網域中的安全性主體。 如果找到，它會檢查以確認此資訊會在更新的安全性主體的網域。 如果資訊已過期，基礎結構主機會執行更新，並接著將這些變更複寫到其網域中其他網域控制站。  
  
此規則適用於兩種例外狀況。 首先，如果所有網域控制站都是通用類別目錄伺服器，裝載基礎結構主機角色的網域控制站是無意義的通用類別目錄複寫更新的資訊，不論它們所屬的網域。 其次，如果樹系只有一個網域，網域控制站裝載基礎結構主機角色是微不足道的因為來自其他網域的安全性主體不存在。  
  
請勿將基礎結構主機放上也是通用類別目錄伺服器的網域控制站。 如果基礎結構主機與通用類別目錄位於相同的網域控制站，將無法運作的基礎結構主機。 基礎結構主機將永遠不會尋找已過期; 的資料因此，它會在網域中的其他網域控制站永遠不會複寫任何變更。  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>操作主機位置的網路連線能力有限

請注意，如果您的環境具有中央位置或中樞站台，您可以在其中放置操作主機角色持有者，這些作業的可用性取決於特定網域控制站作業主要的角色持有者可能會受到影響。  
  
例如，假設組織建立站台 A，B，C，d。 站台連結之間存在 A 和 B，B 和 C，以及 C 和 d。 網路連線完全鏡像站台連結的網路連線能力。 在此範例中，所有操作主機角色位於站都台 A，並將選項**橋接所有站都台連結**未選取。  
  
雖然此設定會導致所有的站台之間成功複寫的操作主機角色函式具有下列限制：  
  
- C 和 D 的站台中的網域控制站無法存取 PDC 模擬器網站 A 中更新密碼，或檢查最近已更新的密碼。  
- C 和 D 的站台中的網域控制站無法存取站台的 Active Directory 安裝完成後取得的初始的 RID 集區，以及重新整理 RID 集區，因為它們耗盡中的 RID 主機。  
- C 和 D 的站台中的網域控制站無法加入或移除 directory、 DNS 或自訂應用程式磁碟分割。  
- C 和 D 的站台中的網域控制站無法進行結構描述變更。  
  
若要協助您規劃操作主機角色定位為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟設置網域控制站 (DSSTOPO_4.doc)。  
  
您必須參考此資訊，當您建立地區網域與樹系根網域。 如需部署樹系根網域的詳細資訊，請參閱 < 部署[部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。 如需部署地區網域的詳細資訊，請參閱[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  

## <a name="next-steps"></a>後續步驟

FSMO 角色定位的相關資訊，可以找到支援主題[FSMO 放置和 Active Directory 網域控制站上的最佳化](https://support.microsoft.com/help/223346)
