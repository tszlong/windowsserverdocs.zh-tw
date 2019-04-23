---
title: 使用者帳戶控制概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887669"
---
# <a name="user-account-control-overview"></a>使用者帳戶控制概觀
使用者帳戶控制\(UAC\)是 Microsoft 的整體安全性展望的基礎元件。  UAC 有助於減少惡意程式的影響。

## <a name="BKMK_OVER"></a>功能描述
UAC 可讓所有使用者使用標準使用者帳戶來登入他們的電腦。 使用標準使用者權杖啟動的程序可以使用授與標準使用者的存取權限來執行工作。 例如，「Windows 檔案總管」會自動繼承標準使用者層級權限。 此外，任何使用 Windows 檔案總管執行的程式\(例如，透過雙精度浮點數\-按一下 應用程式捷徑\)也使用一組標準的使用者權限來執行。 許多應用程式，包括隨附於作業系統本身，專為在這種方式中正常運作。

其他應用程式，尤其是特別設計並未考慮到，安全性設定通常會需要額外的權限，才能順利執行。 這些類型的程式稱為舊版應用程式。 此外，動作，例如安裝新軟體和變更程式，例如 Windows 防火牆的組態需要比標準使用者帳戶提供更多的權限。

當使用多個標準使用者權限來執行應用程式需求時，UAC 可以還原其他使用者群組的權杖。 這可讓使用者能夠明確控制要對使用者的電腦或裝置進行系統層級變更的程式。

## <a name="BKMK_APP"></a>實際的應用程式
在 UAC 中的系統管理員核准模式中，可協助防止惡意程式系統管理員的不知情的情況下以無訊息方式安裝。 它也有助於防止不小心系統\-寬的變更。 最後，它可以用來強制執行較高層級的規範，規定每個系統管理程序都必須由系統管理員主動同意或提供認證才能執行。



