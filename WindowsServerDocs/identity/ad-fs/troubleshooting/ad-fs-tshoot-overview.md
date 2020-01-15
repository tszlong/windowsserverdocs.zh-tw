---
title: 疑難排解 AD FS
description: 本檔說明如何針對 AD FS 的各個層面進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fc5c2384a6d8d13f807a1ce99c5db78bee5b108
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950133"
---
# <a name="troubleshooting-ad-fs"></a>疑難排解 AD FS
AD FS 有很多移動部分，觸及許多不同的東西，而且有許多不同的相依性。  自然地，這可能會增加各種問題。  本檔的設計目的是要協助您開始針對這些問題進行疑難排解。  本檔將為您介紹應專注于的一般區域、如何啟用其他資訊的功能，以及可用於追蹤問題的各種工具。  

>[!NOTE]
>如需其他資訊，請參閱[ADFS](https://adfshelp.microsoft.com)說明，這會在單一位置提供有效的工具，讓使用者和系統管理員更快速地解決驗證問題。 


## <a name="what-to-check-first"></a>首先要檢查的內容
深入瞭解疑難排解之前，您應該先檢查幾件事。  這些類型包括：
- **DNS**設定-您可以解析 federation service 的名稱嗎？  這應該會解析為伺服器陣列中其中一個 AD FS 伺服器的負載平衡器 IP 位址或 IP 位址。  如需詳細資訊，請參閱[AD FS 疑難排解-DNS](ad-fs-tshoot-dns.md)。
- **AD FS 端點**-您可以流覽至 AD FS 端點嗎？  藉由流覽至此方法，您可以判斷您的 AD FS web 伺服器是否正在回應要求。  如果您可以前往這個檔案，那麼您就知道 AD FS 是透過443來服務要求的問題。  如需詳細資訊，請參閱[AD FS 疑難排解-端點](ad-fs-tshoot-endpoints.md)。
- **Idp 起始**的登入-您可以透過 [Idp 起始的登入] 頁面來登入和驗證嗎？  您必須確定此頁面已啟用，因為它預設為停用。  使用 `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` 來啟用頁面。  如果您可以登入並進行驗證，則您知道 AD FS 在此區域中運作。  如需詳細資訊，請參閱[AD FS 疑難排解-登錄](ad-fs-tshoot-initiatedsignon.md)。
  ##  <a name="common-troubleshooting-areas"></a>常見的疑難排解區域

|名稱|說明|
|-----|-----|
|[事件記錄和審核](ad-fs-tshoot-logging.md)|使用 Windows 事件記錄檔，透過系統管理員和追蹤記錄來查看高階和低層級的資訊。  它也可以用來查看安全性的審核。|
|[SQL 連線能力](ad-fs-tshoot-sql.md)|測試 AD FS 伺服器與後端 SQL 資料庫之間連接的資訊|
|[宣告發行](ad-fs-tshoot-claims-issuance.md)|判斷 AD FS 是否正確發出宣告的資訊。|
|[迴圈偵測](ad-fs-tshoot-loop.md)|有關判斷和防止使用者在 Idp 和 RP 之間來回回復的資訊。|
|[憑證](ad-fs-tshoot-certs.md)|Typcial 可能發生的憑證問題|
|[Fiddler](ad-fs-tshoot-fiddler.md)|有關如何安裝和使用 Fiddler 的資訊|
|[WS-同盟與 Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|WS-同盟互動的詳細 Fiddler 追蹤|
|[宣告規則](ad-fs-tshoot-claims-rules.md)|針對宣告規則和其語法進行疑難排解的相關資訊|
|[整合式 Windows 驗證](ad-fs-tshoot-iwa.md)|針對整合式驗證進行疑難排解的資訊。|
|[Azure AD](ad-fs-tshoot-azure.md)|與 Azure AD AD FS 互動疑難排解的資訊。|
|[AD FS 診斷分析器](ad-fs-diagnostics-analyzer.md)|AD FS Help Diagnostics Analyzer 可協助您使用診斷 PowerShell 模組執行基本的 AD FS 檢查。 