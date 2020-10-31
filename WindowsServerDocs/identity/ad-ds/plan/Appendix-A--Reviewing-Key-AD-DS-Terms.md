---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 附錄 A-審核金鑰 AD DS 詞彙
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4f8f9d0b89868ccdc795740ecadb00b979dbc066
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071180"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>附錄 A︰檢視重要的 AD DS 術語

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下列詞彙與 Windows Server 2008 Active Directory Domain Services (AD DS) 的部署程式有關。

## <a name="active-directory-domain"></a>Active Directory 網域
電腦網路中的管理單位，為了方便管理，會將數個功能分組，包括下列各項：

-   **整個網路的使用者身分識別** 。 在網域中，您可以建立一次使用者身分識別，然後在任何已加入網域的樹系的電腦上進行參考。 組成網域存放區使用者帳戶和使用者認證（例如密碼或憑證）安全的網域控制站。

-   **驗證** 。 網域控制站為使用者提供驗證服務。 它們也提供額外的授權資料，例如使用者群組成員資格。 系統管理員可以使用這些服務來控制對網路上資源的存取。

-   **信任關係** 。 網域會透過自動雙向信任，將驗證服務延伸至自己樹系中其他網域的使用者。 網域也會透過樹系信任或手動建立的外部信任，將驗證服務延伸至其他樹系中網域的使用者。

-   **原則管理** 。 網域是系統管理原則的範圍，例如密碼複雜性和密碼重複使用規則。

-   **複寫** 。 網域會定義目錄樹狀結構的磁碟分割，此磁碟分割會提供足夠的資料來提供所需的服務，並在網域控制站之間進行複寫。 如此一來，所有網域控制站都是網域中的對等，而且是以一個單位來管理。

## <a name="active-directory-forest"></a>Active Directory 樹系
一或多個 Active Directory 網域的集合，共用共同的邏輯結構、目錄架構和網路設定，以及自動、雙向、可轉移的信任關係。 每個樹系都是目錄的單一實例，而且它會定義安全性界限。

## <a name="active-directory-functional-level"></a>Active Directory 功能等級
AD DS 設定，可啟用 advanced 全網域或全樹系的 AD DS 功能。

## <a name="migration"></a>移轉
將物件從來源網域移至目標網域的程式，同時保留或修改物件的特性，以便在新的網域中進行存取。

## <a name="domain-restructure"></a>網域重建
牽涉到變更樹系網域結構的遷移程式。 網域重建可以牽涉到合併或加入網域，而且可以在樹系之間或樹系內進行。

## <a name="domain-consolidation"></a>網域合併
重建程式會將其內容與其他網域的內容合併，以消除 AD DS 的網域。

## <a name="domain-upgrade"></a>網域升級
將網域的目錄服務升級至較新版本之目錄服務的程式。 這包括升級所有網域控制站上的作業系統，並在適用的情況下提高 AD DS 功能等級。

## <a name="in-place-domain-upgrade"></a>就地網域升級
升級指定網域中所有網域控制站的作業系統（例如，將 Windows Server 2003 升級至 Windows Server 2008，以及提高網域的功能等級（如使用者和群組）的程式）的處理常式。

## <a name="forest-root-domain"></a>樹系根域
在 Active Directory 樹系中建立的第一個網域。 此網域會自動指定為樹系根域。 它提供 Active Directory 樹系基礎結構的基礎。

## <a name="regional-domain"></a>地區網域
在地理區域中建立以優化複寫流量的子域。



