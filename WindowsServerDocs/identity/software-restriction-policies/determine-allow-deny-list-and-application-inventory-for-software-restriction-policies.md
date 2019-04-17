---
title: "軟體限制原則判斷拒絕允許清單與應用程式清單"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>軟體限制原則判斷拒絕允許清單與應用程式清單

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題適用於 IT 專業人員，提供指引如何建立允許和拒絕受軟體限制原則 (SRP) 與 Windows Server 2008 和 Windows Vista 的開頭的應用程式] 清單。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是群組原則的功能辨識中加入網域的電腦上執行的軟體程式，以及控制執行這些程式的能力。 您可以使用軟體限制原則來建立高度限制的電腦，您可讓只專門辨識應用程式執行設定。 這些整合在一起 Microsoft Active Directory Domain Services 及群組原則，但是您也可以在獨立的電腦上設定。 針對 SRP 的起點，請查看[軟體限制原則](software-restriction-policies.md)。

開始使用 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用於而不是或 SRP 搭配您的應用程式控制項策略的一部分。

如需如何完成特定工作使用 SRP，請查看下列資訊：

-   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

-   [為了協助保護您的電腦免受電子郵件病毒使用軟體限制原則](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>選擇哪些預設的規則：允許或拒絕
其中一個預設規則的基準的兩種模式可部署軟體限制原則：允許清單或拒絕清單。 您可以建立辨識允許; 環境中執行每個應用程式原則您的原則中的預設規則會限制，並將會封鎖所有您明確不允許執行的應用程式。 您可以建立的原則，將無法執行; 每個應用程式辨識預設值是不受限制 \ 和您明確列出的應用程式可限制。

> [!IMPORTANT]
> 拒絕清單模式可能會為您的組織相關的應用程式控制項高維護策略。 建立及維護就是禁止所有的惡意程式碼和其他發生問題的應用程式的進化清單會很費時且容易使用。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>建立允許清單中的應用程式清單
若要允許預設規則有效的使用，您需要判斷完全哪些應用程式需要您在組織中。 有而設計的應用程式清單，例如清單中的行程 Microsoft 應用程式的相容性工具組工具。 但 SRP 有進階登入功能可協助您了解完全哪些應用程式中執行您的環境。

##### <a name="to-discover-which-applications-to-allow"></a>若要探索允許的應用程式

1.  在測試環境中，軟體限制原則部署預設規則設定限制以並移除任何其他規則。 如果您不必限制的任何應用程式讓 SRP，SPR 無法監視功能的應用程式正在執行。

2.  建立下列登錄值，以讓進階登入功能，並設定路徑寫入登入檔案的位置。

    **「HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers」**

    字串：*NameLogFile NameLogFile 路徑*

    因為 SRP 正在評估所有應用程式執行時，項目寫入登入檔案*NameLogFile*應用程式執行時，每一次。

3.  評估登入檔案

    顯示每個登入的項目：

    -   播報來電者的軟體限制原則與 ID () 的 PID 呼叫處理程序

    -   正在評估目標

    -   發生該應用程式執行時 SRP 規則

    -   識別字 SRP 規則。

    輸出寫入登入檔案的範例：

**explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe Guid 不受限制的 usingpath 規則 = {320bd852-aa7c-4674-82c5-9a80321670a3}**所有應用程式和 SRP 檢查並封鎖設定的相關驗證碼將 experience 登入檔案，然後您可以使用它來判斷的可執行檔視為的允許清單中。


