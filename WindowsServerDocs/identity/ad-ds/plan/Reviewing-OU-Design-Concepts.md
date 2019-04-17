---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: "審查組織單位設計概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>審查組織單位設計概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

單位（組織單位）結構網域包括下列項目：  
  
-   組織單位階層的簡圖  
  
-   一份 Ou  
  
-   針對每個組織單位：  
  
    -   組織單位的用途  
  
    -   清單中的使用者或群組中組織單位有控制組織單位或物件  
  
    -   使用者和群組可以透過在 [組織單位物件的控制項類型  
  
不需要的組織單位階層不會反映部門階層組織或群組。 Ou 建立某特定用途，例如的管理委派或限制的物件可見性的群組原則、應用程式。  
  
您可以設計組織單位結構管理委派個人或群組在組織中需要有自治以管理他們自己的資源和資料。 Ou 代表管理範圍，以及讓您控制資料系統管理員權限的範圍。  
  
例如，您可以建立組織單位稱為 ResourceOU 並使用它來儲存的檔案和由群組列印伺服器屬於所有電腦帳號。 然後，您可以設定安全性組織單位上，只有在群組中的資料系統管理員可以存取組織單位。 如此可防止資料其他群組中的系統管理員的檔案和列印伺服器帳號竄改。  
  
您可以建立特定用途，例如應用程式的群組原則，或限制的受保護的物件可見性特定的使用者可以看到他們的 Ou 子進一步改善您的組織單位結構。 例如如果您需要套用群組原則來選取使用者或資源群組，您可以新增的使用者或資源到組織單位，並再該組織單位適用於群組原則。 您也可以使用組織單位階層要進一步控制管理委派。  
  
在您的組織單位結構層級數目無技術限制時，性我們建議您限制您的組織單位結構深度不會超過 10 層級。 還有 Ou 在每個層級的數目無限制技術。 請注意該 Active Directory Domain Services (AD DS)-讓應用程式中可能會有數字字元分辨名稱（也就是在 directory 物件的完整輕量型 Directory 存取通訊協定 (LDAP) 路徑）中使用的限制或組織單位深度階層中。  
  
組織單位中的結構 AD DS 不是看到給使用者。 組織單位結構是服務管理員和資料的系統管理員，管理工具，很容易變更。 若要檢視和更新您的組織單位結構設計反映變更您的系統管理結構，並支援原則為主管理繼續。  
  


