---
title: Kerberos Constrained Delegation Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821929"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>適用於：Windows Server （半年通道），Windows Server 2016

此概觀主題適用於 IT 專業人員說明 Windows Server 2012 R2 和 Windows Server 2012 中的 Kerberos 限制委派的新功能。

**功能描述**

Windows Server 2003 推出的 Kerberos 限制委派，可提供一種較安全的委派形式，供服務使用。 設定此功能時，限制委派會限制指定伺服器可以代表使用者執行的服務。 這需要網域系統管理員權限才能設定服務的網域帳戶，並將該帳戶限制為單一網域。 在現今的企業，前端服務不會設計為限制為只有在其網域中的服務與整合。

在網域系統管理員可以設定服務的舊版作業系統中，服務系統管理員並沒有實用的方式可以得知哪些前端服務已委派給他們所擁有的資源服務。 而且，可以委派給資源服務的任何前端服務都代表一個潛在的攻擊點。 如果裝載前端服務的伺服器已遭到洩露，而且已將它設定為委派給資源服務，則資源服務可能也會遭到洩露。

在 Windows Server 2012 R2 和 Windows Server 2012 中，若要設定服務的限制的委派的能力已從移轉網域系統管理員為服務系統管理員。 如此一來，後端服務系統管理員便能允許或拒絕前端服務。

如需 Windows Server 2003 推出之限制委派的詳細資訊，請參閱 [Kerberos 通訊協定轉換與限制委派](https://technet.microsoft.com/library/cc739587(v=ws.10))。

Windows Server 2012 R2 和 Windows Server 2012 的 Kerberos 通訊協定實作包含特別適用於限制委派的延伸模組。  Service for User to Proxy (S4U2Proxy) 讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。 這些擴充功能可讓後端服務的帳戶，可以位於另一個網域上設定限制的委派。 如需有關這些延伸模組的詳細資訊，請參閱 < [ \[MS-SFU\]:Kerberos 通訊協定延伸：Service for User 與限制的委派通訊協定規格](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)MSDN Library 中。

**實際的應用程式**

限制的委派讓服務系統管理員指定與強制執行限制的範圍，應用程式服務可以代表使用者的應用程式信任界限的能力。 服務系統管理員能夠設定哪些前端服務帳戶可以委派給它們的後端服務。

由 Windows Server 2012 R2 和 Windows Server 2012 中，前端服務，例如 Microsoft Internet Security and Acceleration (ISA) Server、 Microsoft Forefront Threat Management Gateway、 Microsoft Exchange 中跨網域支援限制委派Outlook Web Access (OWA) 和 Microsoft SharePoint Server，都可以設定為使用限制的委派來驗證其他網域中的伺服器。 這樣可以使用現有的 Kerberos 基礎結構，支援跨網域服務解決方案。 Kerberos 限制委派可以由網域系統管理員或服務系統管理員來管理。

## <a name="resource-based-constrained-delegation-across-domains"></a>跨網域的資源型限制委派

當前端服務和資源服務不在相同的網域時，可以使用 Kerberos 限制委派來提供限制委派。 服務系統管理員能夠藉由指定前端服務的網域帳戶來設定新委派，而這些前端服務可以模擬資源服務帳戶物件上的使用者。

**這個變更增加了什麼價值？**

藉由跨網域支援限制委派，可以將服務設定為使用限制委派來驗證其他網域中的伺服器，而不是使用非限制委派。 這樣便能使用現有的 Kerberos 基礎結構來提供跨網域服務解決方案的驗證支援，而不需信任要委派給任何服務的前端服務。

這也會進入伺服器是否應該信任來源的資源擁有者委派從委派從網域系統管理員身分識別的決策。

**有哪些不同？**

基礎通訊協定中的變更允許跨網域的限制委派。 Windows Server 2012 R2 和 Windows Server 2012 的 Kerberos 通訊協定實作包含 service for User to Proxy (S4U2Proxy) 通訊協定的延伸模組。 此為一組 Kerberos 通訊協定的延伸，讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。

如需有關這些延伸模組的實作資訊，請參閱[ \[MS-SFU\]:Kerberos 通訊協定延伸：Service for User 與限制的委派通訊協定規格](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx)MSDN 中。

相較於 Service for User (S4U) 延伸，如需有關含轉送票證授權票證 (TGT) 之 Kerberos 委派基本訊息順序的詳細資訊，請參閱 [1.3.3 通訊協定概觀](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx)一節，位於＜[MS-SFU]：Kerberos 通訊協定延伸：Service for User 與限制委派通訊協定規格＞中。

**安全性意涵之資源型限制委派**

資源型限制委派將控制項的手中的系統管理員擁有所存取之資源的委派。 這取決於資源服務，而不是受信任委派的服務屬性。 如此一來，資源型限制的委派無法使用先前控制通訊協定轉換的受信任-至-驗證-進行-委派位元。 KDC 會永遠允許通訊協定轉換，執行資源型限制委派，就好像已設為位元時。

因為 KDC 不會限制通訊協定轉換，導入兩個新的已知 Sid 給這個控制項到資源管理員。  這些 Sid 找出是否發生，通訊協定轉換，並可以搭配標準存取控制清單來授與或限制存取所需。

|SID|描述|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|表示用戶端的身分識別的 SID 為基礎的用戶端認證的擁有權證明驗證授權單位所判斷提示。|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|表示用戶端的身分識別的 SID 是由服務所判斷提示。|

後端服務可以使用標準的 ACL 運算式，以判斷使用者的驗證方式。

**您要如何設定資源型限制委派？**

若要設定資源服務以允許代表使用者進行前端服務存取，請使用 Windows PowerShell Cmdlet。

-   若要擷取主體清單，請使用**Get-adcomputer**， **Get-adserviceaccount**，並**Get-aduser** cmdlet 搭配**屬性PrincipalsAllowedToDelegateToAccount**參數。

-   若要設定資源服務，請使用**New-adcomputer**， **New-adserviceaccount**， **New-aduser**， **Set-adcomputer**， **Set-adserviceaccount**，並**Set-aduser**指令程式，其**PrincipalsAllowedToDelegateToAccount**參數。

## <a name="BKMK_SOFT"></a>軟體需求
資源型限制的委派只能設定執行 Windows Server 2012 R2 和 Windows Server 2012，網域控制站上，但可以混合模式的樹系內套用。

您必須將下列 hotfix 套用到執行 Windows Server 2012 中使用者帳戶網域轉介路徑上執行作業系統的版本早於 Windows Server 的前端和後端網域間的所有網域控制站：資源型限制委派 KDC_ERR_POLICY 在含有 Windows Server 2008 R2 網域控制站的環境中失敗。
