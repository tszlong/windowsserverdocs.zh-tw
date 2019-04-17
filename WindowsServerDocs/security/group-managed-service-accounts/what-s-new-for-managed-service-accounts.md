---
title: "適用於帳號受管理的服務的新功能"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>功能與 #39; s 帳號受管理的服務的新工具

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員描述功能變更管理服務帳號導入的群組管理服務 Account (gMSA) 在 Windows Server 2012 和 Windows 8 中使用。

受管理的服務 account 的設計目的是提供服務及 Windows 服務和分享他們自己的網域帳號，而不需要手動管理所有的這些帳號密碼管理員 IIS 應用程式集區的工作。 完全受管理的核對提供管理自動密碼。

## <a name="versions"></a>管理服務帳號，在 Windows Server 2012 和 Windows 8 中的新功能
下列描述在 Windows Server 2012 和 Windows 8 中的 MSA 做哪些功能變更。

### <a name="group-managed-service-accounts"></a>帳號群組受管理的服務
核對網域中的伺服器設定之後，client 電腦就可以驗證，並連接到該服務。 之前，只有兩個 account 類型所提供的身分而不需要密碼管理。 但這些 account 類型限制：

-   電腦 account 限於網域伺服器，電腦受密碼

-   管理的服務 Account 受限於網域伺服器，並電腦管理密碼。

無法分享這些帳號跨多個系統。 因此，您必須定期維持為每個服務帳號，以避免垃圾的密碼到期每個系統。

**這項變更新增值為何？**

管理服務 Account 群組解決了這個問題，由於由 Windows Server 2012 網域控制站的密碼，並且可以擷取透過多個 Windows Server 2012 系統。 這將最小化藉由 Windows 處理密碼管理這些帳號的服務 account 管理負擔。

**有哪些方式各不相同？**

在電腦上可從一個伺服器管理執行 Windows Server 2012 或 Windows 8，可以建立和管理服務控制管理員，以便在許多執行個體的服務，例如部署伺服器陣列，透過 MSA 群組。 工具與公用程式，您用來管理受管理的服務，例如 IIS 應用程式集區管理員帳號，可以用於管理服務帳號群組。 網域系統管理員可以委派服務的系統管理員，可以管理整個週期管理服務 Account 或群組管理服務 Account 的服務管理。 現有 client 的電腦無法驗證到任何這類的服務，而不需要知道他們正在進行驗證的服務執行個體。

### <a name="interoperability"></a>移除或已取代功能
針對 Windows Server 2012、 Windows PowerShell cmdlet 管理群組管理服務帳號而伺服器管理服務帳號預設值。

## <a name="see-also"></a>也了

-   [群組多媒體受管理的服務帳號概觀](group-managed-service-accounts-overview.md)

-   [Active Directory Domain Services 概觀](active-directory-domain-services-overview.md)

-   [了解受管理的服務帳號:、 實作最佳做法，和疑難排解](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


