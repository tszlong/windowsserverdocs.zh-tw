---
title: Active Directory Domain Services 總覽
description: 瞭解 Active Directory Domain Services 如何提供儲存目錄資料的方法，並讓網路使用者和系統管理員可以使用此資料。
ms.topic: article
ms.assetid: 6cfe9479-5d17-41d5-939a-891e5233fdca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 7bf44fc77dfa89c3663b74c5fd1a7468e9ffa82c
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112974"
---
# <a name="overview-of-active-directory-domain-services"></a>Active Directory Domain Services 總覽

>適用於：Windows Server (半年度管道)、Windows Server 2016

目錄是一種階層式結構，可將物件的相關資訊儲存在網路上。 目錄服務（例如 Active Directory Domain Services (AD DS) ）提供儲存目錄資料的方法，並讓網路使用者和系統管理員可以使用此資料。 例如，AD DS 會儲存使用者帳戶的相關資訊，例如名稱、密碼、電話號碼等，並讓相同網路上的其他授權使用者存取此資訊。

Active Directory 會將物件的相關資訊儲存在網路上，並讓系統管理員和使用者輕鬆地尋找和使用此資訊。 Active Directory 使用結構化資料存放區作為目錄資訊的邏輯階層式組織基礎。

此資料存放區（也稱為目錄）包含 Active Directory 物件的相關資訊。 這些物件通常包含共用資源，例如伺服器、磁片區、印表機，以及網路使用者和電腦帳戶。 如需 Active Directory 資料存放區的詳細資訊，請參閱 [目錄資料存放區](/previous-versions/windows/it-pro/windows-server-2003/cc736627(v=ws.10))。

安全性透過登入驗證和存取控制，與目錄中的物件整合 Active Directory。 透過單一網路登入，系統管理員可以管理整個網路中的目錄資料和組織，而授權的網路使用者可以存取網路上任何位置的資源。 原則式的系統管理甚至可以減輕最複雜的網路管理工作。 如需 Active Directory 安全性的詳細資訊，請參閱安全性總覽。

Active Directory 也包含：
* 一組規則（ **架構**），定義目錄中包含的物件和屬性的類別、這些物件之實例的條件約束和限制，以及其名稱的格式。 如需架構的詳細資訊，請參閱架構。


* **通用類別目錄**，包含目錄中每個物件的相關資訊。 這可讓使用者和系統管理員尋找目錄資訊，而不論目錄中的哪個網域實際包含資料。 如需通用類別目錄的詳細資訊，請參閱通用類別目錄的角色。


* **查詢和索引機制**，可讓網路使用者或應用程式發佈及尋找物件和其屬性。 如需有關查詢目錄的詳細資訊，請參閱尋找目錄資訊。


* 將目錄資料分散到網路的複寫 **服務** 。 網域中的所有網域控制站都參與複寫，並且包含其網域的所有目錄資訊的完整複本。 目錄資料的任何變更都會複寫到網域中的所有網域控制站。 如需 Active Directory 複寫的詳細資訊，請參閱複寫總覽。

## <a name="understanding-active-directory"></a>瞭解 Active Directory
 本節提供核心 Active Directory 概念的連結：

* [Active Directory 結構和儲存技術](/previous-versions/windows/it-pro/windows-server-2003/cc759186(v=ws.10))
* [網域控制站角色](/previous-versions/windows/it-pro/windows-server-2003/cc786438(v=ws.10))
* Active Directory 架構
* [瞭解信任](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771294(v=ws.10))
* [Active Directory 複寫技術](/previous-versions/windows/it-pro/windows-server-2003/cc786438(v=ws.10))
* [Active Directory 搜尋和發行技術](/previous-versions/windows/it-pro/windows-server-2003/cc775686(v=ws.10))
* 與 DNS 和群組原則交互操作
* [瞭解架構](/previous-versions/windows/it-pro/windows-server-2003/cc759402(v=ws.10))

如需 Active Directory 概念的詳細清單，請參閱 [瞭解 Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc781408(v=ws.10))。