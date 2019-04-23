---
title: 決定軟體限制原則的允許拒絕清單和應用程式清查
description: Windows Server 安全性
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847289"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>決定軟體限制原則的允許拒絕清單和應用程式清查

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題適用於 IT 專業人員，提供如何建立允許和拒絕清單，將受軟體限制原則 (SRP) 從 Windows Server 2008 和 Windows Vista 的應用程式的指引。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory 網域服務和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的起始點，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用於取代或搭配 SRP 應用程式控制策略的一部分。

如需如何完成使用 SRP 的特定工作的資訊，請參閱下列各項：

-   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

-   [使用軟體限制原則來協助保護電腦免於電子郵件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>若要選擇哪些預設規則：允許或拒絕
是您的預設規則的基礎的兩個模式之一，就可以部署軟體限制原則：允許清單或拒絕清單。 您可以建立原則，識別允許環境中執行每個應用程式您的原則中的預設規則是限制的網站，而且會封鎖您明確不允許執行的所有應用程式。 您可以建立識別，無法執行; 每個應用程式的原則預設規則沒有限制，並限制只有已明確列出的應用程式。

> [!IMPORTANT]
> 拒絕清單 模式下可能是您的組織對於應用程式控制的高維護策略。 建立和維護一個不斷演變的清單，其中禁止所有惡意程式碼和其他有問題的應用程式會是耗時且容易出錯。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>建立您的應用程式允許清單的詳細目錄
若要有效地使用允許預設規則，您需要判斷哪些應用程式只需要您組織中。 有一些工具來產生應用程式清查，例如 Microsoft Application Compatibility Toolkit 清查收集器所設計。 但 SRP 有進階的記錄功能可協助您了解完全哪些應用程式正在執行您的環境中。

##### <a name="to-discover-which-applications-to-allow"></a>若要探索哪些允許的應用程式

1.  在測試環境中，部署預設規則設定為不受限制的軟體限制原則，並移除任何額外的規則。 如果您沒有強制限制的任何應用程式啟用 SRP，SPR 將能夠監視功能的應用程式正在執行。

2.  建立下列登錄值以啟用進階的記錄功能，並將路徑設定為寫入記錄檔的位置。

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    字串值：*NameLogFile NameLogFile 路徑*

    因為 SRP 會評估所有的應用程式，在執行時，將項目會寫入記錄檔*NameLogFile*每次在執行應用程式。

3.  評估的記錄檔

    每個記錄項目狀態：

    -   軟體限制原則和程序識別碼 (PID) 呼叫的處理序的呼叫端

    -   要評估的目標

    -   執行該應用程式時，發現 SRP 規則

    -   SRP 規則識別碼。

    寫入至記錄檔的輸出範例：

**explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe 不受限制的 usingpath 規則是，Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** 所有應用程式和相關聯的程式碼，SRP 會檢查並設定為封鎖會記錄在記錄檔檔案，然後您可以使用以判斷哪一個可執行檔應該列入允許清單。


