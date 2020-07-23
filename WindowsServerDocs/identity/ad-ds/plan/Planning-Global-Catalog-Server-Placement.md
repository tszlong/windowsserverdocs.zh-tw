---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: 規劃設置通用類別目錄伺服器
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5a67e85a239036851d628ad1c261a21f6fbc4566
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959430"
---
# <a name="planning-global-catalog-server-placement"></a>規劃設置通用類別目錄伺服器

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

除非您有單一網域樹系，否則通用類別目錄位置需要進行規劃。 在單一網域樹系中，將所有網域控制站設定為通用類別目錄伺服器。 由於每個網域控制站會在樹系中儲存唯一的網域目錄分割，因此將每個網域控制站設定為通用類別目錄伺服器不需要任何額外的磁碟空間使用量、CPU 使用量或複寫流量。 在單一網域樹系中，所有網域控制站都扮演虛擬通用類別目錄伺服器;也就是說，它們全都可以回應任何驗證或服務要求。 此特殊條件適用于單一網域樹系的設計。 當有多個網域時，驗證要求並不需要聯繫通用類別目錄伺服器，而使用者可以是存在於不同網域中之萬用群組的成員。 不過，只有指定為通用類別目錄伺服器的網域控制站可以回應通用類別目錄埠3268上的通用類別目錄查詢。 為了簡化此案例中的管理，並確保回應一致，將所有網域控制站指定為通用類別目錄伺服器，可免除哪些網域控制站可以回應通用類別目錄查詢的顧慮。 具體而言，每當使用者使用 Start\Search\For 人員或尋找印表機或展開萬用群組時，這些要求只會移至通用類別目錄。

在多網域樹系中，通用類別目錄伺服器有助於使用者登入要求和整個樹系搜尋。 下圖顯示如何判斷哪些位置需要通用類別目錄伺服器。

![規劃 gc 放置](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)

在大部分情況下，建議您在安裝新的網域控制站時包含通用類別目錄。 下列情況則例外：

- 有限頻寬：在遠端網站中，如果遠端網站與中樞網站之間的廣域網路（WAN）連結受到限制，您可以使用遠端網站中的萬用群組成員資格快取，以配合網站中使用者的登入需求。
- 基礎結構操作主機角色不相容：除非網域中的所有網域控制站都是通用類別目錄伺服器，或樹系只有一個網域，否則請勿將通用類別目錄放在裝載網域中基礎結構操作主機角色的網域控制站上。

## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>根據應用程式需求新增通用類別目錄伺服器

某些應用程式（例如 Microsoft Exchange、訊息佇列（也稱為 MSMQ））和使用 DCOM 的應用程式不會透過潛在的 WAN 連結提供適當的回應，因此需要高可用性的通用類別目錄基礎結構，以提供低查詢延遲。 判斷在較慢的 WAN 連結上執行效能不佳的應用程式是否在位置中執行，或位置是否需要 Microsoft Exchange Server。 如果您的位置包含未透過 WAN 連結提供適當回應的應用程式，您必須在該位置放置通用類別目錄伺服器，以減少查詢延遲。

> [!NOTE]
> 唯讀網域控制站（Rodc）可以成功升級為通用類別目錄伺服器狀態。 不過，某些啟用目錄的應用程式無法支援 RODC 做為通用類別目錄伺服器。 例如，Microsoft Exchange Server 的任何版本都不會使用 Rodc。 不過，只要有可寫入的網域控制站，Microsoft Exchange Server 就能在包含 Rodc 的環境中運作。 Exchange Server 2007 實際上會忽略 Rodc。 Exchange Server 2003 也會忽略預設情況下的 Rodc，Exchange 元件會自動偵測可用的網域控制站。 Exchange Server 2003 未進行任何變更，因此無法得知唯讀目錄伺服器。 因此，嘗試強制 Exchange Server 2003 服務和管理工具使用 Rodc 可能會導致無法預期的行為。

## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>為大量使用者新增通用類別目錄伺服器

將通用類別目錄伺服器放在包含超過100個使用者的所有位置，以減少網路 WAN 連結的擁塞，並避免發生 WAN 連結失敗時的生產力損失。

## <a name="using-highly-available-bandwidth"></a>使用高可用性頻寬

您不需要將通用類別目錄放在不包含需要通用類別目錄伺服器之應用程式的位置上，包括少於100個使用者，而且也會連線到另一個位置，其中包含可供 Active Directory Domain Services （AD DS）100% 的 WAN 連結的通用類別目錄伺服器。 在此情況下，使用者可以透過 WAN 連結存取通用類別目錄伺服器。

當漫遊使用者第一次在任何位置登入時，就必須聯絡通用類別目錄伺服器。 如果無法接受 WAN 連結的登入時間，請將通用類別目錄放在大量漫遊使用者所造訪的位置。

## <a name="enabling-universal-group-membership-caching"></a>啟用萬用群組成員資格快取

對於包含少於100個使用者，且不包含大量漫遊使用者或需要通用類別目錄伺服器之應用程式的位置，您可以部署執行 Windows Server 2008 的網域控制站，並啟用萬用群組成員資格快取。 確定通用類別目錄伺服器不是來自已啟用萬用群組成員資格快取之網域控制站的一個以上複寫躍點，因此可以重新整理快取中的萬用群組資訊。 如需萬用群組快取運作方式的相關資訊，請參閱[通用類別目錄的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc737410(v=ws.10))一文。

如需協助您記載規劃放置萬用群組快取之通用類別目錄伺服器和網域控制站的工作表，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟網域控制站放置（DSSTOPO_4.doc）。 當您部署樹系根域和地區網域時，請參閱您需要放置通用類別目錄伺服器之位置的相關資訊。
