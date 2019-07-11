---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Active Directory Domain Services 概觀
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed8a22881cd20633e6fcd61b146f3b0aad7a757b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792284"
---
# <a name="active-directory-domain-services-overview"></a>Active Directory Domain Services 概觀

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


目錄是階層式結構儲存在網路上的物件的相關資訊。 目錄服務，例如 Active Directory 網域服務 (AD DS)，提供方法來儲存目錄資料，並且讓這項資料可供網路使用者和系統管理員。 例如，AD DS 會儲存使用者帳戶，例如名稱、 密碼、 電話號碼等等的相關資訊，並讓其他授權的使用者在相同的網路存取這項資訊。

Active Directory 會將物件的相關資訊儲存在網路上，並將這項資訊更容易尋找和使用的系統管理員和使用者。 Active Directory 會使用結構化的資料存放區為基礎的邏輯階層式組織的目錄資訊。

此資料存放區，也就是目錄，包含 Active Directory 物件的相關資訊。 這些物件通常會包括共用的資源，例如伺服器、 磁碟區、 印表機和網路的使用者和電腦帳戶。 如需有關 Active Directory 資料存放區的詳細資訊，請參閱 < [Directory 資料存放區](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx)。

安全性透過登入驗證與目錄中的物件的存取控制與 Active Directory 整合。 使用單一網路登入，系統管理員可以管理目錄資料和他們的網路，整個組織，並已獲授權的網路使用者可以存取網路上的任何位置的資源。 原則式的系統管理甚至可以減輕最複雜的網路管理工作。 如需有關 Active Directory 安全性的詳細資訊，請參閱[安全性概觀](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

Active Directory 也包括：
* 一組規則，**結構描述**、 定義的物件類別和屬性中所包含的目錄、 條件約束以及這些物件的執行個體和其名稱的格式限制。 如需有關結構描述的詳細資訊，請參閱結構描述。


* A**通用類別目錄**，其中包含目錄中的每個物件的相關資訊。 這可讓使用者和系統管理員來尋找目錄資訊不論其所在的網域目錄中實際包含的資料。 如需有關通用類別目錄的詳細資訊，請參閱通用類別目錄角色。


* A**查詢與索引機制**，好讓物件和其屬性可以發行並找到的網路使用者或應用程式。 如需有關如何查詢目錄的詳細資訊，請參閱尋找目錄資訊。


* A**複寫服務**，在網路上散發目錄資料。 在網域中的所有網域控制站會參與複寫，且包含其網域的所有目錄資訊的完整複本。 目錄資料的任何變更都會複寫到網域中的所有網域控制站。 如需有關 Active Directory 複寫的詳細資訊，請參閱複寫概觀。

## <a name="understanding-active-directory"></a>了解 Active Directory
 此章節提供連結至 Active Directory 的核心概念：
 
* [Active Directory 結構和儲存體技術](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [網域控制站角色](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 架構](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771796(v=ws.10))
* [瞭解信任](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771568(v=ws.10)) 
* [Active Directory 複寫技術](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 搜尋與發行集的技術](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* [群組原則與 DNS 交互操作](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197486(v=ws.10))
* [了解結構描述](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

如需 Active Directory 概念的詳細清單，請參閱[了解 Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx)。 


