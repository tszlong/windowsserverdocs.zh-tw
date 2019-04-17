---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: "規劃作業角色位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>規劃作業角色位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 支援多主機複寫 directory 資料，這表示任何網域控制站能接受 directory 變更及其他網域控制站複製所做的變更。 不過，特定的變更，架構修改，例如一些實用多重的方式執行。 基於這個原因稱為操作主機，某些網域控制站按住角色負責接受要求的某些特定的變更。  
  
> [!NOTE]  
> 操作主角持有必須 Active Directory 資料庫寫入一些資訊。 唯讀網域控制站 (RODC) 上的 Active Directory 資料庫唯讀狀態，因為 Rodc 無法做為作業主角位置。  
  
每個網域中，有三種操作主要角色（也稱為彈性的單一主機操作或 FSMO）：  
  
-   主要網域控制站 (PDC) 模擬器操作主機處理密碼的所有更新。  
  
-   相對 ID (RID) 操作主機全球 RID 集區的網域和配置的區域以確保擁有的唯一建立網域中的所有安全性原則的所有網域控制站 Rid 集區。  
  
-   指定網域的基礎結構作業主機維護來自其他成員的其網域中的群組網域的安全性原則的清單。  
  
三個網域層級操作主機角色，除了每個森林中有兩項作業主要的角色：  
  
-   架構操作主機管理變更結構描述。  
  
-   網域命名操作主機新增並的樹系移除網域和其他 directory 磁碟分割（例如網域名稱系統」(DNS) 應用程式的磁碟分割）。  
  
放置裝載網路可靠性不高] 區域中的這些操作主機角色網域控制站和確保肯定和 RID 主機一致的可用。  
  
建立特定的網域中的第一個網域控制站時，會自動指定作業主角位置。 建立森林中的第一個網域控制站指派兩種層級的樹系角色（架構主機和網域命名主機）。 此外，以建立網域中的第一個網域控制站指派三個網域層級角色（RID 主機、的基礎結構主機和肯定）。  
  
> [!NOTE]  
> 自動作業主角擁有者設定的進行只會建立新的網域和時降級目前的角色擁有者。 所有其他角色擁有者變更必須系統管理員的身分由車載機起始。  
  
非常高 CPU 使用率可能造成這些自動作業主角指派樹系或網域中建立的第一個網域控制站。 若要避免這個問題，您的樹系或網域中的各種網域控制站主機角色指派（傳輸）作業。 放置主機作業主機的網路不可靠區域中的角色網域控制站和位置操作主機可以存取所有其他網域控制站樹系。  
  
您也應該指定待命（替代）操作所有作業的主機主要角色。 待命操作主機的網域控制站的您可能會傳送操作主機角色以防原始的角色持有失敗。 確定已複寫直接合作夥伴的實際操作主機待命操作主機。  
  
## <a name="planning-the-pdc-emulator-placement"></a>規劃 PDC 模擬器位置  
肯定處理 client 變更密碼。 只有一個網域控制站做為每個網域中的樹系肯定。  
  
即使網域控制站升級至 Windows 2000、Windows Server 2003 及 Windows Server 2008、Windows 2000 的原生功能層級操作網域，肯定接收執行其他網域中的網域控制站密碼變更的優先複寫。 如果您最近變更密碼，這項變更花一些時間複寫網域中的每個網域控制站。 如果登入驗證失敗，在另一部網域控制站因為不正確密碼，該網域控制站轉送給肯定驗證要求之前決定接受或拒絕嘗試登入。  
  
將肯定中有很多的使用者該網域視轉寄作業密碼的位置。 此外，確定位置也連接到最小化複寫延遲其他位置。  
  
試算表中列出您想要放置 PDC 模擬器和使用者的每個網域在每個位置的相關資訊來協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並左網域控制站位置 (DSSTOPO_4.doc)。  
  
您需要將您要部署區域網域時，將 PDC 模擬器位置的相關資訊。 如需部署網域區域的相關資訊，請查看[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="requirements-for-infrastructure-master-placement"></a>適用於基礎結構主要位置的需求  

基礎結構主機更新安全性原則從其他群組自己網域中加入的網域的名稱。 例如，如果使用者網域中的第二個網域中的群組成員，以及變更的第一個網域中的使用者名稱，第二個網域不會告知必須更新群組成員資格清單中的使用者名稱。 因為一個網域中的網域控制站不到另一個網域中的網域控制站複寫安全性原則，第二個網域永遠不會變得注意到的基礎結構主機缺少的變更。  
  
基礎結構主顯示器持續群組成員資格，尋找其他網域的安全性原則。 如果找到它，它就會檢查與驗證的資訊更新的安全性原則的網域。 如果是最新的資訊，請基礎結構主機執行更新，並再複製到其他網域控制站其網域中的 [變更。  
  
此規則適用於兩個例外。 首先，如果所有網域控制站伺服器通用，裝載基礎結構主角網域控制站是不重要因為全球目錄複寫更新無論及其所屬的網域資訊。 第二，如果樹系只有一個網域，裝載基礎結構主角網域控制站是不重要因為來自其他網域的安全性原則不存在。  
  
不要將基礎結構主機放網域控制站也是一個通用伺服器上。 如果通用與基礎結構主機上相同的網域控制站，基礎結構主機將無法運作。 基礎結構主機一律不會尋找資料已過期。因此，它會網域中的其他網域控制站不會複寫的任何變更。  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>操作有限連接的主要網路位置  
請注意，如果您的環境確實中央位置或中樞中，就可以作業主角持有的網站，這些作業的可用性而定某些網域控制站作業主要持有可能會受到影響的角色。  
  
例如，假設公司所建立的網站，B C，並 D.網站連結存在之間和 B B 和 C，以及之間 C 和 D.網路連接完全鏡射網路連接的網站連結。 在此範例中，所有作業主機角色位於都網站和選項**所有都網站的連結，ios 都橋接器**取消選取。  
  
此設定會導致之間所有網站的成功複寫，雖然作業主角功能有以下限制：  
  
-   在 [網站 C 和 D 網域控制站無法存取肯定 A 網站來更新密碼，或是來檢查有最近已更新的密碼。  
  
-   在 [網站 C 和 D 網域控制站無法存取 RID 主機網站 A 安裝 Active Directory 之後，取得初始 RID 集區，並為他們成為耗盡重新整理 RID 集區中。  
  
-   在網站 C 和 D 網域控制站無法新增或移除 directory、DNS 或自訂應用程式的磁碟分割。  
  
-   在 [網站 C 和 D 網域控制站無法變更結構描述。  
  
試算表來協助您計畫作業主角位置，會看到輔助適用於[Windows Server 2003 部署套件](https://go.microsoft.com/fwlink/?LinkID=102558)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並左網域控制站位置 (DSSTOPO_4.doc)。  
  
您將需要當您建立的樹系根網域和區域網域參考這項資訊。 如需有關部署森林根網域中，查看部署[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。 如需部署網域區域的相關資訊，請查看[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  


