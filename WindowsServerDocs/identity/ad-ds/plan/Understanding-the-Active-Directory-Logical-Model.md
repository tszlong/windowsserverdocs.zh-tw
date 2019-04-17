---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: "了解 Active Directory 邏輯模型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 邏輯模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 設計您邏輯結構涉及定義您 directory 中容器之間的關聯。 這些關聯性可能根據委派權限，例如系統需求，或也可以透過操作需求，例如需要控制複寫定義。  
  
則設計 Active Directory 邏輯結構之前，請務必以了解 Active Directory 邏輯模型。 AD DS 是分散式的資料庫來儲存及管理網路資源，以及應用程式特定資料的相關資訊從 directory 功能的應用程式。 AD DS 成階層包含結構，讓組織項目（例如，使用者、電腦與裝置）的網路系統管理員。 最上層容器是樹。 森林中的網域，並網域中的組織單位 (Ou)。 因為它是不受影響的實體部署，例如網域控制站在每種網域和網路拓撲所需的層面稱為邏輯模型。  
  
## <a name="active-directory-forest"></a>Active Directory 森林  
樹系是一或多個 Active Directory 網域共用相同的邏輯結構的集合 directory 架構（課程和屬性定義）、directory 設定（網站與複寫資訊），與通用（樹系的搜尋功能）。 在相同的樹系的網域自動雙向、轉移信任關係的連結。  
  
## <a name="active-directory-domain"></a>Active Directory domain  
Active Directory 森林中的磁碟分割網域。 分割資料，讓組織複寫只需要的位置資料。 如此一來，directory 可以全球有限的頻寬，在網路上縮放。 此外，網域支援許多其他核心管理相關功能包括：  
  
-   全網路使用者的身分。 網域讓使用者建立一次並加入網域所在的樹系的任何電腦上所參照的身分。 網域構成網域控制站用來儲存確實帳號及使用者認證（例如密碼或憑證）。  
  
-   驗證。 網域控制站提供驗證使用者服務，並提供額外的授權資料，例如使用者群組成員資格，可以用來控制資源網路上的存取權。  
  
-   標示為信任的關聯。 網域可以透過信任擴充驗證服務自己的樹系外網域中的使用者。  
  
-   複寫。 網域定義 directory 包含不足提供網域服務的資料，再將它複製之間的網域控制站的磁碟分割。 如此一來，所有網域控制站的同儕網域中的，並為單位管理。  
  
## <a name="active-directory-organizational-units"></a>Active Directory 組織單位  
Ou 可用於形成的容器階層網域中。 Ou 用於群組物件給系統管理員使用群組原則的應用程式或授權委派例如。 控制項（透過組織單位，它中的物件）由存取控制清單 (Acl) 在 [組織單位和組織單位中的物件。 若要加速管理大量物件，AD DS 支援委派權限的概念。 透過委派，擁有者可以轉移到其他使用者或群組的完整或有限管理控制物件。 因為它有助於跨多個以執行管理工作受信任的人散發大量物件的管理委派務必。  
  


