---
title: Software Restriction Policies
description: 深入瞭解 Windows Server 2012 中的 (SRP) 的軟體限制原則和 Windows 8，並尋找有關從 Windows Server 2003 開始之 SRP 的技術資訊連結。
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 3d98c52719efeec447d9bdedc4fb824c6dbeca4d
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112944"
---
# <a name="software-restriction-policies"></a>Software Restriction Policies

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題適用于 IT 專業人員，說明 Windows Server 2012 和 Windows 8 中 (SRP) 的軟體限制原則，並提供從 Windows Server 2003 開始之 SRP 技術資訊的連結。

如需程式和疑難排解秘訣，請參閱 [管理軟體限制原則](administer-software-restriction-policies.md) 和 [疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)。

## <a name="software-restriction-policies-description"></a><a name="BKMK_OVER"></a>軟體限制原則說明
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 軟體限制原則是 Microsoft 安全性與管理策略的一部分，可以協助企業提高電腦的可靠性、完整性及管理性。

您也可以使用軟體限制原則，來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 軟體限制原則已與 Microsoft Active Directory 和群組原則整合。 您也可以在獨立電腦上建立軟體限制原則。 軟體限制原則是信任原則，這些原則是由系統管理員設定的規範，可以限制未受到完全信任的指令碼和其他程式碼的執行。

您可以透過本機群組原則編輯器的軟體限制原則延伸或 Microsoft Management Console (MMC) 的本機安全性原則嵌入式管理單元中定義這些原則。

如需 SRP 的深入資訊，請參閱[軟體限制原則技術概觀](software-restriction-policies-technical-overview.md)。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
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

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>新功能和變更的功能
對於軟體限制原則的功能沒有任何變更。

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>已移除或過時的功能
對於軟體限制原則沒有任何已移除或過時的功能。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
本機群組原則編輯器的軟體限制原則延伸可以透過 MMC 加以存取。

下列為建立和維護本機電腦上軟體限制原則所需的功能：

-   本機群組原則編輯器

-   Windows Installer

-   Authenticode 和 WinVerifyTrust

如果您設計適用於這些原則的網域部署的呼叫，則除了上述清單之外，還需要下列功能：

-   Active Directory 網域服務

-   群組原則

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>伺服器管理員資訊
軟體限制原則是本機群組原則編輯器的延伸，無法透過伺服器管理員的新增角色及功能進行安裝。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>請參閱
下表提供了解與使用 SRP 相關資源的連結。

|內容類型|參考|
|--------|-------|
|**產品評估**|[使用軟體限制原則鎖定應用程式](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
|**規劃**|[軟體限制原則技術總覽](software-restriction-policies-technical-overview.md) ( Windows Server 2012 ) <p>[軟體限制原則技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10)) (Windows Server 2003)|
|**部署**|沒有可用資源。|
|**作業**| ( Windows Server 2012 ) [管理軟體限制原則](administer-software-restriction-policies.md)<p>[軟體限制原則產品](/previous-versions/windows/it-pro/windows-server-2003/cc779607(v=ws.10)) 說明 (Windows Server 2003) |
|**疑難排解**|疑難排解 Windows Server 2012 ( 的[軟體限制原則](troubleshoot-software-restriction-policies.md)) <p>疑難排解 Windows Server 2003 (的[軟體限制原則](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10))) |
|**Security**|[軟體限制原則的安全威脅與因應對策](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10)) (Windows  Server 2008)<p>[軟體限制原則的威脅和因應措施](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10)) (Windows Server 2008 R2) |
|**工具及設定**|Windows Server 2003) [的軟體限制原則工具和設定](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10)) (|
|**社群資源**|[使用軟體限制原則鎖定應用程式](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
