---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 附錄 A-檢閱重要的 AD DS 術語
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852019"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>附錄 A：檢閱重要的 AD DS 術語

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

下列詞彙是與 Windows Server 2008 Active Directory 網域服務 (AD DS) 的部署程序。  
  
## <a name="active-directory-domain"></a>Active Directory 網域  
管理單位中的電腦網路，為了管理方便起見，群組數個功能，包括下列：  
  
-   **整體網路使用者識別**。 在網域中，可以建立一次，然後參考已加入網域所在的樹系的任何電腦上使用者身分識別。 建立網域的網域控制站安全地儲存使用者帳戶和使用者認證，例如密碼或憑證。  
  
-   **驗證**： 網域控制站會提供使用者的驗證服務。 它們也會提供額外的授權資料，例如使用者群組成員資格。 系統管理員可以使用這些服務，來控制網路上資源的存取權。  
  
-   **信任關係**。 網域會延伸到他們自己的樹系中其他網域中的使用者驗證服務，透過自動的雙向信任。 網域也會延伸到其他樹系中的網域中的使用者驗證服務透過樹系信任或以手動方式建立的外部信任。  
  
-   **原則管理**。 網域會是範圍內的系統管理原則，例如密碼複雜性及密碼重複使用的規則。  
  
-   **複寫**。 網域會定義提供的資料，這是適合用來提供所需的服務，且已複寫網域控制站之間的目錄樹狀結構的資料分割。 如此一來，所有網域控制站都是在網域中的對等，並做為一個單位管理。  
  
## <a name="active-directory-forest"></a>Active Directory 樹系  
共用常見的邏輯結構，目錄結構描述，和網路設定，以及自動、 雙向、 可轉移的信任關係的一或多個 Active Directory 網域的集合。 每個樹系是單一執行個體的目錄中，也會定義安全性界限。  
  
## <a name="active-directory-functional-level"></a>Active Directory 功能等級  
設定 AD DS 可讓進階的全網域或全樹系 AD DS 功能。  
  
## <a name="migration"></a>移轉  
保留或修改的物件，並可存取新的網域中的特性時目標網域，將物件從來源網域的程序。  
  
## <a name="domain-restructure"></a>網域進行重建  
移轉程序，包括變更樹系的網域結構。 網域重建可能是彙總或加入的網域，而且它可以進行樹系之間或樹系中。  
  
## <a name="domain-consolidation"></a>網域彙總  
重新架構的程序，包括透過合併其內容與其他網域的內容來消除 AD DS 網域。  
  
## <a name="domain-upgrade"></a>網域升級  
升級至更新版本的目錄服務的目錄服務網域的程序。 這包括升級所有網域控制站上的作業系統和情況適用時引發的 AD DS 功能等級。  
  
## <a name="in-place-domain-upgrade"></a>就地升級網域  
升級指定的網域中的所有網域控制站的作業系統，例如，從 Windows Server 2003 升級到 Windows Server 2008，和如果適用的話，提高功能等級的網域，同時讓網域物件，例如使用者的程序和群組，在進行中。  
  
## <a name="forest-root-domain"></a>樹系根網域  
建立 Active Directory 樹系中的第一個網域。 此網域會自動指定為樹系根網域中。 它提供 Active Directory 樹系基礎結構的基礎。  
  
## <a name="regional-domain"></a>地區網域  
若要最佳化複寫流量的地理區域中建立的子網域。  
  


