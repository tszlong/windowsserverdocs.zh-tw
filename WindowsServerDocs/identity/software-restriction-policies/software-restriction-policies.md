---
title: 軟體限制原則
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875819"
---
# <a name="software-restriction-policies"></a>軟體限制原則

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題適用於 IT 專業人員說明軟體限制原則 (SRP) 設定 Windows Server 2012 和 Windows 8 中，並提供從 Windows Server 2003 開始的 SRP 相關技術資訊的連結。

程序及疑難排解秘訣，請參閱[管理軟體限制原則](administer-software-restriction-policies.md)並[疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)。

## <a name="BKMK_OVER"></a>軟體限制原則說明
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 軟體限制原則是 Microsoft 安全性與管理策略的一部分，可以協助企業提高電腦的可靠性、完整性及管理性。

您也可以使用軟體限制原則，來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 軟體限制原則已與 Microsoft Active Directory 和群組原則整合。 您也可以在獨立電腦上建立軟體限制原則。 軟體限制原則是信任原則，這些原則是由系統管理員設定的規範，可以限制未受到完全信任的指令碼和其他程式碼的執行。

您可以透過本機群組原則編輯器的軟體限制原則延伸或 Microsoft Management Console (MMC) 的本機安全性原則嵌入式管理單元中定義這些原則。

如需 SRP 的深入資訊，請參閱 [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md)。

## <a name="BKMK_APP"></a>實際的應用程式
系統管理員可以針對下列工作使用軟體限制原則：

-   定義什麼是受信任的程式碼

-   針對規範指令碼、可執行檔，以及 ActiveX 控制項設計彈性的群組原則。

軟體限制原則是透過遵循軟體限制原則的作業系統和應用程式 (例如，指令碼應用程式) 強制執行的。

具體而言，系統管理員可以基於下列目的使用軟體限制原則：

-   指定要在用戶端上執行哪一個軟體 (可執行檔)

-   防止使用者在共用電腦上執行特定的程式

-   指定誰可以將受信任的發行者新增到用戶端

-   設定軟體限制原則的範圍 (指定原則是否會影響用戶端上的所有使用者或使用者子集)

-   防止可執行檔在本機電腦、組織單位 (OU)、站台或網域上執行。 當您未使用軟體限制原則來處理惡意使用者的潛在問題時，這個狀況便適用此做法。

## <a name="BKMK_NEW"></a>新功能和變更
對於軟體限制原則的功能沒有任何變更。

## <a name="BKMK_DEP"></a>已移除或過時的功能
對於軟體限制原則沒有任何已移除或過時的功能。

## <a name="BKMK_SOFT"></a>軟體需求
本機群組原則編輯器的軟體限制原則延伸可以透過 MMC 加以存取。

下列為建立和維護本機電腦上軟體限制原則所需的功能：

-   本機群組原則編輯器

-   Windows Installer

-   Authenticode 和 WinVerifyTrust

如果您設計適用於這些原則的網域部署的呼叫，則除了上述清單之外，還需要下列功能：

-   Active Directory Domain Services

-   群組原則

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
軟體限制原則是本機群組原則編輯器的延伸，無法透過伺服器管理員的新增角色及功能進行安裝。

## <a name="BKMK_LINKS"></a>另請參閱
下表提供了解與使用 SRP 相關資源的連結。

|內容類型|參考|
|--------|-------|
|**產品評估**|[使用軟體限制原則的應用程式鎖定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**規劃**|[軟體限制原則技術概觀](software-restriction-policies-technical-overview.md)(Windows Server 2012)<br /><br />[軟體限制原則技術參考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**部署**|沒有可用資源。|
|**操作**|[管理軟體限制原則](administer-software-restriction-policies.md)(Windows Server 2012)<br /><br />[軟體限制原則產品說明](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**疑難排解**|[疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)(Windows Server 2012)<br /><br />[軟體限制原則疑難排解](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**安全性**|[軟體限制原則的安全威脅與因應對策](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows  Server 2008)<br /><br />[軟體限制原則的安全威脅與因應對策](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**工具及設定**|[軟體限制原則工具及設定](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**社群資源**|[使用軟體限制原則的應用程式鎖定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



