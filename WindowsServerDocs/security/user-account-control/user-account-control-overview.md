---
title: 使用者帳戶控制概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bdc9f4dc4b8e19d62288f12a4f2b4e8c86b93b68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403315"
---
# <a name="user-account-control-overview"></a>使用者帳戶控制概觀
\(UAC\) 的使用者帳戶控制是 Microsoft 整體安全性願景的基礎元件。  UAC 有助於減少惡意程式的影響。

## <a name="BKMK_OVER"></a>功能描述
UAC 可讓所有使用者使用標準使用者帳戶來登入他們的電腦。 使用標準使用者權杖啟動的程序可以使用授與標準使用者的存取權限來執行工作。 例如，「Windows 檔案總管」會自動繼承標準使用者層級權限。 此外，使用 Windows Explorer 執行的任何程式 \(例如，\-按兩下應用程式快捷方式\) 也會使用一組標準的使用者權限執行。 許多應用程式（包括作業系統本身所含的）都是設計成以這種方式適當地運作。

其他應用程式（特別是未特別設計的安全性設定），通常需要額外的許可權才能順利執行。 這些類型的程式稱為繼承應用程式。 此外，像是安裝新的軟體，以及對 Windows 防火牆之類的程式進行設定變更等動作，需要的許可權比標準使用者帳戶可用的還多。

當應用程式需要以超過標準使用者權限執行時，UAC 可以將其他使用者群組還原至權杖。 這可讓使用者明確控制對其電腦或裝置進行系統層級變更的程式。

## <a name="BKMK_APP"></a>實際應用
UAC 中的管理核准模式有助於防止惡意程式在沒有系統管理員知識的情況下，以無訊息方式安裝。 它也有助於防止意外的系統\-的變更。 最後，它可以用來強制執行較高層級的規範，規定每個系統管理程序都必須由系統管理員主動同意或提供認證才能執行。



