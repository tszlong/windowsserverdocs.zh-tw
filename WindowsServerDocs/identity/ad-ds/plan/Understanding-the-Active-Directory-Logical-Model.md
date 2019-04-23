---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: 了解 Active Directory 邏輯模型
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834269"
---
# <a name="understanding-the-active-directory-logical-model"></a>了解 Active Directory 邏輯模型

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 網域服務 (AD DS) 設計您的邏輯結構，需要定義您的目錄中的容器之間的關聯性。 這些關聯性可能會根據系統管理的需求，例如 委派權限，或可能由操作的需求，例如控制複寫的需求。  
  
在設計您的 Active Directory 邏輯結構之前，務必了解 Active Directory 邏輯模型。 AD DS 是一個分散式的資料庫，儲存和管理目錄的應用程式的網路資源，以及應用程式專屬資料的相關資訊。 AD DS 可讓系統管理員，來組織 （例如使用者、 電腦及裝置） 將網路項目，為階層式內含項目結構。 最上層容器是樹系。 樹系內網域，而網域內組織單位 (Ou)。 這被稱為邏輯模型，因為它是獨立的部署，例如每個網域和網路拓撲內所需的網域控制站數目的實體層面。  
  
## <a name="active-directory-forest"></a>Active Directory 樹系  
樹系是共用通用的邏輯結構的一或多個 Active Directory 網域的集合目錄結構描述 （類別和屬性定義）、 目錄組態 （站台與複寫資訊） 和通用類別目錄 （全樹系搜尋功能）。 相同的樹系中網域具有雙向、 可轉移的信任關係，會自動連結。  
  
## <a name="active-directory-domain"></a>Active Directory 網域  
網域是 Active Directory 樹系中的分割區。 分割資料，可讓組織僅以需要的地方複寫資料。 如此一來，目錄可以全域擴充可用的頻寬有限的網路。 此外，網域會支援其他核心數函式與系統管理，包括：  
  
-   整個網路的使用者身分識別。 網域可以讓使用者身分，來建立一次並參考已加入網域所在樹系的任何電腦上。 建立網域的網域控制站用來安全地儲存使用者帳戶和使用者認證 （例如密碼或憑證）。  
  
-   驗證。 網域控制站提供驗證服務的使用者，並提供額外的授權資料，例如使用者群組成員資格，可用來控制網路上資源的存取權。  
  
-   信任關係。 網域可以擴充到他們自己的樹系之外網域中的使用者驗證服務，透過信任。  
  
-   複寫。 網域會定義包含充足的資料來提供網域服務，並再將它複製網域控制站之間的目錄磁碟分割。 如此一來，所有網域控制站都是在網域中的對等，並做為一個單位管理。  
  
## <a name="active-directory-organizational-units"></a>Active Directory 組織單位  
Ou 可以用於網域內形成的容器階層架構。 基於管理目的，例如群組原則的應用程式或委派權限，Ou 用來群組物件。 （對 OU 和其中的物件） 控制由存取控制清單 (Acl) OU 及 OU 中的物件。 為了方便管理大量的物件，AD DS 支援委派權限的概念。 透過委派，擁有者可以傳輸到其他使用者或群組的完整或有限的系統管理控制權的物件。 委派是人員的很重要，因為它有助於將大量物件的管理跨多個受信任可以執行管理工作。  
  


