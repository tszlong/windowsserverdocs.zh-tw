---
title: "NTLM 概觀"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>NTLM 概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員描述 NTLM 任何變更功能，並提供 Windows 驗證 NTLM for Windows Server 2012 和舊版的技術資源的連結。

## <a name="BKMK_OVER"></a>描述的功能
NTLM 驗證」會包含在 Windows Msv1\_0.dll 驗證通訊協定的家庭。 NTLM 驗證通訊協定包括 At 版本 1、2、和 NTLM 第 1 版和 2。 NTLM 驗證通訊協定驗證使用者與電腦根據證明使用者知道密碼 account 相關聯的網域控制站伺服器或回應 challenge\ 日機制。 資源伺服器 NTLM 通訊協定使用時，必須執行下列動作，以驗證身分使用者或電腦時需要新的憑證存取其中一項：

-   Account 核對是否尋求的電腦或使用者 account 網域網域控制站網域驗證服務。

-   如果 account 本機 account，查看電腦或使用者的本機 account 資料庫中 account。

## <a name="BKMK_APP"></a>目前的應用程式
NTLM 驗證仍然受支援，及 Windows 驗證必須使用與系統設定為工作群組成員。 NTLM 驗證也可用於 non\ 網域控制站在登入本機驗證。 F:kerberos 版本 5 驗證慣用的驗證方法 Active Directory 環境中，但 non\ Microsoft 或 Microsoft 應用程式可能仍然會使用 NTLM。

減少 IT 環境中 NTLM 通訊協定] 的使用量需要部署的應用程式需求的兩個知識 NTLM 策略和步驟來設定電腦環境使用其他通訊協定。 已可協助您了解如何使用 NTLM 以選擇性限制 NTLM 流量新增新的工具與設定。 如如何分析及限制 NTLM 使用您的環境中相關資訊，請查看[簡介限制 NTLM 驗證](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx)存取稽核和限制 NTLM 使用量指南。

## <a name="BKMK_NEW"></a>新功能和變更功能
Windows Server 2012 的 NTLM 的功能有任何變更。

## <a name="BKMK_DEP"></a>移除或已取代功能
還有移除或已被取代 NTLM for Windows Server 2012 的功能。

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
您無法從伺服器管理員設定 NTLM。 您可以使用的安全性原則設定或群組原則管理之間的電腦系統 NTLM 驗證使用量。 在網域中，Kerberos 是預設驗證通訊協定。

## <a name="BKMK_LINKS"></a>也了
下表列出相關資源 NTLM 和其他 Windows 驗證技術。

|內容類型|資訊尋找參考資料|
|--------|-------|
|**Product 評估**|[簡介 NTLM 驗證的限制](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[變更 NTLM 驗證](https://technet.microsoft.com/library/dd566199.aspx)|
|**規劃**|[IT 基礎結構威脅模型指南](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[威脅和措施：在 Windows Server 2003 及 Windows XP 的安全性設定](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[威脅和措施快速入門：Windows Server 2008 和 Windows Vista 中的安全性設定](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[威脅和措施快速入門：Windows Server 2008 R2 和 Windows 7 中的安全性設定](https://technet.microsoft.com/library/hh125921.aspx)|
|**部署**|[驗證延伸的保護](https://support.microsoft.com/kb/968389)<br /><br />[稽核和限制 NTLM 使用指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[要求服務 Directory 小組：NTLM 封鎖與您：應用程式分析及稽核在 Windows 7 中的方法](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Windows 驗證部落格](https://blogs.technet.com/authentication/)<br /><br />[設定 MaxConcurrentAPI NTLM pass\ 透過驗證](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**開發**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT At \(NTLM\) 驗證通訊協定規格](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT At \(NTLM\) 驗證：網路消息傳輸通訊協定 \(NNTP\) 擴充功能](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: HTTP 通訊協定規格透過 NTLM](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**疑難排解**|未提供|
|**社群資源**|[死亡尚未，這個馬：NTLM 瓶頸和 RPC 執行階段](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



