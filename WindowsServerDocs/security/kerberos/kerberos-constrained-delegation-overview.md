---
title: "Kerberos 限制委派概觀"
description: "Windows Server 安全性"
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos 限制委派概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

適用於 IT 專業人員此概觀主題描述 Kerberos 限制委派 Windows Server 2012 R2 和 Windows Server 2012 中的新功能。

## <a name="feature-description"></a>描述的功能
Windows Server 2003，以提供更安全地表單的委派無法使用的服務中引進委派 Kerberos 限制。 設定之後，限制的委派限制的使用者代表指定的伺服器可做的服務。 這需要設定核對服務網域系統管理員權限，而且會限制 account 單一網域。 今天的企業版，前端服務並未設計成限於整合只他們網域中的服務。

更早版本作業系統網域系統管理員位置設定的服務，在服務系統管理員必須知道委派給其所擁有的資源服務的前端服務不實用的方式。 並無法委派給資源服務任何前端服務代表潛在的攻擊點。 如果受損裝載前端服務的伺服器，並設定委派給資源服務，可能也入侵資源服務。

Windows Server 2012 R2 和 Windows Server 2012 中，若要設定限制的委派服務的能力已經傳輸網域系統管理員從服務系統管理員。 如此一來後, 端服務系統管理員可以允許或拒絕前端服務。

限制委派的詳細資訊的為引進了 Windows Server 2003，查看[Kerberos 通訊協定轉換和限制委派](https://technet.microsoft.com/library/cc739587(v=ws.10))。

Windows Server 2012 R2 和 Windows Server 2012 的 Kerberos 通訊協定實作包括專為限制委派擴充功能。  使用者 Proxy (S4U2Proxy) 服務可讓您使用使用者其 Kerberos 服務票證後端服務取得的服務票證金鑰 Distribution 中心 (KDC) 的服務。 這些擴充功能讓限制的委派後端服務的帳號，另一個網域中的設定。 如需有關這些擴充功能的詳細資訊，請查看[\[MS-SFU\]: Kerberos 通訊協定擴充功能：使用者和限制委派通訊協定規格服務](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)MSDN 媒體櫃中。

## <a name="practical-applications"></a>實用的應用程式
限制的委派提供服務的系統管理員指定並執行的應用程式信任邊界藉由限制位置服務的應用程式可以依據代表使用者範圍的功能。 服務系統管理員可以設定，可以將前端服務帳號委派給他們後端服務。

藉由支援限制的委派網域中的 Windows Server 2012 R2 和 Windows Server 2012 上，可以使用限制的委派給其他網域中的伺服器的驗證設定前端服務，例如 Microsoft 網際網路安全性加速 (ISA) 伺服器、Microsoft Forefront 威脅管理閘道、Microsoft Exchange Outlook Web Access (OWA)，並 Microsoft SharePoint 伺服器。 這提供支援跨網域服務方案，使用現有的 Kerberos 基礎結構。 網域系統管理員或服務的系統管理員可以管理委派 Kerberos 限制。

## <a name="new-and-changed-functionality"></a>新功能和變更功能
**跨網域型資源限制的委派**

當前端服務和資源服務不在相同的網域中提供限制的委派可委派 Kerberos 限制。 服務管理員是可以指定前端服務，可模擬 account 物件的資源服務使用者的網域帳號設定新委派。

**這項變更新增值為何？**

藉由支援限制的委派跨網域，可以使用限制的委派中其他網域，而不要使用 [無限制的委派伺服器驗證服務設定。 這信任前端服務委派給任何服務，而不需要使用現有 Kerberos 基礎結構提供驗證支援跨網域服務方案。

**有哪些方式各不相同？**

變更基礎通訊協定跨網域可限制的委派。 Windows Server 2012 R2 和 Windows Server 2012 的 Kerberos 通訊協定實作包含 Proxy (S4U2Proxy) 通訊協定使用者服務擴充功能。 這是一組擴充功能來 Kerberos 通訊協定，允許使用使用者其 Kerberos 服務票證後端服務取得的服務票證金鑰 Distribution 中心 (KDC) 的服務。

這些擴充功能實作資訊，請查看[\[MS-SFU\]: Kerberos 通訊協定擴充功能：使用者和限制委派通訊協定規格服務](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx)在 MSDN。

基本訊息順序 Kerberos 委派與轉送票證授與票證 (TGT) 相較於服務使用者 (S4U) 擴充功能的相關詳細資訊，會看到一節[1.3.3 通訊協定概觀](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx)在 [MS-SFU]: Kerberos 通訊協定擴充功能：使用者和限制委派通訊協定規格服務。

若要設定允許前端服務存取的使用者代表資源服務，請使用 Windows PowerShell cmdlet。

-   擷取一系列原則，使用**取得-ADComputer**，**取得-ADServiceAccount**，並**取得-ADUser** cmdlet 使用**屬性 PrincipalsAllowedToDelegateToAccount**參數。

-   若要設定資源服務，使用**新-ADComputer**，**新-ADServiceAccount**，**新-ADUser**，**設定-ADComputer**，**設定-ADServiceAccount**，和**設定-ADUser** cmdlet 與**PrincipalsAllowedToDelegateToAccount**參數。

## <a name="BKMK_SOFT"></a>軟體需求
資源型限制的委派只能執行 Windows Server 2012、Windows Server 2012 R2 網域控制站設定，但可以在混合模式樹系套用。

您必須將下列 hotfix 套用到所有執行 Windows Server 2012 中使用者 account 網域推薦路徑更早版本與 Windows Server 執行作業系統前端和端網域之間的網域控制站：資源型限制委派 KDC_ERR_POLICY 失敗 Windows Server 2008 R2 網域控制站環境中。
