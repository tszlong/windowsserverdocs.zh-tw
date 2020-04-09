---
title: 決定軟體限制原則的允許拒絕清單和應用程式清查
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7609ebb0fdcb6d429cd40d99399eaaedb732df08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855091"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>決定軟體限制原則的允許拒絕清單和應用程式清查

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題適用于 IT 專業人員，提供如何建立應用程式的允許和拒絕清單，以供從 Windows Server 2008 和 Windows Vista 開始的軟體限制原則（SRP）管理。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的起點，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用來取代或與 SRP 搭配使用，以進行部分的應用程式控制策略。

如需如何使用 SRP 完成特定工作的詳細資訊，請參閱下列各項：

-   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

-   [使用軟體限制原則來協助保護您的電腦免于電子郵件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>要選擇的預設規則： [允許] 或 [拒絕]
軟體限制原則可以部署為預設規則基礎的兩種模式之一： [允許清單] 或 [拒絕清單]。 您可以建立一個原則來識別允許在環境中執行的每個應用程式;原則內的預設規則是受限制的，而且會封鎖您未明確允許執行的所有應用程式。 或者，您可以建立一個原則來識別每個無法執行的應用程式;預設規則是 [不受限制]，而且只會限制您已明確列出的應用程式。

> [!IMPORTANT]
> [拒絕清單] 模式可能是您的組織關於應用程式控制的高維護策略。 建立和維護妨礙所有惡意程式碼和其他有問題之應用程式的不斷進化清單會很耗時，而且容易發生錯誤。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>為允許清單建立應用程式的清查
若要有效地使用允許預設規則，您需要判斷組織中所需的應用程式是否正確。 有一些工具的設計是用來產生應用程式清查，例如 Microsoft 應用程式相容性工具組中的清查收集器。 但是 SRP 具有先進的記錄功能，可協助您瞭解您的環境中正在執行的應用程式。

##### <a name="to-discover-which-applications-to-allow"></a>探索允許的應用程式

1.  在測試環境中，將預設規則設為 [不受限制] 的軟體限制原則部署，並移除任何其他規則。 如果您啟用 SRP 而不強制它限制任何應用程式，則 SPR 將能夠監視正在執行哪些應用程式。

2.  建立下列登錄值，以啟用 advanced 記錄功能，並將路徑設定為應該寫入記錄檔的位置。

    **"HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    字串值： *NameLogFile 至 NameLogFile 的路徑*

    因為 SRP 會在執行時評估所有的應用程式，所以每次執行應用程式時，都會將專案寫入記錄檔*NameLogFile* 。

3.  評估記錄檔

    每個記錄專案的狀態如下：

    -   軟體限制原則的呼叫端，以及呼叫進程的處理序識別碼（PID）

    -   要評估的目標

    -   應用程式執行時所遇到的 SRP 規則

    -   SRP 規則的識別碼。

    寫入記錄檔的輸出範例：

**explorer .exe （PID = 4728） identifiedC： \ Windows\system32\onenote.exe as 不受限制的 usingpath 規則，Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}**   SRP 檢查和設定為 [封鎖] 的所有應用程式和相關程式碼都會記錄在記錄檔中，接著您可以使用該檔案來判斷哪些可執行檔應視為允許清單。


