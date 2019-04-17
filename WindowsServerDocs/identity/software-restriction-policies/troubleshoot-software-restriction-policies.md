---
title: "疑難排解軟體限制原則"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>疑難排解軟體限制原則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述常見問題和他們方案時進行疑難排解與 Windows Server 2008 和 Windows Vista 軟體限制原則 (SRP) 開始。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是群組原則的功能辨識中加入網域的電腦上執行的軟體程式，以及控制執行這些程式的能力。 您可以使用軟體限制原則來建立高度限制的電腦，您可讓只專門辨識應用程式執行設定。 這些整合在一起 Microsoft Active Directory Domain Services 及群組原則，但是您也可以在獨立的電腦上設定。 如需 SRP 的詳細資訊，請查看[軟體限制原則](software-restriction-policies.md)。

開始使用 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用於而不是或 SRP 搭配您的應用程式控制項策略的一部分。

### <a name="windows-cannot-open-a-program"></a>Windows 無法開放程式
使用者收到訊息，指出「因為它軟體限制原則，不讓 Windows 無法開放此程式。 如需詳細資訊，開放事件檢視器或連絡您的系統管理員」 或者，在命令列中，郵件標示為「系統無法執行指定的計畫」。

**原因：**預設的安全性等級（或規則），以設定的軟體程式建立**不允許]**，並不會因此開始]。

**方案：**的訊息深入描述事件木頭中的外觀。 事件登入訊息指出哪些軟體程式設定為**不允許]**並哪些規則適用於「計畫。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>已修改的軟體限制原則不會拍攝效果
**原因：**軟體限制原則網域透過群組原則中所指定覆寫本機上設定的原則設定。 這可能表示就會覆寫您的原則設定的網域原則設定。

**原因：**群組原則可能無法更新的原則設定。 群組原則變更套原則設定定期;因此，則可能的 directory 中所做的變更原則不尚未已經更新。

**方案：**

1.  必須可以連絡網域控制站在電腦，您可以修改軟體限制原則的網路。 請確定電腦，可以連絡網域控制站。

2.  重新整理原則網路使用登入，然後再登入網路一次。 如果任何原則透過群組原則、登入步驟可重新整理那些原則。

3.  您可以重新整理原則設定的命令列的公用程式 gpupdate 或登出，然後再登入您的電腦。 最好執行 gpupdate，再從登出並重新登入您的電腦。 一般而言的安全性設定的重新整理工作站或伺服器每 90 分鐘的網域控制站在每個 5 分鐘時間。 設定也會重新整理每個 16 小時的時間，或有任何變更。 這些是可設定的設定，因此每個網域中重新整理間隔可能會不同。

4.  檢查適用的原則。 檢查網域層級原則適用於**不覆寫**設定。

5.  透過群組原則網域中指定的軟體限制原則覆寫本機上設定的任何原則。 使用 Gpresult 命令列工具來判斷效果原則的網路功能。 這可能表示就會覆寫您的本機設定的網域原則。

6.  如果在同一個 GPO SRP 和 AppLocker 原則設定，AppLocker 設定將會優先在 Windows 7、Windows Server 2008 R2 上及更新版本。 建議您在不同 Gpo 放 SRP 和 AppLocker 原則設定。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>新增透過 SRP 規則之後, 您無法登入您的電腦
**原因：**您的電腦存取許多程式及檔案時，它會開始。 您可能會無意設定其中一個程式或檔案，以**不允許]**。 因為程式或檔案，也無法存取電腦，它無法運作開始。

**方案：**電腦處於安全模式本機系統管理員身分登入，然後變更 [允許的程式或檔案執行的軟體限制原則。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的原則設定不會套用到特定的檔案名稱擴充功能
**原因：**副檔名不在清單中支援的檔案類型。

**方案：**副檔名新增到清單的 SRP 支援的檔案類型。

軟體限制原則地址規範未知或受信任的程式碼的問題。 軟體限制原則是以找出軟體，以及控制能力的本機電腦，在 [網站、網域或組織單位上執行的安全性設定，可以透過 GPO 實作。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>不會如預期般限制預設規則
**原因：**的套用順序特定，可能會導致預設規則覆寫特定規則。 SRP 適下列順序（最特定一般）中的規則：

1.  Hash 規則

2.  憑證規則

3.  路徑規則

4.  網際網路區規則

5.  預設規則

**方案：**評估規則限制該應用程式，如果適用，移除預設規則以外的所有。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>無法探索套用的限制
**原因：**未預期的行為，不能發揮原因，以便所需的調查 GPO 重新整理已不解決的問題。

**方案：**

1.  調查系統事件登入篩選來源的「軟體限制原則」。 項目明確陳述實作每個應用程式規則。

2.  支援進階登入。 查看[判斷允許拒絕清單和應用程式清單軟體限制原則的](software-restriction-policies.md)如需詳細資訊。


