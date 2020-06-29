---
title: What's New for Managed Service Accounts
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-gmsa
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 1411f312def0da79de4c18d6d652e0223ea27b48
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474445"
---
# <a name="what39s-new-for-managed-service-accounts"></a>受管理的服務帳戶的新功能&#39;

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明在 Windows Server 2012 和 Windows 8 中引進群組受管理的服務帳戶（gMSA）時，受管理的服務帳戶的功能變更。

受管理的服務帳戶主要用於提供服務與工作，例如 Windows 服務和 IIS 應用程式集區，以共用自己的網域帳戶，同時讓系統管理員不需手動管理這些帳戶的密碼。 這是一個提供自動密碼管理的受管理的網域帳戶。

## <a name="whats-new-for-managed-service-accounts-in-windows-server-2012-and-windows-8"></a><a name="versions"></a>Windows Server 2012 和 Windows 8 中受管理的服務帳戶的新功能
以下說明 Windows Server 2012 和 Windows 8 中 MSA 的功能變更。

### <a name="group-managed-service-accounts"></a>群組受管理的服務帳戶
在網域中為伺服器設定網域帳戶時，用戶端電腦可以驗證並連接至該服務。 之前只有兩種帳戶類型在不需要密碼管理的情況下提供身分識別。 但是這些帳戶類型有所限制：

-   電腦帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

-   受管理的服務帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

這些帳戶無法跨多個系統進行共用。 因此，您必須定期維護每個系統上的每個服務的帳戶，以防不必要的密碼過期狀況。

**這個變更增加了什麼價值？**

群組受管理的服務帳戶可解決這個問題，因為帳戶密碼是由 Windows Server 2012 網域控制站所管理，而且可以由多個 Windows Server 2012 系統抓取。 這允許 Windows 處理這些帳戶的密碼管理，將服務帳戶的管理負擔降至最低。

**有哪些不同？**

在執行 Windows Server 2012 或 Windows 8 的電腦上，您可以透過服務控制管理員來建立和管理群組 MSA，如此一來，就可以從一部伺服器管理多個服務實例，例如透過伺服器陣列進行部署。 您用來管理受管理的服務帳戶的工具和公用程式，例如 IIS 應用程式集區管理員，可以搭配群組受管理的服務帳戶使用。 網域管理員可以將服務管理委派給服務管理員，以管理受管理的服務帳戶或群組受管理的服務帳戶的整個週期。 現有的用戶端電腦將能夠向任何這種服務進行驗證，而不需要知道它們向哪個服務執行個體進行驗證。

### <a name="removed-or-deprecated-functionality"></a><a name="interoperability"></a>已移除或過時的功能
對於 Windows Server 2012，Windows PowerShell Cmdlet 預設為管理群組受管理的服務帳戶，而不是伺服器受管理的服務帳戶。

## <a name="additional-references"></a>其他參考

-   [群組受管理的服務帳戶概觀](group-managed-service-accounts-overview.md) \(機器翻譯\)

-   [Active Directory Domain Services 概觀](active-directory-domain-services-overview.md)

-   [受管理的服務帳戶了解、實作、最佳做法以及疑難排解](https://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


