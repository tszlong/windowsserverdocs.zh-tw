---
title: Kerberos Constrained Delegation Overview
description: Windows Server 安全性
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: cdee2aaecf8710b9801b689b141b16d0dbacc691
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766791"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>適用於：Windows Server (半年度管道)、Windows Server 2016

IT 專業人員的本總覽主題說明 Windows Server 2012 R2 和 Windows Server 2012 中 Kerberos 限制委派的新功能。

**功能描述**

Windows Server 2003 推出的 Kerberos 限制委派，可提供一種較安全的委派形式，供服務使用。 設定此功能時，限制委派會限制指定伺服器可以代表使用者執行的服務。 這需要網域系統管理員權限才能設定服務的網域帳戶，並將該帳戶限制為單一網域。 在現今的企業中，前端服務的設計不限於僅與其網域中的服務整合。

在較早的作業系統中，當網域系統管理員設定服務時，服務系統管理員沒有實用的方式可知道哪個前端服務被委派給他們擁有的資源服務。 此外，任何可委派給資源服務的前端服務都會暴露一個潛在的受攻擊點。 若裝載前端服務的伺服器被入侵，而且它是設定為委派給資源服務，則資源服務可能也會被入侵。

在 Windows Server 2012 R2 和 Windows Server 2012 中，為服務設定限制委派的能力已從網域系統管理員傳送給服務管理員。 使用這種方式，後端服務系統管理員可以允許或拒絕前端服務。

如需 Windows Server 2003 推出之限制委派的詳細資訊，請參閱 [Kerberos 通訊協定轉換與限制委派](/previous-versions/windows/it-pro/windows-server-2003/cc739587(v=ws.10))。

Kerberos 通訊協定的 Windows Server 2012 R2 和 Windows Server 2012 執行包括特別針對限制委派的延伸模組。  Service for User to Proxy (S4U2Proxy) 讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。 這些延伸模組可讓您在後端服務帳戶上設定限制委派，此帳戶可以位於另一個網域中。 如需有關這些延伸模組的詳細資訊，請參閱 MSDN Library 中的[ \[ Ms-chap \] ： Kerberos 通訊協定延伸： Service For User 與限制委派通訊協定規格](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94)。

**實際應用**

限制委派讓服務系統管理員能夠藉由限制應用程式服務可以代表使用者採取的範圍，來指定和強制執行應用程式信任界限。 服務系統管理員能夠設定哪些前端服務帳戶可以委派給它們的後端服務。

藉由在 Windows Server 2012 R2 和 Windows Server 2012 中跨網域支援限制委派，前端服務（例如 Microsoft Internet Security and 加速 (ISA) Server、Microsoft Forefront Threat Management Gateway、Microsoft Exchange Outlook Web 存取 (OWA) 和 Microsoft SharePoint Server 可以設定為使用限制委派來驗證其他網域中的伺服器。 這樣可以使用現有的 Kerberos 基礎結構，支援跨網域服務解決方案。 Kerberos 限制委派可以由網域系統管理員或服務系統管理員來管理。

## <a name="resource-based-constrained-delegation-across-domains"></a>跨網域的資源型限制委派

當前端服務和資源服務不在相同的網域時，可以使用 Kerberos 限制委派來提供限制委派。 服務系統管理員能夠藉由指定前端服務的網域帳戶來設定新委派，而這些前端服務可以模擬資源服務帳戶物件上的使用者。

**這個變更增加了什麼價值？**

藉由跨網域支援限制委派，可以將服務設定為使用限制委派來驗證其他網域中的伺服器，而不是使用非限制委派。 這樣便能使用現有的 Kerberos 基礎結構來提供跨網域服務解決方案的驗證支援，而不需信任要委派給任何服務的前端服務。

這也會決定伺服器是否應該信任委派的身分識別來源，從網域系統管理員委派給資源擁有者。

**有哪些不同？**

基礎通訊協定中的變更允許跨網域的限制委派。 Kerberos 通訊協定的 Windows Server 2012 R2 和 Windows Server 2012 執行包括服務的擴充功能，可供使用者 Proxy (S4U2Proxy) 通訊協定。 此為一組 Kerberos 通訊協定的延伸，讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。

如需這些擴充功能的相關資訊，請參閱 MSDN 中的[ \[ Ms-chap \] ： Kerberos 通訊協定延伸： Service For User 與限制委派通訊協定規格](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94)。

相較於 Service for User (S4U) 延伸，如需有關含轉送票證授權票證 (TGT) 之 Kerberos 委派基本訊息順序的詳細資訊，請參閱 [1.3.3 通訊協定概觀](/openspecs/windows_protocols/ms-sfu/1fb9caca-449f-4183-8f7a-1a5fc7e7290a) 一節，位於＜[MS-SFU]：Kerberos 通訊協定延伸：Service for User 與限制委派通訊協定規格＞中。

**以資源為基礎的限制委派安全性含意**

以資源為基礎的限制委派可將委派的控制權交給擁有所要存取資源的系統管理員。 這取決於資源服務的屬性，而非受信任委派的服務。 如此一來，以資源為基礎的限制委派無法使用先前控制通訊協定轉換的受信任的驗證委派位。 當您執行以資源為基礎的限制委派時（如同設定的位），KDC 一律會允許通訊協定轉換。

因為 KDC 不會限制通訊協定轉換，所以引進了兩個新的知名 Sid，將此控制項授與資源管理員。  這些 Sid 會識別是否發生通訊協定轉換，而且可以搭配標準存取控制清單使用，以視需要授與或限制存取權。

|SID|描述|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|表示用戶端身分識別的 SID，是由驗證授權單位根據擁有的用戶端認證來判斷提示。|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|表示服務會判斷用戶端身分識別的 SID。|

後端服務可以使用標準 ACL 運算式來判斷使用者的驗證方式。

**如何設定以資源為基礎的限制委派？**

若要設定資源服務以允許代表使用者進行前端服務存取，請使用 Windows PowerShell Cmdlet。

-   若要取出主體清單，請使用 **get-adcomputer**、 **uninstall-adserviceaccount**和 **set-aduser** Cmdlet 搭配 **Properties >serverc** 參數。

-   若要設定資源服務，請使用 **新的-get-adcomputer**、 **uninstall-adserviceaccount**、 **set-aduser**、 **set-get-adcomputer**、 **Set-uninstall-adserviceaccount**和 **set-set-aduser** Cmdlet 搭配 **>serverc** 參數。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
以資源為基礎的限制委派只能在執行 Windows Server 2012 R2 和 Windows Server 2012 的網域控制站上進行設定，但可在混合模式樹系內套用。

您必須將下列的修正程式套用到在執行 windows server 之前的作業系統之前端和後端網域的路徑上的使用者帳戶網域中執行 Windows Server 2012 的所有網域控制站：以資源為基礎的限制委派 KDC_ERR_POLICY 在具有 Windows Server 2008 R2 網域控制站 (的環境中發生失敗 https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro) 。