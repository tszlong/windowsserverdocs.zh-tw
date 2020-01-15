---
title: NTLM Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 74e6e05df037041b6c80239e26b39b5566474326
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949752"
---
# <a name="ntlm-overview"></a>NTLM Overview

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題適用于 IT 專業人員，說明 NTLM、任何功能變更，並提供 windows 驗證的技術資源連結，以及 Windows Server 2012 和舊版的 NTLM。

## <a name="BKMK_OVER"></a>功能描述
NTLM 驗證是包含在 Windows Msv1\_0 .dll 中的一系列驗證通訊協定。 NTLM 驗證通訊協定包含 LAN Manager 版本 1 與 2，以及 NTLM 版本 1 與 2。 NTLM 驗證通訊協定會根據挑戰來驗證使用者和電腦\/回應機制，以向伺服器或網域控制站證明使用者知道與帳戶相關聯的密碼。 使用 NTLM 通訊協定時，資源伺服器必須採取下列其中一項動作，以便在每次需要新的存取權杖時，驗證電腦或使用者的身分識別：

-   如果帳戶是網域帳戶，請連絡網域控制站上的網域驗證服務，以取得電腦或使用者的帳戶網域。

-   如果帳戶是本機帳戶，請查詢本機帳戶資料庫中的電腦或使用者帳戶。

## <a name="BKMK_APP"></a>目前的應用程式
目前仍然支援 NTLM 驗證，但是必須用於已設為工作群組成員之系統的 Windows 驗證。 NTLM 驗證也用於非\-網域控制站上的本機登入驗證。 Kerberos 第5版驗證是 Active Directory 環境慣用的驗證方法，但非\-的 Microsoft 或 Microsoft 應用程式可能仍使用 NTLM。

在需要 NTLM 上已部署應用程式需求的知識以及設定運算環境所需策略和步驟的 IT 環境中，減少使用 NTLM 通訊協定以使用其他通訊協定。 已加入新的工具和設定，協助您探索如何使用 NTLM 以便選擇性地限制 NTLM 流量。 如需了解如何分析和限制您的環境中的 NTLM 使用量，請參閱 [NTLM 驗證的限制簡介](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) 來存取稽核及限制 NTLM 使用量指南。

## <a name="BKMK_NEW"></a>新的和已變更的功能
Windows Server 2012 的 NTLM 功能沒有任何變更。

## <a name="BKMK_DEP"></a>已移除或過時的功能
Windows Server 2012 的 NTLM 沒有已移除或過時的功能。

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
無法從伺服器管理員設定 NTLM。 您可以使用安全性原則設定或群組原則來管理電腦系統之間的 NTLM 驗證使用。 在網域中，Kerberos 是預設的驗證通訊協定。

## <a name="BKMK_LINKS"></a>另請參閱
下表列出 NTLM 及其他 Windows 驗證技術的相關資源。

|內容類型|參考資料|
|--------|-------|
|**產品評估**|[NTLM 驗證的限制簡介](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[NTLM 驗證的變更](https://technet.microsoft.com/library/dd566199.aspx)|
|**規劃**|[IT 基礎結構威脅模型化指南](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[威脅與因應對策： Windows Server 2003 和 Windows XP 中的安全性設定](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[威脅與對策指南： Windows Server 2008 和 Windows Vista 中的安全性設定](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[威脅與對策指南： Windows Server 2008 R2 和 Windows 7 中的安全性設定](https://technet.microsoft.com/library/hh125921.aspx)|
|**部署**|[驗證擴充保護](https://support.microsoft.com/kb/968389)<br /><br />[審核和限制 NTLM 使用方式指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[詢問目錄服務小組： NTLM 封鎖和您： Windows 7 中的應用程式分析和審核方法](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Windows 驗證 Blog](https://blogs.technet.com/authentication/)<br /><br />[透過驗證設定 NTLM pass\-的 MaxConcurrentAPI](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**開發**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]： NT LAN Manager \(NTLM\) 驗證通訊協定規格](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]： NT LAN Manager \(NTLM\) 驗證：網路新聞傳輸通訊協定 \(NNTP\) 延伸模組](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]： NTLM Over HTTP 通訊協定規格](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**疑難排解**|尚未提供|
|**社群資源**|[這是不正確： NTLM 瓶頸與 RPC 執行時間](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



