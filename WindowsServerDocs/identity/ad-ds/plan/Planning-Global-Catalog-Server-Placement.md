---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: 規劃設置通用類別目錄伺服器
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820679"
---
# <a name="planning-global-catalog-server-placement"></a>規劃設置通用類別目錄伺服器

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

通用類別目錄放置需要規劃以外，如果您有單一網域樹系。 在單一網域樹系中，設定所有網域控制站為通用類別目錄伺服器。 因為每個網域控制站的樹系中儲存唯一的網域目錄磁碟分割，設定每個網域控制站為通用類別目錄伺服器不需要任何額外的磁碟空間使用量、 CPU 使用量或複寫流量。 在單一網域樹系中所有網域控制站都做為虛擬的通用類別目錄伺服器;也就是說，它們可以所有回應任何驗證或服務要求。 這個特別的條件，適用於單一網域樹系是預設行為。 驗證要求並不需要連絡通用類別目錄伺服器，當有多個網域，而且使用者可以是位於不同網域的萬用群組的成員時一樣。 不過，指定為通用類別目錄伺服器的網域控制站可以回應通用類別目錄連接埠 3268 上的通用類別目錄查詢。 若要簡化管理工作，在此案例中，並確保一致的回應，並將所有網域控制站都指定為通用類別目錄伺服器就不需網域控制站可以回應通用類別目錄查詢的考量。 具體來說，每當使用者使用 Start\Search\For 人員或尋找印表機，或展開萬用群組，這些要求會傳送只至通用類別目錄。  
  
在多重網域的樹系通用類別目錄伺服器會促進使用者登入要求和全樹系搜尋。 下圖顯示如何判斷哪些位置需要通用類別目錄伺服器。  
  
![規劃 gc 的位置](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
在大部分情況下，建議您包含 通用類別目錄，當您安裝新網域控制站。 適用於下列情況除外：  
  
- 有限的頻寬：在遠端站台，如果遠端站台與中樞站台之間的廣域網路 (wan) 連結有限，您可以使用萬用群組成員資格快取在遠端站台以容納站台中的使用者的登入需求。  
- 基礎結構操作主機角色不相容：請勿將通用類別目錄放在主機基礎結構作業主要角色的網域中，除非網域中的所有網域控制站都是通用類別目錄伺服器，或樹系只有一個網域的網域控制站上。  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>新增應用程式的需求為基礎的通用類別目錄伺服器

某些應用程式，例如 Microsoft Exchange、 Message Queuing (也稱為 MSMQ)，以及使用 DCOM 應用程式不提供適當的回應潛在的 WAN 連結，並因此需要高度可用的通用類別目錄基礎結構提供低的查詢延遲。 決定是否透過慢速 WAN 連結，請執行效能很差的任何應用程式正在執行中的位置或位置是否需要 Microsoft Exchange Server。 如果您的位置包含應用程式不會透過 WAN 連結提供適當的回應，您必須以減少查詢延遲的位置放置通用類別目錄伺服器。  
  
> [!NOTE]  
> 唯讀網域控制站 (Rodc) 可以成功升級為通用類別目錄伺服器的狀態。 不過，特定目錄的應用程式不支援做為通用類別目錄伺服器的 RODC。 例如，任何版本的 Microsoft Exchange Server 會不使用 Rodc。 不過，Microsoft Exchange Server 適用於包含 Rodc 的環境中，只要有可用的可寫入網域控制站。 Exchange Server 2007 實際上會忽略 Rodc。 Exchange Server 2003 也會忽略 Exchange 元件會自動偵測可用的網域控制站的預設條件中的 Rodc。 至 Exchange Server 2003 所不做任何變更，讓它知道唯讀目錄伺服器。 因此，嘗試強制 Exchange Server 2003 服務和管理工具，可使用 Rodc 可能會導致無法預期的行為。  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>新增大量使用者的通用類別目錄伺服器

將通用類別目錄伺服器放在包含 100 個以上的使用者，以降低網路的 WAN 連結的壅塞並避免產能的損失 WAN 連結失敗時的所有位置。  
  
## <a name="using-highly-available-bandwidth"></a>使用高度可用的頻寬

您不需要將通用類別目錄放置的位置不包含需要通用類別目錄伺服器的應用程式、 包含少於 100 個使用者，而且也會連接到另一個位置，其中包含 通用類別目錄伺服器的 100%的 WAN 連結可用的 Active Directory 網域服務 (AD DS)。 在此情況下，使用者可以存取透過 WAN 連結的通用類別目錄伺服器。  
  
漫遊使用者必須連絡通用類別目錄伺服器，無論何時登入第一次在任何位置。 如果無法接受透過 WAN 連結的登入時間，在瀏覽大量漫遊使用者的位置放置通用類別目錄。  
  
## <a name="enabling-universal-group-membership-caching"></a>啟用萬用群組成員資格快取

包含少於 100 個使用者，且不含大量漫遊的使用者或需要通用類別目錄伺服器的應用程式的位置，您可以部署執行 Windows Server 2008，並啟用萬用群組成員資格的網域控制站快取。 請確定通用類別目錄伺服器不是網域控制站的萬用群組成員資格快取已啟用，以便可以重新整理快取中的萬用群組資訊的多個複寫躍點。 如需如何萬用群組快取的運作資訊，請參閱文章[通用類別目錄的運作方式](https://go.microsoft.com/fwlink/?LinkId=107063)。  
  
為了協助您記載您打算啟用萬用群組快取中放置通用類別目錄伺服器和網域控制站為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟設置網域控制站 (DSSTOPO_4.doc)。 請參閱您要放置通用類別目錄伺服器，當您部署地區網域與樹系根網域的位置的相關資訊。  
