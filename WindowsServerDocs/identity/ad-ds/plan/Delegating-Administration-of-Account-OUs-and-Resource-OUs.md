---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: "Account Ou 和資源 Ou 的管理委派"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Account Ou 和資源 Ou 的管理委派

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Account 組織單位 (Ou) 可能包含使用者、 群組與電腦物件。 資源 Ou 包含資源和帳號，是負責管理那些資源。 樹系擁有者負責建立組織單位結構管理這些物件和資源，以及控制該結構委派給組織單位擁有者。  
  
## <a name="delegating-administration-of-account-ous"></a>Account Ou 的管理委派  
如果需要建立及修改使用者、 群組與電腦物件，委派 account 組織單位結構資料系統管理員。 Account 組織單位結構是子樹 Ou 的每個 account 類型必須獨立控制。 例如，組織單位擁有者可以委派特定控制各種資料系統管理員子女 ou 服務帳號負責組織單位使用者、 電腦、 群組中。  
  
下圖顯示組織單位結構 account 的一個例子。  
  
![管理委派](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
下表列出，並告訴您，您可以建立組織單位結構 account 可能子女 Ou。  
  
|組織單位|用途|  
|------|-----------|  
|使用者|包含帳號非的人員。|  
|服務帳號|需要存取權的網路資源部分服務執行帳號。 這個組織單位是不同的服務帳號建立組織單位，使用者中所包含的使用者帳號。 此外，在不同的 Ou 將不同類型的帳號可讓您管理依據他們特定的系統需求。|  
|電腦|包含帳號網域控制站以外的電腦。|  
|群組|包含除了管理群組分開管理所有類型的群組。|  
|系統管理員|使用者和群組帳號包含資料系統管理員中 account 組織單位結構讓他們從一般的使用者分開進行管理。 讓稽核這個組織單位，讓您可以變更管理使用者和群組。|  
  
下圖顯示 account 組織單位結構管理群組設計的其中一個範例。  
  
![管理委派](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
管理子女 Ou 群組會授與完全控制只能透過種特定物件他們所負責管理。  
  
您用來控制組織單位結構中的委派群組的類型根據帳號位於何處和目的地的相對，管理組織單位結構。 系統管理員使用者帳號和組織單位結構所有存在於單一網域中，如果您使用委派建立群組必須全域群組。 如果您的組織管理自己帳號，並有一個以上的地理區域中的部門，您可能會資料系統管理員負責管理帳號 Ou 一個以上的網域中的群組。 如果所有的資料系統管理員的使用者存在單一網域中，您有多個您要委派控制項的網域中的結構組織單位，將這些系統帳號通用群組成員，並委派組織單位結構每個網域的全域群組中的控制。 如果您將委派組織單位結構控制資料的系統管理員帳號來自多個網域，您必須使用通用群組。 萬用群組可以包含使用者網域不同，因此，它們可以用來委派在多個網域控制。  
  
## <a name="delegating-administration-of-resource-ous"></a>資源 Ou 的管理委派  
資源 Ou 可用來管理資源的存取權。 資源組織單位擁有者建立電腦帳號加入網域包含資源，例如檔案共用、 資料庫和印表機的伺服器。 資源組織單位擁有者也會建立控制那些資源群組。  
  
下圖顯示資源組織單位兩個位置。  
  
![管理委派](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
資源組織單位可能位於網域根下，或為子女的組織單位管理階層對應 account 組織單位組織單位。 資源 Ou 不需要任何標準子女 Ou。 電腦及群組位於直接資源組織單位。  
  
資源組織單位擁有者組織單位中的物件的擁有，但不是擁有組織單位容器本身。 資源組織單位擁有者只能管理電腦及群組物件。無法建立組織單位中的物件其他種類，無法建立子女 Ou。  
  
> [!NOTE]  
> 建立者或物件的擁有者對無論繼承父容器的權限的物件設定存取控制清單 (ACL) 的功能。 如果資源組織單位擁有者可以重設 ACL 組織單位，該擁有者可以建立組織單位，包括使用者任何種物件。 基於這個原因，以建立 Ou 不允許的資源組織單位擁有者。  
  
建立組織單位網域中每個資源，代表資料管理員負責管理 content 組織單位的全域群組。 此群組擁有透過群組和電腦物件組織單位，但不是會透過組織單位容器本身完整控制權。  
  
下圖顯示資源組織單位管理群組設計。  
  
![管理委派](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
將電腦帳號放入資源組織單位讓組織單位 account 物件的擁有者控制，但不會電腦的系統管理員讓組織單位擁有者。 在 Active Directory domain，網域管理群組，預設會放在所有的電腦上的本機系統管理員群組。 也就是服務系統管理員可以控制那些電腦。 如果資源組織單位擁有者需要管理控制其 Ou 在的電腦，樹系擁有者套用限制群組群組原則中該組織單位的電腦上讓資源組織單位擁有者的系統管理員群組成員。  
  


