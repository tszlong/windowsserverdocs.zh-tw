---
title: "軟體限制原則"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>軟體限制原則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題適用於 IT 專業人員描述軟體限制原則 (SRP) 在 Windows Server 2012 和 Windows 8，並提供與 Windows Server 2003 SRP 開頭技術的資訊連結。

程序與疑難排解提示，請查看[管理軟體限制原則](administer-software-restriction-policies.md)和[疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)。

## <a name="BKMK_OVER"></a>軟體限制原則描述
軟體限制原則 (SRP) 是群組原則的功能辨識中加入網域的電腦上執行的軟體程式，以及控制執行這些程式的能力。 軟體限制原則的 Microsoft 安全性與管理策略協助企業增加可靠性、 完整性及管理其電腦的一部分。

您也可以使用軟體限制原則來建立高度限制的電腦，您可讓只專門辨識應用程式執行設定。 整合軟體限制原則與 Microsoft Active Directory 群組原則。 您也可以在獨立的電腦上建立的軟體限制原則。 軟體限制原則是信任原則的系統管理員的身分為限制指令碼和其他驗證碼不會完全無法執行受信任的規範。

您可以定義透過本機群組原則編輯器] 或 [本機安全性原則嵌入式管理單元軟體限制原則延伸這些原則 Microsoft Management Console (MMC)。

適用於 SRP 的深入資訊，請查看[軟體限制原則技術概觀](software-restriction-policies-technical-overview.md)。

## <a name="BKMK_APP"></a>實用的應用程式
系統管理員可以使用軟體限制原則下列任務：

-   定義信任的程式碼

-   設計彈性的群組原則的規範指令碼，可執行檔和 ActiveX 控制項

作業系統和應用程式 （例如指令碼處理的應用程式） 符合使用軟體限制原則，會執行軟體限制原則。

具體而言，系統管理員可以使用軟體限制原則下列目的：

-   指定戶端可以執行的軟體 （可執行檔）

-   防止共用的電腦上執行特定程式

-   指定誰可以戶端新增受信任的發行者

-   設定的軟體限制原則 （指定原則會影響所有使用者是否或戶端上的使用者子集） 範圍

-   防止可執行檔的本機電腦，單位 （組織單位）、 網站或網域上執行。 您不具有惡意的使用者使用軟體限制原則潛在問題時，這是適用於案例。

## <a name="BKMK_NEW"></a>新功能和變更功能
功能軟體限制原則有任何變更。

## <a name="BKMK_DEP"></a>移除或已取代功能
還有軟體限制原則不移除或已被取代功能。

## <a name="BKMK_SOFT"></a>軟體需求
可透過 MMC 存取軟體限制原則延伸到本機群組原則編輯器。

建立及維護本機電腦上的軟體限制原則需要下列功能：

-   本機群組原則編輯器

-   Windows 安裝程式

-   驗證碼與 WinVerifyTrust

如果您的設計呼叫網域部署這些原則，除了上述清單中，下列功能都需要：

-   Active Directory Domain Services

-   群組原則

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
軟體限制原則哪些擴充功能的本機群組原則編輯器是，透過在伺服器管理員、 新增角色與功能尚未安裝。

## <a name="BKMK_LINKS"></a>也了
下表中提供了解與使用 SRP 中相關資源的連結。

|內容類型|資訊尋找參考資料|
|--------|-------|
|**Product 評估**|[應用程式鎖定使用軟體限制原則](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**規劃**|[軟體限制原則技術概觀](software-restriction-policies-technical-overview.md)(Windows Server 2012)<br /><br />[軟體限制原則技術參考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)(Windows Server 2003)|
|**部署**|不是資源。|
|**作業**|[管理軟體限制原則](administer-software-restriction-policies.md)(Windows Server 2012)<br /><br />[軟體限制原則 Product 協助](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx)(Windows Server 2003)|
|**疑難排解**|[疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)(Windows Server 2012)<br /><br />[疑難排解軟體限制原則](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)(Windows Server 2003)|
|**安全性**|[威脅和措施的軟體限制原則](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)(Windows Server 2008)<br /><br />[威脅和措施的軟體限制原則](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)(Windows Server 2008 R2)|
|**工具和設定**|[軟體限制原則工具並設定](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)(Windows Server 2003)|
|**社群資源**|[應用程式鎖定使用軟體限制原則](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



