---
title: Kerberos Constrained Delegation Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6e62effcb875c0e3a1cdd6c886f3d74923e1b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403421"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>適用於：Windows Server (半年通道)、Windows Server 2016

這個適用于 IT 專業人員的總覽主題說明 Windows Server 2012 R2 和 Windows Server 2012 中 Kerberos 限制委派的新功能。

**功能描述**

Windows Server 2003 推出的 Kerberos 限制委派，可提供一種較安全的委派形式，供服務使用。 設定此功能時，限制委派會限制指定伺服器可以代表使用者執行的服務。 這需要網域系統管理員權限才能設定服務的網域帳戶，並將該帳戶限制為單一網域。 在現今的企業中，前端服務的設計不限於僅與網域中的服務整合。

在網域系統管理員可以設定服務的舊版作業系統中，服務系統管理員並沒有實用的方式可以得知哪些前端服務已委派給他們所擁有的資源服務。 而且，可以委派給資源服務的任何前端服務都代表一個潛在的攻擊點。 如果裝載前端服務的伺服器已遭到洩露，而且已將它設定為委派給資源服務，則資源服務可能也會遭到洩露。

在 Windows Server 2012 R2 和 Windows Server 2012 中，為服務設定限制委派的能力已從網域系統管理員轉移給服務系統管理員。 如此一來，後端服務系統管理員便能允許或拒絕前端服務。

如需 Windows Server 2003 推出之限制委派的詳細資訊，請參閱 [Kerberos 通訊協定轉換與限制委派](https://technet.microsoft.com/library/cc739587(v=ws.10))。

Kerberos 通訊協定的 Windows Server 2012 R2 和 Windows Server 2012 的執行包含特別用於限制委派的延伸模組。  Service for User to Proxy (S4U2Proxy) 讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。 這些擴充功能可讓您在後端服務的帳戶上設定限制委派，這可以位於另一個網域中。 如需這些擴充功能的詳細資訊，請參閱 MSDN Library 中的[\[ms-chap\]： Kerberos 通訊協定延伸模組：適用于使用者的服務和限制委派通訊協定規格](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)。

**實際應用**

限制委派讓服務系統管理員能夠指定並強制執行應用程式信任界限，方法是限制應用程式服務可以代表使用者採取行動的範圍。 服務系統管理員能夠設定哪些前端服務帳戶可以委派給它們的後端服務。

藉由在 Windows Server 2012 R2 和 Windows Server 2012 中跨網域支援限制委派，前端服務（例如 Microsoft Internet Security and 加速（ISA） Server、Microsoft Forefront Threat Management Gateway、Microsoft Exchange）Outlook Web 存取（OWA）和 Microsoft SharePoint Server 可以設定為使用限制委派來驗證其他網域中的伺服器。 這樣可以使用現有的 Kerberos 基礎結構，支援跨網域服務解決方案。 Kerberos 限制委派可以由網域系統管理員或服務系統管理員來管理。

## <a name="resource-based-constrained-delegation-across-domains"></a>跨網域的資源型限制委派

當前端服務和資源服務不在相同的網域時，可以使用 Kerberos 限制委派來提供限制委派。 服務系統管理員能夠藉由指定前端服務的網域帳戶來設定新委派，而這些前端服務可以模擬資源服務帳戶物件上的使用者。

**這個變更增加了什麼價值？**

藉由跨網域支援限制委派，可以將服務設定為使用限制委派來驗證其他網域中的伺服器，而不是使用非限制委派。 這樣便能使用現有的 Kerberos 基礎結構來提供跨網域服務解決方案的驗證支援，而不需信任要委派給任何服務的前端服務。

這也會決定伺服器是否應該信任委派身分識別的來源，從委派的網域系統管理員到資源擁有者。

**有哪些不同？**

基礎通訊協定中的變更允許跨網域的限制委派。 Kerberos 通訊協定的 Windows Server 2012 R2 和 Windows Server 2012 的執行包含服務的擴充功能，可供使用者對 Proxy （S4U2Proxy）通訊協定。 此為一組 Kerberos 通訊協定的延伸，讓服務能夠針對使用者使用它的 Kerberos 服務票證，取得從金鑰發佈中心 (KDC) 到後端服務的服務票證。

如需這些延伸模組的執行資訊，請參閱 MSDN 中的[\[ms-chap\]： Kerberos 通訊協定延伸模組：適用于使用者的服務和限制委派通訊協定規格](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx)。

相較於 Service for User (S4U) 延伸，如需有關含轉送票證授權票證 (TGT) 之 Kerberos 委派基本訊息順序的詳細資訊，請參閱 [1.3.3 通訊協定概觀](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) 一節，位於＜[MS-SFU]：Kerberos 通訊協定延伸：Service for User 與限制委派通訊協定規格＞中。

**以資源為基礎之限制委派的安全性含意**

以資源為基礎的限制委派會將委派控制權交給擁有要存取之資源的系統管理員。 這取決於資源服務的屬性，而不是受信任委派的服務。 因此，以資源為基礎的限制委派無法使用先前控制之通訊協定轉換的信任對驗證委派位。 在執行以資源為基礎的限制委派時，KDC 一律允許通訊協定轉換，如同設定的位。

因為 KDC 不會限制通訊協定轉換，所以引進了兩個新的知名 Sid，將此控制項授與資源管理員。  這些 Sid 會識別是否發生通訊協定轉換，並可與標準存取控制清單搭配使用，以視需要授與或限制存取權。

|SID|描述|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|SID，表示用戶端的身分識別是由驗證授權單位根據擁有的用戶端認證的證明來判斷。|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|SID，表示服務會判斷提示用戶端的身分識別。|

後端服務可以使用標準 ACL 運算式來判斷使用者的驗證方式。

**如何設定以資源為基礎的限制委派？**

若要設定資源服務以允許代表使用者進行前端服務存取，請使用 Windows PowerShell Cmdlet。

-   若要抓取主體清單，請使用**get-adcomputer**、 **get-uninstall-adserviceaccount**和**adserviceaccount** Cmdlet 搭配**Properties PrincipalsAllowedToDelegateToAccount**參數。

-   若要設定資源服務，請使用**get-adcomputer**、 **uninstall-adserviceaccount**、 **adserviceaccount**、 **set-get-adcomputer**、 **set-uninstall-adserviceaccount**和**adserviceaccount**參數的**Cmdlet。**

## <a name="BKMK_SOFT"></a>軟體需求
以資源為基礎的限制委派只能在執行 Windows Server 2012 R2 和 Windows Server 2012 的網域控制站上設定，但可以在混合模式樹系中套用。

您必須在執行 windows server 之前作業系統的前端和後端網域之間，于使用者帳戶網域中的所有域2012控制器上套用下列修補程式：以資源為基礎的限制委派 KDC_ERR_POLICY 失敗的環境中有 Windows Server 2008 R2 架構的網域控制站（ https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro)。
