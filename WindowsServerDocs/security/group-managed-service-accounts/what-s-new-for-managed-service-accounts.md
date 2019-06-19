---
title: What's New for Managed Service Accounts
description: Windows Server 安全性
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872179"
---
# <a name="what39s-new-for-managed-service-accounts"></a>什麼&#39;的新的受管理的服務帳戶

>適用於：Windows Server （半年通道），Windows Server 2016

本主題適用於 IT 專業人員的功能，受管理的服務帳戶群組受控服務帳戶 (gMSA) （Windows Server 2012 和 Windows 8 中引進，描述所做的變更。

受管理的服務帳戶主要用於提供服務與工作，例如 Windows 服務和 IIS 應用程式集區，以共用自己的網域帳戶，同時讓系統管理員不需手動管理這些帳戶的密碼。 這是一個提供自動密碼管理的受管理的網域帳戶。

## <a name="versions"></a>什麼是 Windows Server 2012 和 Windows 8 中的受控服務帳戶的新功能
以下說明的功能變更已對 Windows Server 2012 和 Windows 8 中的 MSA。

### <a name="group-managed-service-accounts"></a>群組受管理的服務帳戶
在網域中為伺服器設定網域帳戶時，用戶端電腦可以驗證並連接至該服務。 之前只有兩種帳戶類型在不需要密碼管理的情況下提供身分識別。 但是這些帳戶類型有所限制：

-   電腦帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

-   受管理的服務帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

這些帳戶無法跨多個系統進行共用。 因此，您必須定期維護每個系統上的每個服務的帳戶，以防不必要的密碼過期狀況。

**這個變更增加了什麼價值？**

群組受控服務帳戶會解決此問題，因為帳戶密碼由 Windows Server 2012 網域控制站管理，且可以擷取多個 Windows Server 2012 的系統。 這允許 Windows 處理這些帳戶的密碼管理，將服務帳戶的管理負擔降至最低。

**有哪些不同？**

在電腦上執行 Windows Server 2012 或 Windows 8 中，群組 MSA 可以建立和管理透過服務控制管理員中，如此多的執行個體的服務，例如透過伺服器陣列中，部署可從一部伺服器。 您用來管理受管理的服務帳戶的工具和公用程式，例如 IIS 應用程式集區管理員，可以搭配群組受管理的服務帳戶使用。 網域管理員可以將服務管理委派給服務管理員，以管理受管理的服務帳戶或群組受管理的服務帳戶的整個週期。 現有的用戶端電腦將能夠向任何這種服務進行驗證，而不需要知道它們向哪個服務執行個體進行驗證。

### <a name="interoperability"></a>已移除或過時的功能
Windows Server 2012 中，Windows PowerShell cmdlet 預設為管理群組受控服務帳戶而非伺服器受管理服務帳戶。

## <a name="see-also"></a>另請參閱

-   [群組受管理的服務帳戶概觀](group-managed-service-accounts-overview.md)

-   [Active Directory Domain Services 概觀](active-directory-domain-services-overview.md)

-   [受管理的服務帳戶：了解、 實作、 最佳做法和疑難排解](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


