---
title: 步驟 4 - 設定自動更新的群組原則設定
description: Windows Server Update Service (WSUS) 主題 - 設定自動更新的群組原則設定是部署 WSUS 的四步驟程序中的第四個步驟
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3beaea0ff9d0ab3851cfadbd516f36b45bd697ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624572"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>步驟 4：設定自動更新的群組原則設定

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 環境中，您可以使用群組原則來定義電腦和使用者 (在此文件中稱為 WSUS 用戶端) 如何與 Windows Update 互動，以從 Windows Server Update Services (WSUS) 取得自動更新。

本主題包含兩個主要小節：

[WSUS 用戶端更新的群組原則設定](#group-policy-settings-for-wsus-client-updates)，此節針對群組原則中用來控制 WSUS 用戶端如何與 Windows Update 互動以取得自動更新的 Windows Update 和維護排程器設定，提供相關的規範性指引和行為詳細資料。

[補充資訊](#supplemental-information)包含以下小節：

-   [存取群組原則中的 Windows Update 設定](#accessing-the-windows-update-settings-in-group-policy)，此節提供有關於使用群組原則管理編輯器的一般指引，以及存取群組原則中的更新服務原則延伸和維護排程器設定的相關資訊。

-   [與本指南相關的 WSUS 變更](#changes-to-wsus-relevant-to-this-guide)：對於熟悉 WSUS 3.2 和舊版的系統管理員，此節會簡要摘錄 WSUS 的現行版本與舊版之間與本指南相關的主要差異。

-   [詞彙和定義](#terms-and-definitions)：定義本指南中使用的 WSUS 和更新服務的各個相關詞彙。

## <a name="group-policy-settings-for-wsus-client-updates"></a>WSUS 用戶端更新的群組原則設定
本節提供三個群組原則延伸的相關資訊。 在這些延伸中，您會看到可用來設定 WSUS 用戶端如何與 Windows Update 互動以接收自動更新的設定。

-   [電腦設定 &gt; Windows Update 原則設定](#computer-configuration--windows-update-policy-settings)

-   [電腦設定 &gt; 維護排程器原則設定](#computer-configuration--maintenance-scheduler-policy-settings)

-   [使用者設定 &gt; Windows Update 原則設定](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> 本主題假設您已使用過並且熟悉群組原則。 如果您不熟悉群組原則，建議您先檢閱此文件的[補充資訊](#supplemental-information)一節中的資訊，再嘗試設定 WSUS 的原則設定。

### <a name="computer-configuration--windows-update-policy-settings"></a>電腦設定 > Windows Update 原則設定
本節會詳細說明下列以電腦為基礎的原則設定：

-   [允許立即安裝自動更新](#allow-automatic-updates-immediate-installation)

-   [允許非系統管理員接收更新通知](#allow-non-administrators-to-receive-update-notifications)

-   [允許來自近端內部網路 Microsoft 更新服務位置的已簽署更新](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [自動更新偵測頻率](#automatic-updates-detection-frequency)

-   [設定自動更新](#configure-automatic-updates)

-   [延遲排程安裝的重新啟動](#delay-restart-for-scheduled-installations)

-   [不要將 [關閉 Windows] 對話方塊中的預設選項調整為 [安裝更新並關機]](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [啟用用戶端目標](#enable-client-side-targeting)

-   [啟用 Windows Update 電源管理，以自動喚醒電腦並安裝排定的更新](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [有使用者登入時不自動重新開機以完成排定的自動更新安裝](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [再次提示排程安裝所需的重新啟動](#re-prompt-for-restart-with-scheduled-installations)

-   [重新排程已經排程好的自動更新安裝](#reschedule-automatic-updates-scheduled-installations)

-   [指定近端內部網路 Microsoft 更新服務的位置](#specify-intranet-microsoft-update-service-location)

-   [透過自動更新安裝建議的更新](#turn-on-recommended-updates-via-automatic-updates)

-   [開啟軟體通知](#turn-on-software-notifications)

在 GPME 中，以電腦為基礎的設定適用的 Windows Update 原則會位於下列路徑：*PolicyName* > **電腦設定** > **原則** > **系統管理範本** > **Windows 元件** > **Windows Update**。

> [!NOTE]
> 依預設不會設定這些設定。

#### <a name="allow-automatic-updates-immediate-installation"></a>允許立即安裝自動更新
指定「自動更新」是否會自動安裝不會中斷 Windows 服務或重新啟動 Windows 的更新。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 如果「設定自動更新」原則設定設為 [已停用]  ，此原則就沒有效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定不會立即安裝更新。 本機系統管理員可以使用本機群組原則編輯器來變更此設定。|
|**已啟用**|指定「自動更新」會在已下載並可供安裝後立即安裝更新。|
|**已停用**|指定不會立即安裝更新。|

**選項：** 此設定沒有任何選項。

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>允許非系統管理員接收更新通知
指定非系統管理使用者是否會根據「設定自動更新」原則設定接收更新通知。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|請參閱下表中的詳細資料。|

> [!NOTE]
> 如果「設定自動更新」原則設定已停用或未設定，此原則設定就沒有效用。

> [!IMPORTANT]
> 從 Windows 8 和 Windows RT 開始，依預設會啟用此原則設定。 在所有先前的 Windows 版本中，此設定皆預設為停用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定使用者一律會看到 [帳戶控制] 視窗，且需具備提高的權限才能執行這些工作。 本機系統管理員可以使用本機群組原則編輯器來變更此設定。|
|**已啟用**|指定 Windows 自動更新和 Microsoft Update 在判斷哪些登入的使用者會收到更新通知時，會包含非系統管理員。 非系統管理使用者將能夠安裝他們已收到通知的所有選用、建議和重要的更新內容。 使用者將不會看到 [使用者帳戶控制] 視窗，且他們不需具備提高的權限即可安裝這些更新，但包含使用者介面、使用者授權合約或 Windows Update 設定變更的更新除外。<p>在下列兩種情況下，此設定的效用會取決於作業系統：<p>1.**隱藏**或**還原**更新<br />2.**取消**更新安裝<p>在 Windows Vista 或 Windows XP 中，如果已啟用此原則設定，使用者將不會看到 [使用者帳戶控制] 視窗，且他們不需具備提高的權限即可隱藏、還原或取消更新。<p>在 Windows Vista 中，如果已啟用此原則設定，使用者將不會看到 [使用者帳戶控制] 視窗，且他們不需具備提高的權限即可隱藏、還原或取消更新。 如果此原則設定未啟用，使用者將一律會看到 [帳戶控制] 視窗，且他們必須具備提高的權限才能隱藏、還原或取消更新。<p>在 Windows 7 中，此原則設定沒有效用。 使用者一律會看到 [帳戶控制] 視窗，且需具備提高的權限才能執行這些工作。<p>在 Windows 8 和 Windows RT 中，此原則設定沒有效用。|
|**已停用**|指定只有已登入的系統管理員才會收到更新通知。 **注意：** 在 Windows 8 和 Windows RT 中，依預設會啟用此原則設定。 在所有先前的 Windows 版本中，此設定皆預設為停用。|

**選項：** 此設定沒有任何選項。

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>允許來自近端內部網路 Microsoft 更新服務位置的已簽署更新
指定在近端內部網路 Microsoft 更新服務位置發現更新時，「自動更新」是否接受 Microsoft 以外的實體簽署的更新。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT|

> [!NOTE]
> 近端內部網路 Microsoft 更新服務以外的服務更新一律必須由 Microsoft 簽署，且不受此原則設定影響。

> [!NOTE]
> Windows RT 上不支援此原則。 啟用此原則對於執行 Windows RT 的電腦不會有任何影響。

**選項：** 此設定沒有任何選項。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定來自近端內部網路 Microsoft 更新服務位置的更新，必須由 Microsoft 簽署。|
|**已啟用**|指定「自動更新」會接受透過近端內部網路 Microsoft 更新服務位置接收的更新，但前提是這些更新必須是由本機電腦的受信任發行者憑證存放區中包含的憑證所簽署。|
|**已停用**|指定來自近端內部網路 Microsoft 更新服務位置的更新，必須由 Microsoft 簽署。|

**選項：** 此設定沒有任何選項。

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>一律在排定的時間自動重新啟動
指定是否一律會在 Windows Update 安裝重要更新後立即開始執行重新啟動計時器，而不是在登入畫面上通知使用者至少兩天。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 如果啟用「有使用者登入時不自動重新開機以完成排定的自動更新安裝」原則設定，此原則就沒有效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定 Windows Update 將不會改變電腦的重新啟動行為。|
|**已啟用**|指定一律會在 Windows Update 安裝重要更新後立即開始執行重新啟動計時器，而不是在登入畫面上通知使用者至少兩天。<p>重新啟動計時器的啟動時間可以設定為 15 到 180 分鐘之間的任何值。 當計時器執行完畢時，即使電腦還有登入的使用者，仍會執行重新啟動。|
|**已停用**|指定 Windows Update 將不會改變電腦的重新啟動行為。|

**選項：** 此設定啟用時，您可以指定在安裝更新後需經過多久的時間才會強制電腦重新啟動。

#### <a name="automatic-updates-detection-frequency"></a>自動更新偵測頻率
指定 Windows 用來判斷檢查可用更新之前的等待時間，以小時為單位。 確實等待時間的決定方式，是使用這裡指定的時數減去所指定時數的 0 到 20%。 例如，如果使用這項原則指定 20 小時偵測頻率，則套用這項原則的所有用戶端都會在 16 到 20 小時之間檢查更新。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT|

> [!NOTE]
> 必須啟用「指定近端內部網路 Microsoft 更新服務的位置」設定，此原則才有效用。
>
> 如果「設定自動更新」原則設定停用，此原則就沒有效用。

> [!NOTE]
> Windows RT 上不支援此原則。 啟用此原則對於執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定 Windows 將會依預設的時間間隔 22 小時來檢查可用的更新。|
|**已啟用**|指定 Windows 將會依指定的時間間隔來檢查可用的更新。|
|**已停用**|指定 Windows 將會依預設的時間間隔 22 小時來檢查可用的更新。|

**選項：** 此設定啟用時，您可以指定 Windows Update 在檢查更新之前等待的時間間隔 (以小時為單位)。

#### <a name="configure-automatic-updates"></a>設定自動更新
指定是否要在此電腦上啟用自動更新。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT|

啟用時，您必須選取此群組原則設定中提供的四個選項之一。

若要使用此設定，請選取 [已啟用]  ，然後在 [設定自動更新]  底下的 [選項]  中，選取其中一個選項 (2、3、4 或 5)。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定未在群組原則層級指定要使用自動更新。 不過，電腦系統管理員仍可在控制台中設定自動更新。|
|**已啟用**|指定 Windows 可辨識電腦何時上線，並使用其網際網路連線搜尋 Windows Update 中是否有可用的更新。<p>啟用時，會允許本機系統管理員使用 Windows Update 控制台自行選取其設定選項。 不過，本機系統管理員不可停用自動更新的設定。<p>-   **2 - 通知我下載和通知我安裝**<br />    當 Windows Update 發現適用於電腦的更新時，使用者將會收到更新已可供下載的通知。 然後，使用者可以執行 Windows Update 以下載並安裝任何可用的更新。<br />-   **3 - 自動下載和通知我安裝** (預設設定)<br />    Windows Update 在發現適用的更新後會於背景進行下載；在此過程中，使用者不會收到通知或受到干擾。 下載完成時，使用者將會收到更新已可供安裝的通知。 然後，使用者可以執行 Windows Update 以安裝已下載的更新。<br />-   **4 - 自動下載和排程安裝**<br />    您可以使用此群組原則設定中的選項來指定排程。 若未指定排程，所有安裝的預設排程都將是每天凌晨 3:00。 如果有任何更新需要重新啟動才能完成安裝，Windows 將會自動重新啟動電腦。 (如果使用者在 Windows 準備好重新啟動時已登入電腦，則使用者將會收到通知，並且可以選擇延遲重新啟動。)**注意：** 從 Windows 8 開始，您可以將更新設定為在自動維護期間安裝，而不使用與 Windows Update 繫結的特定排程。 自動維護會在電腦未使用時安裝更新，並且避免在電腦以電池電源執行時安裝更新。 如果自動維護無法在數天內安裝更新，Windows Update 將會立即安裝更新。 其後，使用者將會收到關於擱置重新啟動的通知。 只有在不會意外遺失資料的情況下，才會執行擱置中的重新啟動。    您可以在 [GPME 維護排程器] 設定中指定排程選項 (位於下列路徑中：*PolicyName* > **電腦設定** > **原則** > **系統管理範本** > **Windows 元件** > **維護排程器** > **自動維護啟用界限**)。 如需設定的詳細資料，請參閱此參考文件中具有下列標題的小節：[維護排程器設定](#computer-configuration--maintenance-scheduler-policy-settings)。    **5 - 允許本機系統管理員選擇設定**<br />- 指定是否允許本機系統管理員使用「自動更新」控制台自行選取其設定選項，例如，本機系統管理員是否可選擇排定的安裝時間。<br />    本機系統管理員不可停用自動更新的設定。|
|**已停用**|指定可從公用 Windows Update 服務取得的任何用戶端更新，都必須從網際網路手動下載並安裝。|

#### <a name="delay-restart-for-scheduled-installations"></a>延遲排程安裝的重新啟動
指定「自動更新」在繼續執行排定的重新啟動前所將等待的時間長度。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 只有在「自動更新」設定為執行排定的更新安裝時，此原則才適用。 如果「設定自動更新」原則設定停用，此原則就沒有效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定在安裝更新後，需經過預設的等待時間 15 分鐘，才會執行任何排定的重新啟動。|
|**已啟用**|指定在安裝完成後，必須經過指定的分鐘數後，才會執行排定的重新啟動。|
|**已停用**|指定在安裝更新後，需經過預設的等待時間 15 分鐘，才會執行任何排定的重新啟動。|

**選項：** 此設定啟用時，您可以使用此選項來指定「自動更新」在繼續執行排定的重新啟動之前所等待的時間長度 (以分鐘為單位)。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>不要將 [關閉 Windows] 對話方塊中的預設選項調整為 [安裝更新並關機]
此原則設定可讓您指定是否允許將 [安裝更新並關機]  選項作為 [關閉 Windows]  對話方塊中的預設選項。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 如果 *PolicyName* > **電腦設定** > **原則** > **系統管理範本** > **Windows 元件** > **Windows Update** > **不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項**原則設定已啟用，此原則設定將不會有任何效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定如果在使用者選取關機選項以關閉電腦時有更新可供安裝，則 [安裝更新並關機]  將是 [關閉 Windows]  對話方塊中的預設選項。|
|**已啟用**|如果您啟用此原則設定，則使用者上次的關機選項 (例如 [休眠] 或 [重新啟動])，將是 [關閉 Windows]  對話方塊中的預設選項，無論 [安裝更新並關機]  是否為 [您要電腦執行什麼工作?]  功能表中的可用選項。|
|**已停用**|指定如果在使用者選取關機選項以關閉電腦時有更新可供安裝，則 [安裝更新並關機]  將是 [關閉 Windows]  對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>不連接到任何 Windows Update 網際網路位置
即使 Windows Update 設定為從近端內部網路更新服務接收更新，它仍會定期從公用 Windows Update 服務接收資訊，以便未來與 Microsoft Update 連線，以及接收其他服務 (像是 Microsoft Update 或 Microsoft Store) 的資訊。

啟用此原則後，將會停用從公用 Windows Server Update Service 定期擷取資訊的功能，且可能會導致公用服務 (例如 Microsoft Store) 的連線停止運作。

|支援：|排除：|
|---------|-------|
|從 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8.1 開始，仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 只在電腦設定為使用「指定近端內部網路 Microsoft 更新服務的位置」原則設定連線至近端內部網路更新服務，此原則才適用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|從公用 Windows Server Update Service 擷取資訊的預設行為會持續存在。|
|**已啟用**|指定電腦不會從公用 Windows Server Update Service 擷取資訊。|
|**已停用**|從公用 Windows Server Update Service 擷取資訊的預設行為會持續存在。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項
指定是否要在 [關閉 Windows]  對話方塊中顯示 [安裝更新並關機]  選項。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定如果在使用者選取關機選項以關閉電腦時有可用的更新，則 [安裝更新並關機]  將是 [關閉 Windows]  對話方塊中的可用選項。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定如果即使在使用者選取關機選項以關閉電腦時有更新可供安裝，[安裝更新並關機]  也不會顯示為 [關閉 Windows]  對話方塊中的選項。|
|**已停用**|指定如果在使用者選取關機選項以關閉電腦時有更新可供安裝，則 [安裝更新並關機]  選項將是 [關閉 Windows]  對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="enable-client-side-targeting"></a>啟用用戶端目標
指定在 WSUS 主控台中設定用來從 WSUS 接收更新的目標群組名稱。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT|

> [!NOTE]
> 只有將此電腦設定為支援 WSUS 中指定的目標群組名稱時，此原則才適用。 目標群組名稱若不存在於 WSUS 中，則會被忽略，直到它建立為止。 如果「指定近端內部網路 Microsoft 更新服務的位置」原則設定已停用或未設定，此原則就沒有效用。

> [!NOTE]
> Windows RT 上不支援此原則。 啟用此原則對於執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定不將目標群組資訊傳送至 WSUS。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定將指定的目標群組資訊傳送至 WSUS，以使用它來決定應將哪些更新部署到這部電腦。 如果 WSUS 支援多個目標群組，且您已在 WSUS 的電腦群組清單中新增了目標群組名稱，則可以使用此原則來指定多個群組名稱 (以分號分隔)。 否則必須指定單一群組。|
|**已停用**|指定不將目標群組資訊傳送至 WSUS。|

**選項：** 使用此空間來指定一或多個目標群組名稱。

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>啟用 Windows Update 電源管理，以自動喚醒電腦並安裝排定的更新
指定在有排定要安裝的更新時，Windows Update 是否會使用 Windows 電源管理或電源選項功能，自動將電腦從休眠狀態中喚醒。

只有在 Windows Update 設定為自動安裝更新時，電腦才會自動喚醒。 如果電腦在排定的安裝時間到達且有更新要套用時處於休眠狀態，Windows Update 將會使用 Windows 電源管理或電源選項功能自動喚醒電腦，以安裝更新。 在安裝期限到達時，Windows Update 也會喚醒電腦並安裝更新。

除非有要安裝的更新，否則不會喚醒電腦。 如果電腦使用電池電源，當 Windows Update 將其喚醒時，電腦將不會安裝更新，而會在兩分鐘內自動回到休眠狀態。

|支援：|排除：|
|---------|-------|
|從 Windows Vista 和 Windows Server 2008 (Windows 7) 開始，仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|Windows Update 不會將電腦從休眠狀態中喚醒以安裝更新。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|在前述情況下，Windows Update 會將電腦從休眠狀態中喚醒以安裝更新。|
|**已停用**|Windows Update 不會將電腦從休眠狀態中喚醒以安裝更新。|

**選項：** 此設定沒有任何選項。

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>有使用者登入時不自動重新開機以完成排定的自動更新安裝
指定「自動更新」會等候任何登入的使用者重新啟動電腦以完成排定的安裝，而不是使電腦自動重新啟動。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 只有在「自動更新」設定為執行排定的更新安裝時，此原則才適用。 如果「設定自動更新」原則設定停用，此原則就沒有效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定「自動更新」會通知使用者電腦將在五分鐘內自動重新啟動以完成安裝。|
|**已啟用**|有些更新必須在電腦重新啟動後才會生效。 如果狀態設定為 [已啟用]，且使用者已登入電腦，「自動更新」將不會在排定的安裝執行期間自動重新啟動電腦。 此時，「自動更新」會通知使用者重新啟動電腦。|
|**已停用**|指定「自動更新」會通知使用者電腦將在五分鐘內自動重新啟動以完成安裝。|

**選項：** 此設定沒有任何選項。

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>再次提示排程安裝所需的重新啟動
指定「自動更新」在再次發出排定的重新啟動提示前所等待的時間長度。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT|

> [!IMPORTANT]
> 只有在「自動更新」設定為執行排定的更新安裝時，此原則才適用。 如果「設定自動更新」原則設定停用，此原則就沒有效用。

> [!NOTE]
> 此原則在執行 Windows RT 的電腦上沒有任何效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|排定的重新啟動會在重新啟動提示訊息關閉的十分鐘後執行。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定在上一個重新啟動提示延期之後，排定的重新啟動將會在指定的分鐘數經過之後執行。|
|**已停用**|排定的重新啟動會在重新啟動提示訊息關閉的十分鐘後執行。|

**選項：** 啟用時，您可以使用此設定選項來指定需經過多久的時間 (以分鐘為單位)，才會再次對使用者發出排定的重新啟動提示。

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>重新排程已經排程好的自動更新安裝
指定在電腦啟動後，「自動更新」會等候多久的時間才會繼續執行先前錯過的排程安裝。

如果狀態設為 [未設定]  ，則會在電腦下次啟動的一分鐘後執行先前錯過的排程安裝。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 只有在「自動更新」設定為執行排定的更新安裝時，此原則才適用。 如果「設定自動更新」原則設定停用，此原則就沒有效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定會在電腦下次啟動的一分鐘後執行先前錯過的排程安裝。|
|**已啟用**|指定先前未執行的排程安裝將會在電腦下次啟動後的指定分鐘數經過時執行。|
|**已停用**|指定先前錯過的排程安裝將會與下一個排程安裝一起執行。|

**選項：** 此原則設定啟用時，您可以用它來指定先前未執行的排程安裝將會在電腦下次啟動後的第幾分鐘執行。

#### <a name="specify-intranet-microsoft-update-service-location"></a>指定近端內部網路 Microsoft 更新服務的位置
指定內部網路伺服器來裝載 Microsoft Update 的更新。 然後，您可以使用 WSUS 自動更新網路上的電腦。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|Windows RT。|

此設定可讓您指定網路上要作為內部更新服務的 WSUS 伺服器。 WSUS 用戶端不會使用網際網路上的 Microsoft Update，而是會搜尋此服務中是否有適用的更新。

若要使用此設定，您必須設定兩個伺服器名稱值：用戶端從中偵測和下載更新的伺服器，以及更新的工作站上傳統計資料的目標伺服器。 如果兩個服務設定在同一部伺服器上，則這些值不需要不同。

> [!NOTE]
> 如果「設定自動更新」原則設定停用，此原則就沒有效用。

> [!NOTE]
> Windows RT 上不支援此原則。 啟用此原則對於執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|如果原則或使用者喜好設定未停用自動更新，則此原則會指定用戶端將直接連線至網際網路上的 Windows Update 網站。|
|**已啟用**|指定用戶端會連線至指定的 WSUS 伺服器 (而不是 Windows Update)，以搜尋和下載更新。 啟用這個設定表示組織中的使用者不必通過防火牆就能取得更新，且您有機會在部署更新之前先加以測試。|
|**已停用**|如果原則或使用者喜好設定未停用自動更新，則此原則會指定用戶端將直接連線至網際網路上的 Windows Update 網站。|

**選項：** 此原則設定啟用時，您必須指定 WSUS 用戶端在偵測更新時所將使用的內部網路更新服務，以及更新的 WSUS 用戶端上傳統計資料的目標網際網路統計伺服器。 範例值：


|                    設定選項：                    |    範例值：    |
|-------------------------------------------------------|----------------------|
| 設定用來偵測更新的內部網路更新服務 |  http://wsus01:8530  |
|          設定內部網路統計伺服器           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>透過自動更新安裝建議的更新
指定「自動更新」是否會從 WSUS 傳遞重要和建議的更新。

|支援：|排除：|
|---------|-------|
|從 Windows Vista 開始，仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定「自動更新」會繼續傳遞重要更新 (如果已進行此設定)。|
|**已啟用**|指定「自動更新」會從 WSUS 安裝建議的更新和重要更新。|
|**已停用**|指定「自動更新」會繼續傳遞重要更新 (如果已進行此設定)。|

**選項：** 此設定沒有任何選項。

#### <a name="turn-on-software-notifications"></a>開啟軟體通知
此原則設定可讓您控制使用者是否會看到與 Microsoft Update 服務提供的精選軟體有關的詳細增強通知訊息。 增強通知訊息會傳達選用軟體的價值，並鼓勵使用者安裝及使用。 此原則設定適用於管理不嚴格、且允許使用者存取 Microsoft Update 服務的環境中。

如果您未使用 Microsoft Update 服務，「軟體通知」原則設定就沒有效用。

如果「設定自動更新」原則設定已停用或未設定，「軟體通知」原則設定就沒有效用。

|支援：|排除：|
|---------|-------|
|從 Windows Server 2008 (Windows Vista) 和 Windows 7 開始，仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 依預設會停用此原則設定。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|使用者的電腦若是執行 Windows 7，則不會收到選用應用程式的訊息。 使用者的電腦若是執行 Windows Vista，則不會收到選用應用程式或更新的訊息。 本機系統管理員可以使用控制台或本機原則來變更此設定。|
|**已啟用**|如果您啟用此原則設定，在有可用的精選軟體時，使用者的電腦上將會出現通知訊息。 使用者可按一下通知以開啟 Windows Update，並取得軟體本身或加以安裝的詳細資訊。 使用者也可以按一下 [關閉此訊息]  或 [以後再通知我]  ，以適當地延遲通知。<p>在 Windows 7 中，此原則設定只會控制選用應用程式的詳細通知。 在 Windows Vista 中，此原則設定會控制選用應用程式和更新的詳細通知。|
|**已停用**|指定執行 Windows 7 的使用者將不會收到選用應用程式的詳細通知訊息，而執行 Windows Vista 的使用者將不會收到選用應用程式或選用更新的詳細通知訊息。|

**選項：** 此設定沒有任何選項。

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>電腦設定 > 維護排程器原則設定
若您已在 [設定自動更新] 設定中選取了 [4 - 自動下載並排程安裝]  選項，則可以在 GPMC 中為執行 Windows 8 和 Windows RT 的電腦指定排程維護排程器設定。 如果您未在 [設定自動更新] 設定中選取選項 4，就不需要為了自動更新設定這些設定。 維護排程器設定位於下列路徑：*PolicyName* > **電腦設定** > **原則** > **系統管理範本** > **Windows 元件** > **維護排程器**。 群組原則的維護排程器延伸包含下列設定：

-   [自動維護啟用界限](#automatic-maintenance-activation-boundary)

-   [自動維護隨機延遲](#automatic-maintenance-random-delay)

-   [自動喚醒原則](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>自動維護啟用界限
此原則可讓您設定「自動維護啟用界限」設定。

維護啟用界限是自動維護開始執行的每日排程時間。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 此設定與**設定自動更新**中的選項 4 有關。 如果您未在**設定自動更新**中選取選項 4，就不需要設定此設定。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|若未設定此原則設定，則會套用 [動作中心]   > [自動維護]  控制台中的用戶端電腦所指定的每日排程時間。|
|**已啟用**|若啟用此原則設定，將會覆寫 [控制台]   > [動作中心]   > [自動維護]  (或某些用戶端版本中的 [維護]  ) 中的用戶端電腦上設定的任何預設或已修改的設定。|
|**已停用**|如果您將此原則設定設為 [已停用]  ，則會套用 [控制台] 中的 [動作中心]   > [自動維護]  所指定的每日排程時間。|

#### <a name="automatic-maintenance-random-delay"></a>自動維護隨機延遲
此原則設定可讓您設定自動維護啟用隨機延遲。

維護隨機延遲是「自動維護」從其啟用界限起算的最大延遲時間長度。 此設定適用於隨機維護可能是效能需求的虛擬機器。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 此設定與**設定自動更新**中的選項 4 有關。 如果您未在**設定自動更新**中選取選項 4，就不需要設定此設定。

根據預設，在啟用時，定期維護隨機延遲會設定為 **PT4H**。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|[自動]  會套用四小時的隨機延遲。|
|**已啟用**|「自動維護」會從其啟用界限開始延遲最多達指定的時間長度。|
|**已停用**|「自動維護」不會套用隨機延遲。|

#### <a name="automatic-wakeup-policy"></a>自動喚醒原則
此原則設定可讓您設定「自動維護」喚醒原則。

維護喚醒原則會指定「自動維護」是否應對作業電腦發出每日排程維護的喚醒要求。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 如果作業電腦的電源喚醒原則已明確停用，此設定就沒有效用。

> [!NOTE]
> 此設定與**設定自動更新**中的選項 4 有關。 如果您未在**設定自動更新**中選取選項 4，就不需要設定此設定。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|如果您未設定此原則設定，則會套用 [動作中心]   > [自動維護]  控制台中指定的喚醒設定。|
|**已啟用**|如果您啟用了此原則設定，「自動維護」將會嘗試設定作業系統喚醒原則，並在需要時提出每日排程時間的喚醒要求。|
|**已停用**|如果您停用此原則設定，則會套用 [動作中心]   > [自動維護]  控制台中指定的喚醒設定。|

### <a name="user-configuration--windows-update-policy-settings"></a>使用者設定 > Windows Update 原則設定
本節會詳細說明下列以使用者為基礎的原則設定：

-   [不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [不要將 [關閉 Windows] 對話方塊中的預設選項調整為 [安裝更新並關機]](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [移除對使用所有 Windows Update 功能的存取](#remove-access-to-use-all-windows-update-features)

在 GPMC 中，自動電腦更新的使用者設定位於下列路徑：*PolicyName* > **使用者設定** > **原則** > **系統管理範本** > **Windows 元件** > **Windows Update**。 在選取 Windows Update 原則的 [設定]  索引標籤以依字母順序排序設定時，這些設定的列出順序將會與它們出現在群組原則中的電腦設定和使用者設定延伸中的順序相同。

> [!NOTE]
> 根據預設，除非另有註明，否則不會設定這些設定。

> [!TIP]
> 針對每項設定，您可以使用下列步驟來啟用、停用或瀏覽設定：

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項
指定是否要在 [關閉 Windows]  對話方塊中顯示 [安裝更新並關機]  選項。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定如果在使用者選取關機選項以關閉電腦時有可用的更新，則 [安裝更新並關機]  選項將會出現 [關閉 Windows]  對話方塊中。|
|**已啟用**|如果您起用此原則設定，則即使在使用者選取關機選項以關閉電腦時有更新可供安裝，[安裝更新並關機]  也不會顯示為 [關閉 Windows]  對話方塊中的選項。|
|**已停用**|指定如果在使用者選取關機選項以關閉電腦時有可用的更新，則 [安裝更新並關機]  選項將會出現 [關閉 Windows]  對話方塊中。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>不要將 [關閉 Windows] 對話方塊中的預設選項調整為 [安裝更新並關機]
指定是否允許 [安裝更新並關機]  選項作為 [關閉 Windows]  對話方塊中的預設選項。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

> [!NOTE]
> 如果 *PolicyName* > **使用者設定** > **原則** > **系統管理範本** > **Windows 元件** > **Windows Update** > **不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項**已啟用，此原則設定將不會有任何效用。

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|指定如果在使用者選取關機選項以關閉電腦時有更新可供安裝，[安裝更新並關機]  選項是否為 [關閉 Windows]  對話方塊中的預設選項。|
|**已啟用**|指定使用者上次的關機選項 (例如 [休眠] 或 [重新啟動]) 是否為 [關閉 Windows]  對話方塊中的預設選項，無論 [安裝更新並關機]  是否為 [您要電腦執行什麼工作?]  功能表中的可用選項。|
|**已停用**|指定如果在使用者選取關機選項以關閉電腦時有更新可供安裝，[安裝更新並關機]  選項是否為 [關閉 Windows]  對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="remove-access-to-use-all-windows-update-features"></a>移除對使用所有 Windows Update 功能的存取
此設定可讓您移除對 Windows Update 的 WSUS 用戶端存取權。

|支援：|排除：|
|---------|-------|
|仍在 [Microsoft 產品支援生命週期](https://support.microsoft.com/gp/lifeselect)內的 Windows 作業系統。|null|

|||
|-|-|
|**原則設定狀態**|**行為**|
|**未設定**|使用者可以連線至 Windows Update 網站。|
|**已啟用**|**重要事項：** 如果啟用，則會移除所有的 Windows Update 功能。 這會封鎖從 [開始] 功能表或 [開始] 畫面上的 Windows Update 超連結以及 Internet Explorer 中的 [工具]  功能表對 https://windowsupdate.microsoft.com 上的 Windows Update 網站進行的存取。 Windows 自動更新也會停用；使用者不會收到 Windows Update 的相關通知，也不會收到其重大更新。 此設定也會防止裝置管理員從 Windows Update 網站自動安裝驅動程式更新。<p>啟用時，您可以設定下列其中一個通知選項：<p>-   **0 - 不顯示任何通知**<br />    此設定會移除對 Windows Update 功能的所有存取權，且不會顯示任何通知。<br />-   **1 - 顯示需要重新啟動通知**<br />    此設定會顯示必須重新啟動才能完成安裝的相關通知。 **注意：** 在執行 Windows 8 和 Windows RT 的電腦上，如果啟用此原則，將只會顯示與重新啟動和無法偵測到更新有關的通知。 不支援通知選項。 登入畫面上的通知一律會顯示。|
|**已停用**|使用者可以連線至 Windows Update 網站。|

**選項：** 請參閱此設定在表格中列為 [已啟用]  的部分。

## <a name="supplemental-information"></a>補充資訊
本節提供關於在群組原則中開啟和儲存 WSUS 設定的附加資訊，以及本指南所用詞彙的定義。 熟悉舊版 WSUS (WSUS 3.2 和之前的版本) 的系統管理員，可參考一份簡短摘要說明 WSUS 版本差異的表格。

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>存取群組原則中的 Windows Update 設定
以下程序說明如何在網域控制站上開啟 GPMC。 此程序接著會說明如何開啟現有的網域層級群組原則物件 (GPO) 以進行編輯，或建立新的網域層級 GPO 並開啟以供編輯。

> [!NOTE]
> 您必須是 **Domain Admins** 群組的成員或具同等身分，才能執行此程序。

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>開啟或新增並開啟群組原則物件

1.  在網域控制站上，移至 [伺服器管理員]  > [工具]  > [群組原則管理]  。 此時會開啟群組原則管理主控台。

2.  在左窗格中，展開您的樹系。 例如，按兩下**樹系：example.com**。

3.  在左窗格中按兩下 [網域]  ，然後按兩下您要管理其群組原則物件的網域。 例如，按兩下 **example.com**。

4.  執行下列其中一個動作：

    -  **若要開啟現有的網域層級 GPO 以進行編輯**，請按兩下您要管理的群組原則物件所屬的網域，以滑鼠右鍵按一下您要管理的網域原則，然後按一下 [編輯]  。 群組原則管理編輯器 (GPME) 隨即開啟。

    -  **若要建立新的群組原則物件並開啟以供編輯**：
        1.  以滑鼠右鍵按一下要建立新群組原則物件的網域，然後按一下 [在這個網域中建立 GPO 並連結到]  。

        2.  在 [新增 GPO]  的 [名稱]  中，輸入新群組原則物件的名稱，然後按一下 [確定]  。

        3.  以滑鼠右鍵按一下您新的群組原則物件，然後按一下 [編輯]  。 GPME 隨即開啟。

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>開啟群組原則的 Windows Update 或維護排程器延伸

1.  在群組原則管理編輯器中，執行下列其中一項：

    -   **開啟 [電腦設定] > [群組原則的 Windows Update 延伸]** 。 瀏覽至：*PolicyName* > **電腦設定** > **原則** / **系統管理範本** > **Windows 元件** > **Windows Update**。

    -   **開啟 [使用者設定] > [群組原則的 Windows Update 延伸]** 。 瀏覽至：*PolicyName* > **使用者設定** > **原則** > **系統管理範本** > **Windows 元件** > **Windows Update**。

    -   **開啟 [電腦設定] > [群組原則的維護排程器延伸]** 。 在 GPOE 中，瀏覽至 [PolicyName]   > [電腦設定]   > [原則]   > [系統管理範本]   > [Windows 元件]   > [維護排程器]  。

如需群組原則的詳細資訊，請參閱[群組原則概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831791(v=ws.11))。

> [!TIP]
> 開啟您想要的群組原則延伸之後，您可以使用下列步驟來啟用、停用或瀏覽設定：

##### <a name="to-configure-group-policy-settings"></a>設定群組原則設定

1.  在 *ExtensionOfGroupPolicy* 中，按兩下您要檢視或修改的設定。

2.  若要進行此設定，請執行下列其中一項：

    -   若要保留預設設定未指定的狀態，請選取 [未設定]  。

    -   若要啟用此設定，請選取 [已啟用]  。

    -   若要停用此設定，請選取 [已停用]  。

3.  在 [選項]  中，如果有列出任何選項，請保留預設值，或視需要加以修改。

4.  執行下列其中一個動作：

    -   若要儲存變更並繼續進行下一個設定，請按一下 [套用]  ，然後按 [下一個設定]  。

    -   若要儲存變更並關閉對話方塊，請按一下 [確定]  。

    -   若要捨棄所有未儲存的變更並關閉對話方塊，請按一下 [取消]  。

### <a name="changes-to-wsus-relevant-to-this-guide"></a>與本指南相關的 WSUS 變更
下表摘要說明 WSUS 的現行版本與舊版之間與本指南相關的主要差異。

|Windows Server 和 WSUS 版本|說明|
|------------------|--------|
| 使用 WSUS 6.0 的 Windows Server 2012 R2 和後續版本|從 Windows Server 2012 開始，WSUS 伺服器角色已與作業系統整合，且 WSUS 用戶端的相關群組原則設定依預設會包含在群組原則中。|
| 使用 WSUS 3.2 和更早版本的 Windows Server 2008 (和更早版本的 Windows Server)|在使用 WSUS 3.2 版 (和更早版本) 的 Windows Server 2008 (和更早版本的 Windows Server) 中，管理 WSUS 用戶端的群組原則設定並未包含在這些 Windows Server 作業系統中。 原則設定位於 WSUS 系統管理範本 **wuau.adm** 中。 在這些伺服器版本中，必須先將 WSUS 系統管理範本新增至群組原則管理主控台 (GPMC) 中，才能設定 WSUS 用戶端設定。|

### <a name="terms-and-definitions"></a>詞彙和定義
以下是本指南中使用的詞彙清單。

|詞彙|定義|
|----|-------|
|自動更新|**在 Windows 電腦上執行的服務** (自動更新)：這是指內建於 Microsoft Windows Vista、Windows Server 2003、Windows XP 和 Windows 2000 SP3 作業系統中，用以從 Microsoft Update 或 Windows Update 取得更新的用戶端電腦元件。<p>**一般參考** (自動更新)：此詞彙用來說明 Windows Update 代理程式自動排程及下載更新的情形。|
|自發伺服器|用來指稱系統管理員可在其上管理 WSUS 元件的下游 Windows Server Update Services (WSUS) 伺服器。|
|下游伺服器|用來指稱從 Microsoft Update 或 Windows Update 以外的其他 WSUS 伺服器，取得更新的 Windows Server Update Services (WSUS) 伺服器。|
|群組原則延伸 (也稱為群組原則的延伸)|群組原則中的設定集合，用來控制 (套用原則的) 使用者和電腦如何設定及使用各項 Windows 服務和功能。 系統管理員可搭配使用 WSUS 與群組原則進行自動更新用戶端的用戶端設定，以利確保使用者無法停用或規避公司更新原則。<p>WSUS 不需要使用 Active Directory 或群組原則。 用戶端設定也可藉由使用本機群組原則或修改 Windows 登錄來套用。|
|內部更新服務|對使用一或多個 WSUS 伺服器來散發更新的網路基礎結構所做的一般參考。|
|複本伺服器|用來指稱可對其連接之上游伺服器的核准和設定進行鏡像的下游 Windows Server Update Services (WSUS) 伺服器。 您無法管理複本伺服器上的 WSUS。|
|Microsoft Update|**以網際網路為基礎的 Microsoft 下載網站：** 這是一個 Microsoft 網際網路網站，可儲存和散發 Windows 電腦 (裝置驅動程式)、Windows 作業系統和其他 Microsoft 軟體產品的更新。|
|Software Update Services (SUS)|SUS 是 Windows Server Update Services (WSUS) 產品的前身。|
|更新|任何一組可安裝在電腦上，用以擴充功能或改善效能和安全性的軟體修訂、Hotfix、Service Pack、Feature Pack 和裝置驅動程式。|
|更新檔案|在電腦上安裝更新所需的檔案。|
|更新資訊 (也稱為更新中繼資料)|更新的相關資訊，不同於更新套件中的更新二進位檔案。 例如，中繼資料可提供更新屬性的資訊，而讓您能夠了解更新的用途。 中繼資料也包含 Microsoft 軟體授權條款。 為更新下載的中繼資料套件，通常會比實際更新檔案套件小得多。|
|更新來源|Windows Server Update Services (WSUS) 伺服器進行同步處理以取得更新檔案的目標位置。 此位置可以是 Microsoft Update 或上游 WSUS 伺服器。|
|上游伺服器|一個 Windows Server Update Services (WSUS) 伺服器，可將更新檔案提供給另一部 WSUS 伺服器，也就是所謂的下游伺服器。|
|Windows Server Update Services (WSUS)|在公司網路的一或多部 Windows Server 電腦上執行的伺服器角色程式。 WSUS 基礎結構可讓您管理網路上要為電腦安裝的更新。<p>您可以使用 WSUS，在發行前核准或拒絕更新、強制更新在指定的日期進行安裝，以及取得網路上每一部電腦所需更新的相關詳細報表。 您可以將 WSUS 設定為自動核准特定類別的更新 (重大更新、安全性更新、Service Pack、驅動程式等)。 WSUS 也可讓您核准僅供偵測的更新，而讓您能夠了解哪些電腦需要指定的更新，而不需實際安裝更新。<p>在 WSUS 實作中，網路中至少要有一部 WSUS 伺服器能夠連線至 Microsoft Update，以取得可用的更新。 系統管理員可根據網路安全性和設定，決定有多少部伺服器可以直接連線至 Microsoft Update。<p>您可以設定 WSUS 伺服器，以透過網際網路從如下的位置取得更新：<p>- 公用 Microsoft Update<br />- 公用 Windows Update<br />- Microsoft Store|
|Windows Update|**以網際網路為基礎的 Microsoft 下載網站：** 這是一個 Microsoft 網際網路網站，可儲存和散發 Windows 電腦 (裝置驅動程式) 和 Windows 作業系統的更新。<p>**電腦服務：** 在電腦上執行的 Windows Update 服務名稱。 Windows Update 會在 Windows 電腦上偵測、下載及安裝更新。<p>視電腦和原則設定之不同，Windows Update 代理程式可從下列來源下載更新：<p>- Microsoft Update<br />- Windows Update<br />- Microsoft Store<br />- 網際網路 (網路) 更新服務 (WSUS)<p>未在以 WSUS 為基礎的環境中受到管理的電腦，通常會使用 Windows Update 透過網際網路直接連線至 Windows Update、Microsoft Update 或 Microsoft Store 以取得更新。|
|WSUS 用戶端|從 WSUS 內部網路更新服務接收更新的電腦。<p>如果以群組原則設定控制使用者與「自動更新」的互動，則是指 WSUS 環境中的電腦使用者。|