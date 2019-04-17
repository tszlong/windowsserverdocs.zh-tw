---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "使用組織單位物件的管理委派"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>使用組織單位物件的管理委派

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用組織單位 (Ou) 物件，例如使用者或電腦的系統管理委派組織單位指定的個人或群組中。 若要委派管理，使用組織單位，將的個人或群組的您委派系統管理員權限的群組、 放入組織單位，控制物件，然後管理工作委派的組織單位加入該群組。  
  
Active Directory Domain Services (AD DS) 可讓您控制可在非常詳細層級委派管理工作。 例如，您可以指定; 中有完全控制所有物件群組僅限來建立、 delete，及管理使用者帳號組織單位; 中的權限指派另一個群組然後指派第三個群組右側來重設使用者 account 密碼。 您可以讓這些權限繼承權，使它們套用至任何 Ou 的放置在子的原始組織單位。  
  
預設 Ou 容器 AD DS 在安裝期間建立及服務的系統管理員所控制。 如果您仍控制這些容器服務系統管理員，是最好的作法。 如果您需要委派中 directory 物件的控制，請建立其他 Ou 和置於這些 Ou 物件。 控制這些 Ou 委派給系統管理員適當的資料。 這樣可以委派中 directory 物件的控制，而不變更預設的控制項給定服務的系統管理員。  
  
樹系擁有者判斷委派給組織單位擁有者授權層級。 這的範圍可從 [建立及管理只允許控制單一類型單一中組織單位物件的屬性組織單位中的物件的能力。 授與使用者的能力隱含建立組織單位物件授與使用者的能力來管理的任何物件，使用者會建立任何屬性。 此外，如果容器所建立的物件，使用者隱含有建立及管理任何物件的容器位於的能力。  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [預設容器和 Ou 的管理委派](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Account Ou 和資源 Ou 的管理委派](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


