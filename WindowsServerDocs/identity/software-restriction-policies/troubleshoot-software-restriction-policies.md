---
title: 為軟體限制原則進行疑難排解
description: 從 Windows Server 2008 和 Windows Vista (SRP) 疑難排解軟體限制原則時，瞭解常見問題和解決方案。
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: b9c96f43ebbcc41a210357b2006d2b153a69545a
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113474"
---
# <a name="troubleshoot-software-restriction-policies"></a>為軟體限制原則進行疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明從 Windows Server 2008 和 Windows Vista 開始 (SRP) 疑難排解軟體限制原則時的常見問題和解決方案。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些都與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上進行設定。 如需 SRP 的詳細資訊，請參閱 [軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，您可以使用 Windows AppLocker 來取代或搭配 SRP 使用，以取得應用程式控制策略的一部分。

### <a name="windows-cannot-open-a-program"></a>Windows 無法開啟程式
使用者會收到一則訊息，指出「Windows 無法開啟此程式，因為它已被軟體限制原則阻止。 如需詳細資訊，請開啟事件檢視器或洽詢您的系統管理員。」 或者，在命令列中，訊息指出「系統無法執行指定的程式。」

**原因：** 預設的安全性層級 (或建立的規則) ，因此軟體程式會設定為 [不 **允許**]，因此將不會啟動。

**解決方案：** 查看事件記錄檔以取得訊息的深入說明。 事件記錄檔訊息指出哪些軟體程式設定為 [不 **允許** ]，以及要套用到該程式的規則。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改過的軟體限制原則不會生效
**原因：** 透過群組原則在網域中指定的軟體限制原則，會覆寫在本機設定的任何原則設定。 這可能表示網域中有覆寫原則設定的原則設定。

**原因：** 群組原則可能不會重新整理其原則設定。 群組原則定期套用原則設定的變更;因此，在目錄中所做的原則變更可能尚未重新整理。

**解決 方案：**

1.  您用來修改網路軟體限制原則的電腦必須能夠與網域控制站連線。 確定電腦可以與網域控制站連線。

2.  登出網路，然後再次登入網路，以重新整理原則。 如果任何原則是透過群組原則套用的，則重新登入將會重新整理這些原則。

3.  您可以使用命令列公用程式 gpupdate 或登出，然後重新登入電腦，以重新整理原則設定。 為了獲得最佳結果，請執行 gpupdate，然後登出再重新登入您的電腦。 一般而言，每隔90分鐘就會在工作站或伺服器上重新整理安全性設定，並在網域控制站上每隔5分鐘重新整理一次。 而且不管有無變更皆每隔 16 小時更新一次。 這些是可設定的設定，因此每個網域中的重新整理間隔可能會不同。

4.  檢查適用的原則。 檢查網域層級原則是否 **沒有覆寫** 設定。

5.  透過群組原則在網域中指定的軟體限制原則，會覆寫在本機設定的任何原則。 使用 Gpresult 命令列工具來判斷原則的淨影響。 這可能表示網域中有覆寫您本機設定的原則。

6.  如果 SRP 和 AppLocker 原則設定位於相同的 GPO 中，AppLocker 設定將會優先于 Windows 7、Windows Server 2008 R2 和更新版本。 建議您將 SRP 和 AppLocker 原則設定放在不同的 Gpo 中。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>透過 SRP 新增規則之後，您就無法登入電腦
**原因：** 當電腦啟動時，您的電腦會存取許多程式和檔案。 您可能會不小心將這些程式或檔案中的其中一個設定為不 **允許**。 因為電腦無法存取程式或檔案，所以無法正常啟動。

**解決方案：** 以安全模式啟動電腦，以本機系統管理員身分登入，然後變更軟體限制原則以允許程式或檔案執行。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的原則設定未套用至特定的副檔名
**原因：** 檔案名延伸不在支援的檔案類型清單中。

**解決方案：** 將副檔名新增至 SRP 所支援的檔案類型清單。

軟體限制原則解決了限制未知或不受信任的程式碼的問題。 軟體限制原則是安全性設定，用來識別軟體並控制其在本機電腦、網域或 OU 中執行的能力，並可透過 GPO 來執行。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>預設規則未如預期般限制
**原因：** 依特定順序套用的規則，可能會導致特定規則覆寫預設規則。 SRP 依下列順序套用規則 (最特定的一般) ：

1.  雜湊規則

2.  憑證規則

3.  路徑規則

4.  網際網路區域規則

5.  預設規則

**解決方案：** 評估限制應用程式的規則，並在適當的情況下移除所有預設規則。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>無法探索要套用的限制
**原因：** 未預期的行為沒有明顯的原因，而且 GPO 重新整理尚未解決問題，因此需要進一步調查。

**解決 方案：**

1.  調查系統事件記錄檔，並根據「軟體限制原則」的來源進行篩選。 這些專案會明確陳述針對每個應用程式所執行的規則。

2.  啟用 advanced 記錄。 如需詳細資訊，請參閱 [決定軟體限制原則的 Allow-Deny 清單和應用程式清查](software-restriction-policies.md) 。


