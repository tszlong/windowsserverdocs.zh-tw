---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: 使用 OU 物件委派管理
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: da5fce57b184b8c8d67809b89311405fc5557938
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941088"
---
# <a name="delegating-administration-by-using-ou-objects"></a>使用 OU 物件委派管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用組織單位 (Ou) 將 OU 內的物件管理（例如使用者或電腦）委派給指定的個人或群組。 若要使用 OU 來委派系統管理，請將委派系統管理許可權的個人或群組放在群組中，將要控制的物件集合放入 OU 中，然後將 OU 的系統管理工作委派給該群組。

Active Directory Domain Services (AD DS) 可讓您控制可在非常詳細的層級委派的管理工作。 例如，您可以指派一個群組來擁有 OU 中所有物件的完整控制權;將許可權指派給另一個群組，以在 OU 中建立、刪除及管理使用者帳戶;然後，將第三個群組指派為許可權，以重設使用者帳戶密碼。 您可以將這些許可權設為可繼承的，使其套用至放置在原始 OU 之子樹中的任何 Ou。

預設 Ou 和容器是在安裝 AD DS 期間建立的，由服務系統管理員控制。 最好是服務系統管理員繼續控制這些容器。 如果您需要將控制權委派給目錄中的物件，請建立額外的 Ou，並將物件放在這些 Ou 中。 將這些 Ou 的控制權委派給適當的資料管理員。 這可讓您將控制權委派給目錄中的物件，而不需要變更提供給服務系統管理員的預設控制項。

樹系擁有者會決定委派給 OU 擁有者的授權層級。 這項功能的範圍從建立和操作 OU 內的物件，到只允許控制 OU 內單一物件類型的單一屬性。 授與使用者在 OU 中建立物件的能力，會以隱含方式授與該使用者操作使用者所建立之任何物件之任何屬性的能力。 此外，如果建立的物件是容器，則使用者可以隱含地建立和操作放置在容器中的任何物件。

## <a name="in-this-section"></a>本節內容

-   [委派預設容器與 OU 的管理](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)

-   [委派帳戶 OU 與資源 OU 的管理](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)



