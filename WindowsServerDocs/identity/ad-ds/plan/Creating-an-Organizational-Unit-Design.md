---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: "建立組織單位設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>建立組織單位設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

樹系擁有負責建立組織單位（組織單位）設計的網域。 建立組織單位設計包含設計組織單位結構，指派組織單位擁有者的角色，以及建立 account 和 Ou 資源。  
  
一開始設計，可讓的管理委派組織單位結構。 組織單位設計完成時，您可以建立其他組織單位結構應用程式的群組原則來的使用者與電腦及限制可見性的物件。 如需詳細資訊，請查看 [設計基礎結構群組原則 ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655))。  
  
## <a name="ou-owner-role"></a>組織單位擁有者的角色  
樹系擁有者會指定每個組織單位，您的網域設計組織單位擁有者。 組織單位擁有者的資料管理員負責控制子樹 Active Directory Domain Services (AD DS) 中的物件。 如何管理委派，並且如何原則對其組織單位中的物件，就可以控制組織單位擁有者。 它們也可以建立新子樹和委派 Ou 子這些樹中的管理。  
  
不擁有組織單位擁有者或控制 directory 服務的作業，因為您可以分開擁有權和管理 directory 服務的擁有權和物件、管理數量具有高層級的存取權限的服務系統管理員。  
  
Ou 提供管理自己的系統和控制在 directory 物件的可見性的方法。 Ou 提供隔離的其他資料的系統管理員，但它們並不提供隔離的服務的系統管理員。 雖然組織單位擁有者可以控制物件子樹，樹系擁有者會保留所有子完整控制權。 如此一來更正錯誤，例如存取控制 (ACL，) 清單中的錯誤和取回委派的子資料系統管理員結束時的樹系擁有者。  
  
## <a name="account-ous-and-resource-ous"></a>Account Ou 和資源 Ou  
Account Ou 可能包含使用者、群組與電腦物件。 樹系擁有必須建立組織單位結構管理這些物件，然後將控制結構委派給該組織單位擁有者。 如果您要部署新的網域 AD DS，建立網域 account 組織單位，讓您可以委派帳號網域中的控制。  
  
資源 Ou 包含資源和帳號，是負責管理那些資源。 樹系擁有者也是負責建立組織單位結構管理這些資源，以及控制該結構委派給組織單位擁有者。 建立資源 Ou 視需要為每個群組中組織的資料與設備自主性管理的需求。  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>針對每個網域組織單位設計文件  
組合設計用來控制樹系的資源委派組織單位結構團隊。 樹系擁有者可能會參與設計程序，而且必須核准組織單位設計。 您也可能需要至少服務系統管理員，以確認設計無效。 其他設計團隊參與者可能會包含之資料系統管理員負責管理他們的擁有者 Ou 和組織單位會運作。  
  
請務必文件您組織單位設計。 想要建立 Ou 的清單。 與每個組織單位的文件類型組織單位，組織單位擁有者、家長組織單位（如果有的話），以及該組織單位來源。  
  
試算表列出您的組織單位設計協助您，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [審查組織單位設計概念](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [使用組織單位物件的管理委派](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


