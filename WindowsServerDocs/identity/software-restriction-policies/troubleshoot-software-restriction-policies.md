---
title: 為軟體限制原則進行疑難排解
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c6b3a475f21925b506d073bd3618d78e2ee0c1d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819721"
---
# <a name="troubleshoot-software-restriction-policies"></a>為軟體限制原則進行疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明從 Windows Server 2008 和 Windows Vista 開始針對軟體限制原則（SRP）進行疑難排解時，常見的問題及其解決方案。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的詳細資訊，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用來取代或與 SRP 搭配使用，以進行部分的應用程式控制策略。

### <a name="windows-cannot-open-a-program"></a>Windows 無法開啟程式
使用者會收到一則訊息，指出「Windows 無法開啟此程式，因為軟體限制原則已防止它。 如需詳細資訊，請開啟事件檢視器或聯絡您的系統管理員。」 或者，在命令列上，顯示「系統無法執行指定的程式」的訊息。

**原因：** 預設的安全性層級（或規則）已建立，讓軟體程式設定為不**允許**，因此不會啟動。

**解決方案：** 查看事件記錄檔，以取得訊息的深入描述。 事件記錄檔訊息會指出哪些軟體程式設為 [不**允許**]，以及要套用到程式的規則。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改過的軟體限制原則未生效
**原因：** 透過群組原則，在網域中指定的軟體限制原則會覆寫本機設定的任何原則設定。 這可能表示有一個原則設定來自覆寫原則設定的網域。

**原因：** 群組原則可能尚未重新整理其原則設定。 群組原則會定期將變更套用至原則設定;因此，在目錄中所做的原則變更可能尚未重新整理。

**適用**

1.  您用來修改網路軟體限制原則的電腦必須能夠與網域控制站連線。 確定電腦可以與網域控制站連線。

2.  登出網路，然後再次登入網路，以重新整理原則。 如果透過群組原則套用任何原則，登入將會重新整理這些原則。

3.  您可以使用命令列公用程式 gpupdate 或登出，然後重新登入您的電腦，以更新原則設定。 為獲得最佳結果，請執行 gpupdate，然後登出再重新登入您的電腦。 一般來說，在工作站或伺服器上每90分鐘都會重新整理安全性設定，而在網域控制站上每隔5分鐘重新整理一次。 此外，無論是否有變更，這些設定每 16 小時也都會重新整理一次。 這些是可設定的設定值，因此每個網域中的重新整理間隔可能會不同。

4.  檢查適用的原則。 檢查網域層級原則是否**沒有覆寫**設定。

5.  透過群組原則，在網域中指定的軟體限制原則會覆寫本機設定的任何原則。 使用 Gpresult 命令列工具判斷原則的最終效果。 這可能表示有來自網域的原則正在覆寫您的本機設定。

6.  如果 SRP 和 AppLocker 原則設定位於相同的 GPO 中，AppLocker 設定會優先于 Windows 7、Windows Server 2008 R2 和更新版本。 建議將 SRP 和 AppLocker 原則設定放在不同的 Gpo 中。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>透過 SRP 新增規則之後，您就無法登入電腦
**原因：** 您的電腦會在啟動時存取許多程式和檔案。 您可能會不小心將這些程式或檔案的其中一個設定為 [不**允許**]。 因為電腦無法存取程式或檔案，所以無法正確啟動。

**解決方案：** 以安全模式啟動電腦，以本機系統管理員的身分登入，然後變更軟體限制原則，以允許程式或檔案執行。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的原則設定未套用至特定的副檔名
**原因：** 副檔名不在支援的檔案類型清單中。

**解決方案：** 將檔案名延伸模組新增至 SRP 支援的檔案類型清單。

軟體限制原則解決了控制不明或不受信任程式碼的問題。 軟體限制原則是一種安全性設定，用以識別軟體並控制其在本機電腦、網站、網域或 OU 中執行的能力，並可透過 GPO 來執行。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>預設規則未如預期般地進行限制
**原因：** 以特定順序套用的規則，可能會導致特定規則覆寫預設規則。 SRP 會依照下列順序套用規則（最特定為一般）：

1.  雜湊規則

2.  憑證規則

3.  路徑規則

4.  網際網路區域規則

5.  預設規則

**解決方案：** 評估限制應用程式的規則，並在適當的情況下，移除所有的預設規則。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>無法探索已套用的限制
**原因：** 未預期的行為沒有明顯的原因，而且 GPO 重新整理尚未解決此問題，因此需要進一步的調查。

**適用**

1.  調查 [系統] 事件記錄檔，篩選 [軟體限制原則] 的來源。 這些專案會明確陳述每個應用程式所要執行的規則。

2.  啟用 advanced 記錄。 如需詳細資訊，請參閱[決定軟體限制原則的允許拒絕清單和應用程式清查](software-restriction-policies.md)。


