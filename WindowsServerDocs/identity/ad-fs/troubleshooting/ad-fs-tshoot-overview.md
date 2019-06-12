---
title: 疑難排解 AD FS
description: 本文件說明如何疑難排解各個層面的 AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6410d510085d1772ca6d8ced47226e00239a1a02
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443907"
---
# <a name="troubleshooting-ad-fs"></a>疑難排解 AD FS
AD FS 有很多的移動部分，接觸到許多不同的因素，有許多不同的相依性。  當然，這可以常理的各種問題。  這份文件被設計來協助您開始疑難排解這些問題。  本文件將為您介紹您應該專注於如何啟用功能的詳細資訊，及各種工具，可用來追蹤問題的一般區域。  

>[!NOTE]
>如需詳細資訊，請參閱[ADFS 協助](http://adfshelp.microsoft.com)可在其中一個有效的工具來簡化了使用者和系統管理員以解決驗證問題，更快的步調。 


## <a name="what-to-check-first"></a>要檢查第一個項目
您了解深入的疑難排解之前，有幾件事，您應該先檢查。  其中包括：
- **DNS 設定**-您可以解析 federation service 名稱嗎？  這應該解析為其中一個負載平衡器 IP 位址或其中一個伺服器陣列中的 AD FS 伺服器的 IP 位址。  如需詳細資訊，請參閱[AD FS 疑難排解-DNS](ad-fs-tshoot-dns.md)。
- **AD FS 端點**-您可以瀏覽至 AD FS 端點嗎？  藉由瀏覽至此，您可以判斷您的 AD FS web 伺服器回應要求。  如果您可以取得這個檔案，您便知道 AD FS 會服務要求透過 443 沒有問題。  如需詳細資訊，請參閱[AD FS 疑難排解-端點](ad-fs-tshoot-endpoints.md)。
- **Idp 起始登入**-登入和驗證透過 Idp-Initiated 登入 頁面？  您必須確定已啟用此頁面，因為它預設會停用。  使用`Set-AdfsProperties -EnableIdPInitiatedSignOn $true`若要啟用的頁面。  如果您可以登入，並驗證您知道 AD FS 會使用此區域中。  如需詳細資訊，請參閱[AD FS 疑難排解-登入](ad-fs-tshoot-initiatedsignon.md)。
  ##  <a name="common-troubleshooting-areas"></a>疑難排解的常見區域

|名稱|描述|
|-----|-----|
|[事件記錄與稽核](ad-fs-tshoot-logging.md)|您可以使用 Windows 事件記錄檔來檢視高層級和低層級資訊透過系統管理和追蹤記錄檔。  它也可以用來檢視安全性稽核。|
|[SQL 連線能力](ad-fs-tshoot-sql.md)|在測試 AD FS 伺服器和後端 SQL database 之間的連線資訊|
|[宣告發行](ad-fs-tshoot-claims-issuance.md)|判斷是否 AD FS 發出宣告正確的資訊。|
|[迴圈偵測](ad-fs-tshoot-loop.md)|判斷及防止使用者在 Idp 和 RP 之間來回退資訊。|
|[憑證](ad-fs-tshoot-certs.md)|可能發生的一般憑證問題|
|[Fiddler](ad-fs-tshoot-fiddler.md)|有關如何安裝和使用 Fiddler|
|[使用 Fiddler 的 WS-同盟](ad-fs-tshoot-fiddler-ws-fed.md)|詳細的 WS-同盟互動的 Fiddler 追蹤|
|[宣告規則](ad-fs-tshoot-claims-rules.md)|疑難排解 宣告規則和其語法的詳細資訊|
|[整合式的 Windows 驗證](ad-fs-tshoot-iwa.md)|有關疑難排解整合式的驗證。|
|[Azure AD](ad-fs-tshoot-azure.md)|有關疑難排解 AD FS 與 Azure AD 之間的互動。|
|[AD FS 診斷分析器](ad-fs-diagnostics-analyzer.md)|AD FS 協助診斷分析器，可協助執行基本的 AD FS 檢查使用的診斷的 PowerShell 模組。 