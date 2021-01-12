---
title: What's New for Managed Service Accounts
description: 瞭解 Windows Server 2012 和 Windows 8 中群組受管理的服務帳戶，以瞭解受管理的服務帳戶的功能變更。
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: d7ea925975254533ff7b9c6591ddde4a884a79ae
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112964"
---
# <a name="what39s-new-for-managed-service-accounts"></a>受管理的服務帳戶&#39;的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明受管理的服務帳戶的功能變更，以及 Windows Server 2012 和 Windows 8 中 (gMSA) 的「群組受管理的服務帳戶」簡介。

受管理的服務帳戶主要用於提供服務與工作，例如 Windows 服務和 IIS 應用程式集區，以共用自己的網域帳戶，同時讓系統管理員不需手動管理這些帳戶的密碼。 這是一個提供自動密碼管理的受管理的網域帳戶。

## <a name="whats-new-for-managed-service-accounts-in-windows-server-2012-and-windows-8"></a><a name="versions"></a>Windows Server 2012 和 Windows 8 中受管理的服務帳戶的新功能
以下說明 Windows Server 2012 和 Windows 8 中的 MSA 所做的功能變更。

### <a name="group-managed-service-accounts"></a>群組受管理的服務帳戶
在網域中為伺服器設定網域帳戶時，用戶端電腦可以驗證並連接至該服務。 之前只有兩種帳戶類型在不需要密碼管理的情況下提供身分識別。 但是這些帳戶類型有所限制：

-   電腦帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

-   受管理的服務帳戶限制為一個網域伺服器，而且密碼受到電腦管理。

這些帳戶無法跨多個系統進行共用。 因此，您必須定期維護每個系統上的每個服務的帳戶，以防不必要的密碼過期狀況。

**這個變更增加了什麼價值？**

群組受管理的服務帳戶可解決此問題，因為帳戶密碼是由 Windows Server 2012 網域控制站所管理，而且可以由多個 Windows Server 2012 系統抓取。 這允許 Windows 處理這些帳戶的密碼管理，將服務帳戶的管理負擔降至最低。

**有哪些不同？**

在執行 Windows Server 2012 或 Windows 8 的電腦上，可以透過服務控制管理員來建立和管理群組 MSA，如此一來，就可以從一部伺服器管理服務的多個實例，例如透過伺服器陣列部署。 您用來管理受管理的服務帳戶的工具和公用程式，例如 IIS 應用程式集區管理員，可以搭配群組受管理的服務帳戶使用。 網域管理員可以將服務管理委派給服務管理員，以管理受管理的服務帳戶或群組受管理的服務帳戶的整個週期。 現有的用戶端電腦將能夠向任何這種服務進行驗證，而不需要知道它們向哪個服務執行個體進行驗證。

### <a name="removed-or-deprecated-functionality"></a><a name="interoperability"></a>已移除或過時的功能
若為 Windows Server 2012，Windows PowerShell Cmdlet 預設為管理群組受管理的服務帳戶，而不是伺服器受管理的服務帳戶。

## <a name="additional-references"></a>其他參考資料

-   [群組受管理的服務帳戶總覽](group-managed-service-accounts-overview.md)

-   [Active Directory 網域服務概觀](active-directory-domain-services-overview.md)

-   [受管理的服務帳戶了解、實作、最佳做法以及疑難排解](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/managed-service-accounts-understanding-implementing-best/ba-p/397009)


