---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: 使用 OU 物件委派管理
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4a097f42a0de7a28d93637e0c5df39e04fe50532
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947697"
---
# <a name="delegating-administration-by-using-ou-objects"></a>使用 OU 物件委派管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用組織單位 (Ou) ，將 OU 內的物件管理（例如使用者或電腦）委派給指定的個人或群組。 若要使用 OU 來委派管理，請將您要委派系統管理許可權的個人或群組放入群組中、將要控制的一組物件放入 OU 中，然後將 OU 的系統管理工作委派給該群組。

Active Directory Domain Services (AD DS) 可讓您控制可以在非常詳細的層級委派的系統管理工作。 例如，您可以指派一個群組來擁有 OU 中所有物件的完整控制權;將許可權指派給另一個群組，以建立、刪除及管理 OU 中的使用者帳戶。然後指派第三個群組，只是用來重設使用者帳戶密碼的許可權。 您可以將這些許可權設為可繼承，使其適用于放在原始 OU 之子樹中的任何 Ou。

預設 Ou 和容器會在安裝 AD DS 期間建立，並由服務系統管理員控制。 如果服務系統管理員繼續控制這些容器，這就是最好的做法。 如果您需要委派目錄中物件的控制權，請建立額外的 Ou，並將物件放在這些 Ou 中。 將這些 Ou 的控制權委派給適當的資料管理員。 如此一來，您就可以委派控制目錄中的物件，而不需要變更服務系統管理員所指定的預設控制項。

樹系擁有者會決定委派給 OU 擁有者的授權層級。 其範圍從建立和操作 OU 內的物件，到僅允許控制 OU 中單一物件類型的單一屬性。 授與使用者在 OU 中建立物件的能力，會隱含地授與該使用者操作使用者所建立的任何物件之任何屬性的能力。 此外，如果建立的物件是容器，則使用者可以隱含地建立和操作容器中所放置的任何物件。

## <a name="in-this-section"></a>本節內容

-   [委派預設容器與 OU 的管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)

-   [委派帳戶 OU 與資源 OU 的管理](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)



