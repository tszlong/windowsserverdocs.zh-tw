---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: "Active Directory Domain Services 概觀"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b502017315d2b8b6b3d6ddfdad40c6d0f913e6c3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="active-directory-domain-services-overview"></a>Active Directory Domain Services 概觀

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


Directory 是階層結構儲存在網路上的物件的相關資訊。 Directory 服務，例如 Active Directory Domain Services (AD DS) 提供用於儲存 directory 資料，並讓網路使用者和系統管理員可以使用此資料的方法。 例如，AD DS 儲存帳號，例如名稱、密碼、電話號碼，等等的相關資訊，並讓其他授權的使用者在相同網路存取此資訊。

Active Directory 物件的資訊儲存在網路上，並讓輕鬆，系統管理員的使用者來尋找並使用此資訊。 Active Directory 使用 directory 資訊的邏輯、階層組織為基礎結構化的資料儲存區。

此資料存放區，也就是 directory 包含 Active Directory 物件的相關資訊。 這些物件通常會包含例如伺服器、磁碟、印表機和網路使用者和電腦帳號共用的資源。 如需有關 Active Directory 資料存放區的詳細資訊，請查看[Directory 資料存放區](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx)。

登入驗證及存取控制物件 directory 透過整合 Active Directory 安全性。 單一網路登入的系統管理員可以管理 directory 資料與組織整個網路，並授權的網路使用者可以存取網路上的任何位置點一下資源。 原則管理易於管理即使是最複雜的網路。 如需有關 Active Directory 安全性的詳細資訊，請安全性概觀。

Active Directory 也包含：
* 一組規則的**架構**、類物件定義和中所包含的屬性 directory 的限制和限制這些物件的執行個體與其名稱的格式。 如需有關架構，查看結構描述。


* A**通用**包含每個中 directory 物件的相關資訊。 這可讓使用者和系統管理員，尋找 directory 資訊無論 directory 中的網域確實包含的資料。 如需通用，查看通用的角色。


* A**查詢和索引機制**，以讓物件和他們屬性可以發行和網路使用者的應用程式中找到。 如需有關查詢 directory，查看尋找 directory 資訊。


* A**複寫服務**，將 directory 資料分散網路。 網域中的所有網域控制站參與複寫和包含完整的所有它們 domain directory 資訊副本。 複製所有網域中的網域控制站 directory 資料的任何變更。 有關更多複寫 Active Directory，查看複寫概觀。

## <a name="understanding-active-directory"></a>了解 Active Directory
 本章節提供核心 Active Directory 概念連結：
 
* [Active Directory 結構與技術存放裝置](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [網域控制站的角色](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* Active Directory 架構 
* [了解信任](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx) 
* [Active Directory 複寫技術](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 搜尋及發行技術](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* 相互操作 DNS 與群組原則 
* [了解架構](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Active Directory 概念的詳細清單，請查看[了解 Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx)。 


