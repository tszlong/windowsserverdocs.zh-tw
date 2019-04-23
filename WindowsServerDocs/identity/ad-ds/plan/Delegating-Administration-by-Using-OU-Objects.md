---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: 使用 OU 物件委派管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886939"
---
# <a name="delegating-administration-by-using-ou-objects"></a>使用 OU 物件委派管理

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用委派物件，例如使用者或電腦的系統管理的組織單位 (Ou) 內指定的個人或群組的 OU。 若要使用 OU 來委派管理，放置的個人或群組，您委派系統管理權限的群組放置到 OU，控制物件的集合，然後將系統管理工作委派給該群組的 ou。  
  
Active Directory 網域服務 (AD DS) 可讓您控制可以在非常詳細的層級委派的系統管理工作。 比方說，您可以將 OU; 中擁有的所有物件的完整控制權的一個群組的指派指派另一個群組的權限，才能建立、 刪除和管理 OU; 中的使用者帳戶然後指派第三個群組的權限只重設使用者帳戶密碼。 您可以進行這些權限繼承，使其套用至任何位於原始的 OU 的樹狀子目錄中的 Ou。  
  
預設的 Ou 和容器在 AD DS 安裝期間建立，且受服務系統管理員。 如果服務系統管理員繼續控制這些容器，它是最佳。 如果您要委派控制物件在目錄中，建立其他 Ou，並將物件放在這些 Ou。 這些 Ou 的控制權，委派給適當的資料管理員。 這讓您能夠委派控制目錄中的物件，而不需要變更提供給服務系統管理員的預設控制項。  
  
樹系擁有者決定委派給 OU 擁有者的權限等級。 這可以介於能夠建立和操作來控制單一 OU 中的物件類型的單一屬性只允許 OU 中的物件。 授與使用者能夠以隱含方式建立的 OU 中的物件授與該使用者的能力，來操作任何屬性的任何使用者建立的物件。 此外，所建立的物件是容器，如果使用者以隱含方式能夠建立及操作會放在容器中的任何物件。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [委派預設容器和 Ou 的系統的管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [委派系統管理帳戶 Ou 與資源 Ou](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


