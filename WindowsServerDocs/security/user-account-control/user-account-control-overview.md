---
title: "使用者 Account 控制概觀"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>使用者 Account 控制概觀
使用者 Account 控制 \(UAC\) 是 Microsoft 的整體安全性辨識基本元件。  UAC 可以協助您減少的惡意程式影響。

## <a name="BKMK_OVER"></a>描述的功能
UAC 可以讓所有的使用者來登入他們的電腦使用標準帳號。 使用標準使用者權杖啟動處理程序可能會使用執行工作存取權限授與一般的使用者。 例如，Windows 檔案總管] 就會自動繼承標準使用者層級權限。 此外，使用 Windows 檔案總管會執行任何程式 \ (例如，即可 double\ 應用程式 shortcut\) 也執行一般設定使用者權限。 許多應用程式，包括隨附本身作業系統設計可正常運作，這種方式。

其他應用程式，尤其不是專門的安全性設定改變心意，通常需要額外的權限順利執行。 這類程式被指傳統應用程式。 此外，動作，例如安裝新的軟體和設定變更程式，例如 Windows 防火牆，需要更多比標準使用者過去可用的權限。

當使用多個標準使用者權限來執行應用程式需求時，UAC 可以權杖還原其他使用者群組。 這可讓使用者有明確控制其電腦或裝置的系統層級變更進行的程式。

## <a name="BKMK_APP"></a>實用的應用程式
在 UAC 的系統管理員核准模式可以協助防止惡意程式無訊息方式執行安裝，而不需要系統管理員身分知識。 這也有助於保護的非故意 system\ 層級的變更。 最後，它可以用來執行較高的相容性，系統管理員必須積極同意或每個系統處理程序提供的認證。



