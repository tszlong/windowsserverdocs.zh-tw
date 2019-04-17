---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: "附錄 A-審查金鑰 AD DS 條款"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>答附錄審查金鑰 AD DS 條款

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下列條款的相關部署程序適用於 Windows Server 2008 Active Directory Domain Services (AD DS)。  
  
## <a name="active-directory-domain"></a>Active Directory domain  
電腦網路中管理方便群組部分功能，包括下列管理單元：  
  
-   **全網路使用者的身分**。 網域中的使用者身分建立一次，然後參考已經加入網域所在的樹系的任何電腦上。 網域構成網域控制站安全地儲存帳號及使用者的認證，例如密碼或憑證。  
  
-   **驗證**。 網域控制站提供使用者驗證服務。 它們也會提供額外的授權資料，例如使用者群組成員資格。 系統管理員可以使用這些服務來控制資源網路上的存取權。  
  
-   **信任關係**。 網域中自己的樹系其他網域中的使用者延伸驗證服務，透過自動雙向信任。 網域也會延伸到其他的樹系網域中的使用者驗證服務透過信任的樹系或手動建立的外部信任。  
  
-   **原則管理**。 網域是範圍管理原則，例如複雜密碼及密碼重複使用規則。  
  
-   **複寫**。 網域定義提供的資料，這是適合用來提供所需的服務和的網域控制站之間複製樹狀的磁碟分割。 如此一來，所有的網域控制站同儕在網域中，而且為單位管理它們。  
  
## <a name="active-directory-forest"></a>Active Directory 森林  
一或多個 Active Directory 網域分享常見的邏輯結構、 directory 架構，和網路設定，以及自動、 雙向轉移信任關係的集合。 每個樹系單一 directory 的而且它定義安全性邊界。  
  
## <a name="active-directory-functional-level"></a>Active Directory 功能層級  
AD DS，設定，可讓進階的網域全或樹系 AD DS 功能。  
  
## <a name="migration"></a>移轉  
從來源網域移動物件目標網域，同時保留或修改特性，讓您在新的網域存取物件的程序。  
  
## <a name="domain-restructure"></a>網域重建  
變更網域結構的樹系的移轉程序。 網域重建可能彙總或加入網域，而且它可以進行或之間樹森林中。  
  
## <a name="domain-consolidation"></a>網域彙總  
這牽涉到 AD DS 網域排除合併他們內容與其他網域到重整程序。  
  
## <a name="domain-upgrade"></a>網域升級  
升級 directory 服務網域中的較新版 directory 服務的程序。 這包括升級作業系統上所有的網域控制站和適用提高 AD DS 功能層級。  
  
## <a name="in-place-domain-upgrade"></a>就地網域升級  
升級作業系統的所有網域控制站在特定網域中，例如、 升級 Windows Server 2008、 Windows Server 2003 及提高時就地離開網域物件，使用者和群組]，例如功能層級的網域，如果有的話程序。  
  
## <a name="forest-root-domain"></a>森林根網域  
Active Directory 森林中建立第一個的網域。 這個網域自動指定為森林根網域。 Active Directory 森林基礎結構提供基本知識。  
  
## <a name="regional-domain"></a>地區的網域  
建立最佳化複寫流量的地理區域中的子女網域。  
  


