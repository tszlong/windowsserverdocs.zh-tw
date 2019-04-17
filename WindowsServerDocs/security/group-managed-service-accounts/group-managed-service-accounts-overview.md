---
title: 群組多媒體受管理的服務帳號概觀
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
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>群組多媒體受管理的服務帳號概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題中的 IT 專業人員藉由描述實用的應用程式介紹群組管理服務 Account 變更 Microsoft 實作及硬體與軟體需求。


## <a name="BKMK_OVER"></a>描述的功能
獨立管理服務帳號，這是管理的在 Windows Server 2008 R2 和 Windows 7 中，都是管理的受管理的網域帳號，可提供自動密碼管理簡化的 SPN 管理，包括的其他系統管理員委派。

群組管理服務 Account 提供相同網域中的功能，但也到多部伺服器擴充功能。 連接到服務，伺服器發電廠，例如網路負載平衡裝載時支援互加好友的驗證，驗證通訊協定要求服務的所有執行個體使用相同的原則。 管理服務 Account 的群組做為服務原則，當 Windows 作業系統管理帳號，而非只依賴上系統管理員，管理密碼的密碼。

Microsoft 金鑰 Distribution 服務 \(kdssvc.dll\) 提供機制安全地取得最新的按鍵或按鍵識別碼的 Active Directory 帳號特定的按鍵。 金鑰 Distribution 服務共用用來建立按鍵 account 的密碼。 這些按鍵會定期變更。 適用於群組管理服務 Account 網域控制站計算鍵來散發服務，此外提供給其他屬性管理服務 Account 群組的密碼。  成員主機可以取得連絡網域控制站的目前與先前密碼值。

## <a name="BKMK_APP"></a>實用的應用程式
群組管理服務帳號提供單一身分方案伺服器發電廠，或在之後的網路負載平衡系統上執行的服務。 藉由提供群組 MSA 方案，可以設定服務的新群組 MSA 主體和密碼管理由 Windows。

使用一組管理服務 Account、 服務或服務的系統管理員不需要管理密碼同步之間服務執行個體。 群組管理服務 Account 支援的主機保留離線延伸的期間和管理成員主機服務的所有執行個體。 這表示您可以將支援單一的身分，而不需要知道的服務連接到執行個體 client 現有的電腦可以驗證的伺服器發電廠部署。

容錯 gMSAs 不支援。 不過，上方叢集服務執行的服務，可以使用 gMSA 或 sMSA 如果他們的 Windows 服務，應用程式集區排定的工作，或原生支援 gMSA 或 sMSA。

## <a name="BKMK_SOFT"></a>軟體需求

64\ 位元架構，才能執行的 Windows PowerShell 命令可用來管理管理服務帳號群組。

受管理的服務 account 是仰賴 Kerberos 支援加密類型。使用的 Kerberos DC 建立伺服器的 client 電腦進行驗證時 Kerberos 服務票證受加密俠和伺服器支援。 DC 使用 account 的 msDS\-SupportedEncryptionTypes 屬性來判斷加密伺服器支援，如果有任何屬性，假設 client 的電腦不支援較加密類型。 如果主機設定為無法支援 RC4，將永遠無法驗證。 基於這個原因，好一段應該永遠明確設定 MSAs。

> [!NOTE]
> 開始使用 Windows Server 2008 R2、DES 預設停用。 如需支援的加密類型的詳細資訊，請查看[變更 Kerberos 驗證](https://technet.microsoft.com/library/dd560670(WS.10).aspx)。

群組管理服務帳號並非適用於 Windows Server 2012 之前的 Windows 作業系統。

## <a name="server-manager-information"></a>伺服器管理員資訊
實作 MSA 和群組 MSA 使用伺服器管理員或 Install\-WindowsFeature cmdlet 所需的任何設定步驟。

## <a name="BKMK_LINKS"></a>也了
下表提供額外的資源管理服務帳號，並群組管理服務帳號相關的連結。

|內容類型|資訊尋找參考資料|
|--------|-------|
|**Product 評估**|[適用於帳號受管理的服務的新功能](what-s-new-for-managed-service-accounts.md)<br /><br />[管理的服務帳號適用於 Windows 7 和 Windows Server 2008 R2 的文件](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[服務帳號 Step\ by\ 步驟指南](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**規劃**|未提供|
|**部署**|未提供|
|**作業**|[管理服務帳號 Active Directory 中](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**疑難排解**|未提供|
|**評估**|[開始使用群組管理帳號服務](getting-started-with-group-managed-service-accounts.md)|
|**工具和設定**|[管理服務帳號在 Active Directory Domain Services](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**社群資源**|[了解受管理的服務帳號:、 實作最佳做法，和疑難排解](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**相關的技術**|[Active Directory Domain Services 概觀](active-directory-domain-services-overview.md)|


