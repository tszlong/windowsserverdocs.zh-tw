---
title: 疑難排解 AD FS
description: 瞭解如何疑難排解 AD FS 的各種層面。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.openlocfilehash: 855d4fd24b0cc991c11241013dabcb2310887dd1
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965750"
---
# <a name="troubleshooting-ad-fs"></a>疑難排解 AD FS
AD FS 有很多工作，觸及許多不同的專案，並有許多不同的相依性。  當然，這可能會導致各種問題。  本檔的設計目的是協助您開始針對這些問題進行疑難排解。  本檔將為您介紹您應著重的一般區域、如何啟用功能以提供其他資訊，以及各種可用來追蹤問題的工具。

>[!NOTE]
>如需其他資訊，請參閱 [ADFS](https://adfshelp.microsoft.com) 說明，它可在單一位置提供有效的工具，讓使用者和系統管理員能更快地以更快的步調解決驗證問題。


## <a name="what-to-check-first"></a>要先檢查什麼
深入探討疑難排解之前，您應該先檢查幾件事。  分別是：
- **DNS** 設定-可以解析 federation service 的名稱嗎？  這應該會解析為負載平衡器的 IP 位址，或伺服器陣列中其中一個 AD FS 伺服器的 IP 位址。  如需詳細資訊，請參閱 [AD FS 疑難排解-DNS](ad-fs-tshoot-dns.md)。
- **AD FS 端點** -是否可以流覽至 AD FS 端點？  藉由流覽至此，您就可以判斷您的 AD FS 網頁伺服器是否回應要求。  如果您可以找到這個檔案，則您知道 AD FS 可以正常地處理443的要求。  如需詳細資訊，請參閱 [AD FS 疑難排解-端點](ad-fs-tshoot-endpoints.md)。
- **Idp 起始** 的登入-您可以透過 Idp-Initiated 登入頁面登入並進行驗證嗎？  您必須確定已啟用此頁面，因為它預設為停用。  使用 `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` 啟用頁面。  如果您可以登入並進行驗證，則您知道 AD FS 在此區域中運作。  如需詳細資訊，請參閱 [AD FS 疑難排解-登錄](ad-fs-tshoot-initiatedsignon.md)。
  ##  <a name="common-troubleshooting-areas"></a>常見的疑難排解區域

|名稱|描述|
|-----|-----|
|[事件記錄和審核](ad-fs-tshoot-logging.md)|您可以使用 Windows 事件記錄，透過系統管理員和追蹤記錄來查看高階層級和低層級的資訊。  它也可以用來查看安全性審核。|
|[SQL 連線能力](ad-fs-tshoot-sql.md)|測試 AD FS 伺服器與後端 SQL 資料庫之間的連線能力的資訊|
|[宣告發行](ad-fs-tshoot-claims-issuance.md)|判斷 AD FS 是否正確發出宣告的資訊。|
|[迴圈偵測](ad-fs-tshoot-loop.md)|決定和防止使用者在 Idp 和 RP 之間來回切換的資訊。|
|[憑證](ad-fs-tshoot-certs.md)|可能發生的一般憑證問題|
|[Fiddler](ad-fs-tshoot-fiddler.md)|如何安裝和使用 Fiddler 的資訊|
|[WS-同盟與 Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|WS-Federation 互動的詳細 Fiddler 追蹤|
|[宣告規則](ad-fs-tshoot-claims-rules.md)|針對宣告規則及其語法進行疑難排解的相關資訊|
|[整合式 Windows 驗證](ad-fs-tshoot-iwa.md)|針對整合式驗證進行疑難排解的資訊。|
|[Azure AD](ad-fs-tshoot-azure.md)|疑難排解 AD FS 與 Azure AD 互動的資訊。|
|[AD FS 診斷分析器](ad-fs-diagnostics-analyzer.md)|AD FS 協助診斷分析器可以使用診斷 PowerShell 模組來執行基本的 AD FS 檢查。
