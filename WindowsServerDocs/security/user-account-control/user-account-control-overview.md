---
title: 使用者帳戶控制概觀
description: 瞭解使用者帳戶控制，這是 Microsoft 整體安全性願景的基本元件，以及它如何協助降低惡意軟體的影響。
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 7cb2e6bb17ab92dc2ff23ace2de2654f6a2b2197
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177277"
---
# <a name="user-account-control-overview"></a>使用者帳戶控制概觀
「使用者帳戶控制 \( UAC」 \) 是 Microsoft 整體安全性願景的基本元件。  UAC 有助於減少惡意程式的影響。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
UAC 可讓所有使用者使用標準使用者帳戶來登入他們的電腦。 使用標準使用者權杖啟動的程序可以使用授與標準使用者的存取權限來執行工作。 例如，「Windows 檔案總管」會自動繼承標準使用者層級權限。 此外，只要按兩下應用程式快捷方式，即可使用 Windows 檔案總管執行的任何程式 \( ， \- 也會 \) 以一組標準的使用者權限執行。 許多應用程式（包括隨附于作業系統本身的應用程式）都是以這種方式適當地運作。

其他應用程式（特別是未特別針對安全性設定而設計的應用程式）通常需要額外的許可權才能順利執行。 這些類型的程式稱為繼承應用程式。 此外，像是安裝新軟體，以及對 Windows 防火牆等程式進行設定變更的動作，都需要比標準使用者帳戶更多的許可權。

當應用程式必須以超過標準的使用者權限執行時，UAC 可以將其他使用者群組還原至權杖。 這可讓使用者明確控制對其電腦或裝置進行系統層級變更的程式。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
UAC 中的管理員核准模式有助於防止惡意程式以無訊息模式安裝，而不需要系統管理員的知識。 它也有助於防止意外的 \- 全系統變更。 最後，它可以用來強制執行較高層級的規範，規定每個系統管理程序都必須由系統管理員主動同意或提供認證才能執行。



