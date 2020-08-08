---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: 了解 Active Directory 邏輯模型
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f05e3c4b370379b463abccc5ace09de908cfcea3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964965"
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 邏輯模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設計 Active Directory Domain Services (AD DS) 的邏輯結構，包括定義目錄中容器之間的關聯性。 這些關聯性可能是根據系統管理需求（例如委派的授權），或可能由作業需求定義，例如控制複寫的需求。

在設計 Active Directory 邏輯結構之前，請務必瞭解 Active Directory 邏輯模型。 AD DS 是一個分散式資料庫，可儲存和管理網路資源的相關資訊，以及啟用目錄之應用程式的應用程式特定資料。 AD DS 可讓系統管理員將網路 (的元素（例如使用者、電腦和) 裝置）組織成階層式內含專案結構。 最上層容器是樹系。 在樹系內是網域，而在網域內是組織單位 (Ou) 。 這稱為「邏輯模型」，因為它與部署的實體層面無關，例如每個網域和網路拓撲中所需的網域控制站數目。

## <a name="active-directory-forest"></a>Active Directory 樹系
樹系是一或多個 Active Directory 網域的集合，共用通用邏輯結構、目錄架構 (類別和屬性定義) 、目錄設定 (網站和複寫資訊) ，以及通用類別目錄 (全樹系搜尋功能) 。 相同樹系中的網域會自動與雙向、可轉移的信任關係進行連結。

## <a name="active-directory-domain"></a>Active Directory 網域
網域是 Active Directory 樹系中的資料分割。 分割資料可讓組織只將資料複寫到所需的位置。 如此一來，該目錄就可以在頻寬有限的網路上進行全域調整。 此外，網域也支援一些其他與系統管理相關的核心功能，包括：

-   全網路使用者身分識別。 網域可讓使用者身分識別建立一次，並在加入網域所在樹系的任何電腦上加以參考。 組成網域的網域控制站用來儲存使用者帳戶和使用者認證 (例如) 安全的密碼或憑證。

-   驗證。 網域控制站會為使用者提供驗證服務，並提供額外的授權資料（例如使用者群組成員資格），可用來控制對網路上資源的存取。

-   信任關係。 網域可以透過信任，將驗證服務延伸到其自有樹系以外的網域中的使用者。

-   複寫。 網域會定義目錄的分割區，其中包含足夠的資料來提供網域服務，然後在網域控制站之間進行複寫。 如此一來，所有網域控制站都是網域中的對等，並以一個單位來管理。

## <a name="active-directory-organizational-units"></a>Active Directory 的組織單位
Ou 可以用來形成網域內的容器階層。 Ou 是用來將物件分組以進行系統管理，例如應用群組原則或授權委派。 控制 OU 上的 (和 it 內的物件) 取決於 OU 上的存取控制清單 (Acl) 和 OU 中的物件。 為了協助管理大量物件，AD DS 支援授權委派的概念。 藉由委派，擁有者可以將物件的完整或有限系統管理控制轉移給其他使用者或群組。 委派是很重要的，因為它有助於將大量物件的管理散發給多個受信任執行管理工作的人。



