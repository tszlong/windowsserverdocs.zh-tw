---
title: NTLM Overview
description: Windows Server 安全性
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: d12e61fac3ba6c44dbcac35ea3097e95db54752a
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766781"
---
# <a name="ntlm-overview"></a>NTLM Overview

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明 NTLM、任何功能變更，以及提供 windows Server 2012 和舊版的 Windows 驗證與 NTLM 技術資源連結。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
NTLM 驗證是包含在 Windows Msv10.dll 中的一系列驗證通訊協定 \_ 。 NTLM 驗證通訊協定包含 LAN Manager 版本 1 與 2，以及 NTLM 版本 1 與 2。 NTLM 驗證通訊協定會根據挑戰回應機制來驗證使用者和電腦 \/ ，並向伺服器或網域控制站證明使用者知道與帳戶相關聯的密碼。 使用 NTLM 通訊協定時，資源伺服器必須採取下列其中一項動作，以便在每次需要新的存取權杖時，驗證電腦或使用者的身分識別：

-   如果帳戶是網域帳戶，請連絡網域控制站上的網域驗證服務，以取得電腦或使用者的帳戶網域。

-   如果帳戶是本機帳戶，請查詢本機帳戶資料庫中的電腦或使用者帳戶。

## <a name="current-applications"></a><a name="BKMK_APP"></a>目前的應用程式
目前仍然支援 NTLM 驗證，但是必須用於已設為工作群組成員之系統的 Windows 驗證。 NTLM 驗證也用於非網域控制站上的本機登入驗證 \- 。 Kerberos 第5版驗證是 Active Directory 環境的慣用驗證方法，但非 \- microsoft 或 Microsoft 應用程式可能仍會使用 NTLM。

在需要 NTLM 上已部署應用程式需求的知識以及設定運算環境所需策略和步驟的 IT 環境中，減少使用 NTLM 通訊協定以使用其他通訊協定。 已加入新的工具和設定，協助您探索如何使用 NTLM 以便選擇性地限制 NTLM 流量。 如需了解如何分析和限制您的環境中的 NTLM 使用量，請參閱 [NTLM 驗證的限制簡介](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10)) 來存取稽核及限制 NTLM 使用量指南。

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>新功能和變更的功能
Windows Server 2012 的 NTLM 功能沒有任何變更。

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>已移除或過時的功能
Windows Server 2012 的 NTLM 沒有移除或淘汰的功能。

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>伺服器管理員資訊
無法從伺服器管理員設定 NTLM。 您可以使用安全性原則設定或群組原則來管理電腦系統之間的 NTLM 驗證使用。 在網域中，Kerberos 是預設的驗證通訊協定。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>另請參閱
下表列出 NTLM 及其他 Windows 驗證技術的相關資源。

|內容類型|參考|
|--------|-------|
|**產品評估**|[NTLM 驗證的限制簡介](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10))<p>[NTLM 驗證的變更](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10))|
|**規劃**|[IT 基礎結構威脅模型化指南](/previous-versions/tn-archive/dd941826(v=technet.10))<p>[威脅與對策：Windows Server 2003 和 Windows XP 中的安全性設定](/previous-versions/tn-archive/dd162275(v=technet.10))<p>[威脅與對策指南：Windows Server 2008 和 Windows Vista 中的安全性設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349791(v=ws.10))<p>[威脅與對策指南：Windows Server 2008 R2 和 Windows 7 中的安全性設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125921(v=ws.10))|
|**部署**|[驗證擴充保護](https://support.microsoft.com/kb/968389)<p>[稽核和限制 NTLM 使用量指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))<p>[詢問目錄服務小組︰NTLM 封鎖和您：應用程式分析和稽核 Windows 7 中的方法](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Windows 驗證部落格](https://blogs.technet.com/authentication/)<p>[設定 NTLM 傳遞驗證的 MaxConcurrentAPI](https://support.microsoft.com/help/2688798/how-to-do-performance-tuning-for-ntlm-authentication-by-using-the-maxc)|
|**開發**|[Microsoft NTLM \( Windows\)](/windows/win32/secauthn/microsoft-ntlm)<p>[\[MS \- NLMP \] ： NT LAN Manager \( NTLM \) 驗證通訊協定規格](/openspecs/windows_protocols/ms-nlmp/b38c36ed-2804-4868-a9ff-8dd3182128e4)<p>[\[MS \- NNTP \] ： NT LAN Manager \( NTLM \) 驗證：網路新聞傳輸通訊協定 \( NNTP \) 延伸模組](/openspecs/windows_protocols/ms-nntp/73ae7d96-30fe-4750-807c-bfe7c38b3a0a)<p>[\[MS \- NTHT \] ： NTLM Over HTTP 通訊協定規格](/openspecs/windows_protocols/ms-ntht/f09cf6e1-529e-403b-a8a5-7368ee096a6a)|
|**疑難排解**|尚未提供|
|**社群資源**|[這項產品結束了嗎：NTLM 瓶頸與 RPC 執行階段](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|