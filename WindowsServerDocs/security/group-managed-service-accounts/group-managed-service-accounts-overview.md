---
title: Group Managed Service Accounts Overview
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4e7f46739dd8def6ffc34c6cc50210c0e6999c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403742"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，透過描述實際應用、Microsoft 的實行變更，以及硬體和軟體需求，介紹群組受管理的服務帳戶。


## <a name="BKMK_OVER"></a>功能描述
獨立受管理的服務帳戶（sMSA）是一個受管理的網域帳戶，可提供自動密碼管理、簡化的服務主體名稱（SPN）管理，以及將管理委派給其他系統管理員的能力。 這種類型的受管理的服務帳戶 (MSA) 是在 Windows Server 2008 R2 和 Windows 7 中引進。

群組受管理的服務帳戶（gMSA）會在網域內提供相同的功能，但也會在多部伺服器上擴充該功能。 當連接到裝載于伺服器陣列的服務（例如網路負載平衡解決方案）時，支援相互驗證的驗證通訊協定會要求服務的所有實例都使用相同的主體。 當使用 gMSA 做為服務主體時，Windows 作業系統會管理帳戶的密碼，而不是依賴系統管理員來管理密碼。

Microsoft 金鑰發佈服務 \(kdssvc @ no__t-1 提供機制，以 Active Directory 帳戶的金鑰識別碼安全地取得最新的金鑰或特定金鑰。 金鑰發佈服務共用一個用來建立帳戶金鑰的密碼。 系統會定期變更這些金鑰。 針對 gMSA，網域控制站除了 gMSA 的其他屬性之外，還會計算金鑰發佈服務所提供之金鑰的密碼。  成員主機可以透過聯繫網域控制站，取得目前和先前的密碼值。

## <a name="BKMK_APP"></a>實際應用
Gmsa 為在伺服器陣列上執行的服務，或在網路 Load Balancer 後方的系統上，提供單一身分識別解決方案。 藉由提供 gMSA 解決方案，可以針對新的 gMSA 主體設定服務，而密碼管理則是由 Windows 處理。

使用 gMSA，服務或服務系統管理員不需要管理服務實例之間的密碼同步處理。 GMSA 支援長時間保持離線狀態的主機，以及服務所有實例的成員主機管理。 這表示您可以部署一個支援單一識別的伺服器陣列，現有的用戶端電腦不需知道所連接的服務執行個體，就可以進行驗證。

容錯移轉叢集不支援 gMSA。 不過，在叢集服務上執行的服務如果是 Windows 服務、應用程式集區、排定的工作或原生支援 gMSA 或 sMSA，便可使用 gMSA 或 sMSA。

## <a name="BKMK_SOFT"></a>軟體需求

執行用來管理 Gmsa 的 Windows PowerShell 命令時，需要 64 @ no__t 0bit 架構。

受管理的服務帳戶依存於 Kerberos 受支援的加密類型。當用戶端電腦向使用 Kerberos 的伺服器進行驗證，DC 會建立使用 DC 和伺服器都支援的加密來保護的 Kerberos 服務票證。 DC 會使用帳戶的 [no__t-0SupportedEncryptionTypes] 屬性來判斷伺服器支援的加密，如果沒有屬性，則會假設用戶端電腦不支援更強的加密類型。 如果主機設定為不支援 RC4，則驗證一律會失敗。 基於這個原因，AES 應一律針對 MSA 進行明確設定。

> [!NOTE]
> 從 Windows Server 2008 R2 開始，DES 預設為停用。 如需受支援加密類型的詳細資訊，請參閱 [Kerberos 驗證的變更](https://technet.microsoft.com/library/dd560670(WS.10).aspx)。

Gmsa 不適用於 Windows Server 2012 之前的 Windows 作業系統。

## <a name="server-manager-information"></a>伺服器管理員資訊
使用伺服器管理員或 Install @ no__t-0WindowsFeature Cmdlet 來執行 MSA 和 gMSA 時，不需要進行任何設定步驟。

## <a name="BKMK_LINKS"></a>另請參閱
下表提供與受管理的服務帳戶以及群組受管理的服務帳戶相關的其他資源連結。

|內容類型|參考|
|--------|-------|
|**產品評估**|[受管理的服務帳戶的新功能](what-s-new-for-managed-service-accounts.md)<br /><br />[Windows 7 和 Windows Server 2008 R2 的受管理的服務帳戶檔](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[服務帳戶步驟 @ no__t-1by @ no__t-2Step 指南](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**規劃**|尚未提供|
|**部署**|尚未提供|
|**操作**|[Active Directory 中的受管理的服務帳戶](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**疑難排解**|尚未提供|
|**求**|[開始使用群組受管理的服務帳戶](getting-started-with-group-managed-service-accounts.md)|
|**工具及設定**|[Active Directory Domain Services 中的受管理的服務帳戶](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**社群資源**|@no__t 0Managed 服務帳戶：瞭解、執行、最佳作法和疑難排解 @ no__t-0|
|**相關技術**|[Active Directory Domain Services 概觀](active-directory-domain-services-overview.md)|


