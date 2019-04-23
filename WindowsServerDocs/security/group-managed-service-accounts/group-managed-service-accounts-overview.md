---
title: Group Managed Service Accounts Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836969"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>適用於：Windows Server （半年通道），Windows Server 2016

本主題適用於 IT 專業人員透過描述實際應用，說明群組受管理的服務帳戶的變更在 Microsoft 的實作中，以及硬體和軟體需求。


## <a name="BKMK_OVER"></a>功能描述
獨立的受控服務帳戶 」 (sMSA) 是受管理的網域帳戶提供自動密碼管理、 簡化的服務主要名稱 (SPN) 管理和委派給其他系統管理員管理的能力。 在 Windows Server 2008 R2 和 Windows 7 中引進了這種類型的受管理的服務帳戶 (MSA)。

群組受控服務帳戶 (gMSA) 提供在網域中相同的功能，但也會透過多部伺服器擴充功能。 連接到伺服器陣列，例如網路負載平衡的解決方案上裝載的服務時支援相互驗證的驗證通訊協定需要服務的所有執行個體使用相同的主體。 使用 gMSA 時做為服務主體，Windows 作業系統會管理而不是依賴系統管理員管理密碼的帳戶的密碼。

Microsoft 金鑰發佈服務\(kdssvc.dll\)提供安全取得最新的金鑰或 Active Directory 帳戶的特定金鑰的金鑰識別碼的機制。 金鑰發佈服務共用一個用來建立帳戶金鑰的密碼。 系統會定期變更這些金鑰。 GMSA 的網域控制站會計算針對金鑰發佈服務，除了 gMSA 的其他屬性所提供的金鑰的密碼。  成員主機可以連絡網域控制站，以取得目前以及前述的密碼值。

## <a name="BKMK_APP"></a>實際的應用程式
Gmsa 為伺服器陣列，或網路負載平衡器後方的系統上執行的服務提供單一身分識別解決方案。 藉由提供 gMSA 解決方案，可以將服務設定為新的 gMSA 主體，以及密碼管理由 Windows。

使用 gMSA，服務或服務系統管理員不需要管理服務執行個體之間的密碼同步處理。 GMSA 支援維持離線的延長的期間，與管理服務的所有執行個體的成員主機的主機。 這表示您可以部署一個支援單一識別的伺服器陣列，現有的用戶端電腦不需知道所連接的服務執行個體，就可以進行驗證。

容錯移轉叢集不支援 gMSA。 不過，在叢集服務上執行的服務如果是 Windows 服務、應用程式集區、排定的工作或原生支援 gMSA 或 sMSA，便可使用 gMSA 或 sMSA。

## <a name="BKMK_SOFT"></a>軟體需求

以 64\-位元架構，才能執行 Windows PowerShell 命令來管理 Gmsa。

受管理的服務帳戶依存於 Kerberos 受支援的加密類型。當用戶端電腦向使用 Kerberos 的伺服器進行驗證，DC 會建立使用 DC 和伺服器都支援的加密來保護的 Kerberos 服務票證。 DC 使用帳戶的 Msds-primary-computer\-SupportedEncryptionTypes 屬性來判斷何種加密伺服器支援，而且如果沒有屬性，它就會假設用戶端電腦不支援更強的加密類型。 如果主機已設定為不支援 RC4，則驗證將一律會失敗。 基於這個原因，AES 應一律針對 MSA 進行明確設定。

> [!NOTE]
> 從 Windows Server 2008 R2 開始，DES 預設為停用。 如需受支援加密類型的詳細資訊，請參閱 [Kerberos 驗證的變更](https://technet.microsoft.com/library/dd560670(WS.10).aspx)。

Gmsa 不適用於 Windows Server 2012 之前的 Windows 作業系統。

## <a name="server-manager-information"></a>伺服器管理員資訊
沒有實作 MSA 和 gMSA 使用伺服器管理員或安裝所需的組態步驟\-WindowsFeature cmdlet。

## <a name="BKMK_LINKS"></a>另請參閱
下表提供與受管理的服務帳戶以及群組受管理的服務帳戶相關的其他資源連結。

|內容類型|參考|
|--------|-------|
|**產品評估**|[什麼是受管理的服務帳戶的新功能](what-s-new-for-managed-service-accounts.md)<br /><br />[受管理的服務帳戶的 Windows 7 和 Windows Server 2008 R2 文件](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[服務帳戶的步驟\-由\-逐步指南](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**規劃**|尚未提供|
|**部署**|尚未提供|
|**操作**|[受管理的 Active Directory 中的服務帳戶](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**疑難排解**|尚未提供|
|**評估**|[開始使用群組受管理的服務帳戶](getting-started-with-group-managed-service-accounts.md)|
|**工具及設定**|[受管理的 Active Directory 網域服務中的服務帳戶](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**社群資源**|[受管理的服務帳戶：了解、 實作、 最佳做法和疑難排解](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**相關技術**|[Active Directory 網域服務概觀](active-directory-domain-services-overview.md)|


