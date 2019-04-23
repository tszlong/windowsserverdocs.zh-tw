---
title: 為軟體限制原則進行疑難排解
description: Windows Server 安全性
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872379"
---
# <a name="troubleshoot-software-restriction-policies"></a>為軟體限制原則進行疑難排解

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

疑難排解軟體限制原則 (SRP) 從 Windows Server 2008 和 Windows Vista 時，本主題會說明常見的問題和解決方案。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory 網域服務和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的詳細資訊，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用於取代或搭配 SRP 應用程式控制策略的一部分。

### <a name="windows-cannot-open-a-program"></a>Windows 無法開啟程式
使用者會收到訊息指出 「 因為它已經受軟體限制原則，Windows 無法開啟此程式。 如需詳細資訊，開啟事件檢視器或連絡您的系統管理員。 」 或者，您也可以在命令列中，訊息會指出 「 系統無法執行指定的程式 」。

**原因：** 預設安全性等級 （或規則） 已建立軟體程式已設定為**不允許**，並因此將無法啟動。

**解決方案：** 如需深入的說明訊息的事件記錄檔中尋找。 事件記錄檔訊息，指出哪一個軟體程式會設定為**不允許**和哪些規則會套用至該程式。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改過的軟體限制原則未生效
**原因：** 透過群組原則的網域中所指定的軟體限制原則覆寫本機設定任何原則設定。 這可能表示會有的網域，會覆寫您的原則設定的原則設定。

**原因：** 群組原則可能會不有重新整理它的原則設定。 群組原則變更套用至原則設定會定期;因此，很可能，在目錄中所做的原則變更不尚未已經重新整理。

**解決方案：**

1.  您可以在其修改網路的軟體限制原則的電腦必須能夠連絡網域控制站。 請確定電腦可以連絡網域控制站。

2.  重新整理原則，方法是登上網路，然後再登入網路一次。 如果透過群組原則會套用任何原則，登入將會重新整理這些原則。

3.  您可以更新原則設定，使用命令列公用程式 gpupdate 或登出，然後再登入您的電腦上一步。 為了獲得最佳結果，執行 gpupdate，和從然後登出並再次登入您的電腦。 一般而言，每隔 90 分鐘在工作站或伺服器和網域控制站上每隔 5 分鐘會重新整理的安全性設定。 不管是否有變更，每 16 小時也會重新整理這些設定。 這些是可設定的設定，因此每個網域中，重新整理間隔可能會不同。

4.  套用哪項原則檢查。 檢查網域層級原則**不可強制覆蓋**設定。

5.  透過群組原則的網域中所指定的軟體限制原則覆寫本機設定的任何原則。 您可以使用 Gpresult 命令列工具，判斷原則的淨效應是什麼。 這可能表示已從網域，會覆寫您的本機設定的原則。

6.  在同一個 GPO 中 SRP 與 AppLocker 原則設定時，AppLocker 設定將會優先在 Windows 7、windows Server 2008 R2 和更新版本。 建議將置於不同的 Gpo 中 SRP 與 AppLocker 原則設定。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>在新增之後透過 SRP 的規則，您無法登入您的電腦
**原因：** 啟動時，您的電腦存取許多程式和檔案。 您可能不小心設定其中一個程式或檔案**不允許**。 電腦無法存取檔案的程式，因為它無法正常啟動。

**解決方案：** 以安全模式啟動電腦，以本機系統管理員身分登入，然後變更軟體限制原則來允許執行檔的程式。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的原則設定不會套用到特定的副檔名
**原因：** 檔名的副檔名不是支援的檔案類型清單。

**解決方案：** 將 SRP 所支援的檔案類型清單中的檔名的副檔名。

軟體限制原則處理的控管未知或未受信任的程式碼的問題。 軟體限制原則是安全性設定，以識別軟體，並控制其站台、 網域或 OU 中的本機電腦上執行的能力，並且可以利用 GPO 來實作。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>預設規則不會限制如預期般運作
**原因：** 這可能會導致特定規則所覆寫預設規則以特定順序套用的規則。 SRP 適用於以下列順序 （最特定到一般） 的規則：

1.  雜湊規則

2.  憑證規則

3.  路徑規則

4.  網際網路區域規則

5.  預設規則

**解決方案：** 評估規則限制的應用程式，而且如果適當的話，移除預設規則以外的所有。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>無法探索套用的限制
**原因：** 未預期的行為、 無明顯原因，而且 GPO 重新整理已不解決此問題，因此需要進一步調查。

**解決方案：**

1.  調查系統事件記錄檔篩選來源的 「 軟體限制原則 」。 項目明確指定哪一個規則會針對每個應用程式實作。

2.  啟用進階記錄功能。 請參閱[判斷允許拒絕清單和軟體限制原則的應用程式清查](software-restriction-policies.md)如需詳細資訊。


