---
description: 深入瞭解：瞭解 Active Directory 的邏輯模型
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: 了解 Active Directory 邏輯模型
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: cee56de51925da3d27313a4edcb3481980b8b077
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042226"
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 邏輯模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對 Active Directory Domain Services (AD DS) 設計邏輯結構，需要定義目錄中容器之間的關聯性。 這些關聯性可能是以系統管理需求（例如委派）為基礎，或可能由作業需求定義，例如控制複寫的需求。

在您設計 Active Directory 邏輯結構之前，請務必瞭解 Active Directory 的邏輯模型。 AD DS 是一種分散式資料庫，可儲存和管理網路資源的相關資訊，以及啟用目錄之應用程式的應用程式特定資料。 AD DS 可讓系統管理員將網路 (的元素（例如使用者、電腦和) 裝置）組織成階層式內含專案結構。 最上層容器是樹系。 樹系中的網域是網域，且網域內 (Ou) 的組織單位。 這稱為邏輯模型，因為它與部署的實體層面無關，例如每個網域和網路拓撲中所需的網域控制站數目。

## <a name="active-directory-forest"></a>Active Directory 樹系
樹系是一或多個 Active Directory 網域的集合，共用通用邏輯結構、目錄架構 (類別和屬性定義) 、目錄設定 (網站和複寫資訊) ，以及通用類別目錄 (全樹系搜尋功能) 。 相同樹系中的網域會自動連結至雙向、可轉移的信任關係。

## <a name="active-directory-domain"></a>Active Directory 網域
網域是 Active Directory 樹系中的磁碟分割。 分割資料可讓組織只將資料複寫到需要的位置。 如此一來，您可以透過網路，在頻寬有限的情況下擴充此目錄。 此外，網域也支援許多與管理相關的其他核心功能，包括：

-   整個網路的使用者身分識別。 網域可讓使用者身分識別建立一次，並在加入網域所在樹系的任何電腦上參考。 組成網域的網域控制站會用來儲存使用者帳戶和使用者認證 (例如) 安全的密碼或憑證。

-   驗證。 網域控制站為使用者提供驗證服務，並提供其他授權資料（例如使用者群組成員資格），可用來控制網路上資源的存取權。

-   信任關係。 網域可透過信任方式，將驗證服務延伸至其樹系以外網域中的使用者。

-   複寫。 網域會定義包含足夠資料的目錄分割，以提供網域服務，然後在網域控制站之間進行複寫。 如此一來，所有網域控制站都是網域中的對等，並以一個單位來管理。

## <a name="active-directory-organizational-units"></a>Active Directory 組織單位
Ou 可以用來構成網域內的容器階層。 Ou 可用來將物件分組以進行系統管理，例如群組原則或授權委派的應用程式。 您可以透過 ou 和 OU 中的物件上的存取控制清單 (Acl) ，來控制 OU 上的 (以及它) 內的物件。 為了協助管理大量物件，AD DS 支援授權委派的概念。 藉由委派，擁有者可以將對物件的完整或受限系統管理控制轉移給其他使用者或群組。 委派很重要，因為它有助於將大量物件的管理散發給受信任執行管理工作的許多人。



