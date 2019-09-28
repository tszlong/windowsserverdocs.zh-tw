---
title: 步驟 4-設定自動更新的群組原則設定
description: Windows Server Update Service （WSUS）主題-設定自動更新的群組原則設定，這是部署 WSUS 的四個步驟程式中的步驟四
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a01d8881e8f0f7ca6feff691938f926a12460db0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361659"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>步驟 4：設定自動更新的群組原則設定

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 active directory 環境中，您可以使用群組原則來定義電腦和使用者（在本檔中稱為 WSUS 用戶端）如何與 Windows Update 互動，以從 Windows Server Update Services （WSUS）取得自動更新。

本主題包含兩個主要區段：

[Wsus 用戶端更新的群組原則設定](#group-policy-settings-for-wsus-client-updates)，其中提供群組原則的 Windows Update 和維護排程器設定的規範性指引和行為詳細資料，以控制 WSUS 用戶端如何與 Windows Update 互動取得自動更新。

[補充資訊](#supplemental-information)包含下列各節：

-   [存取群組原則中的 Windows Update 設定](#accessing-the-windows-update-settings-in-group-policy)，其中提供使用 [群組原則管理編輯器] 的一般指引，以及存取 [群組] 中的 [更新服務原則延伸] 和 [維護排程器] 設定的相關資訊策略.

-   與[本指南相關的 Wsus 變更](#changes-to-wsus-relevant-to-this-guide)：對於熟悉 WSUS 3.2 和舊版的系統管理員，本節提供與本指南相關之目前和過去版本的 wsus 之間主要差異的簡短摘要。

-   [詞彙和定義](#terms-and-definitions)：與本指南中所使用 WSUS 和更新服務相關之各種詞彙的定義。

## <a name="group-policy-settings-for-wsus-client-updates"></a>WSUS 用戶端更新的群組原則設定
本節提供三個群組原則擴充功能的相關資訊。 在這些延伸模組中，您會找到可用來設定 WSUS 用戶端如何與 Windows Update 互動以接收自動更新的設定。

-   [電腦設定 &gt; Windows Update 原則設定](#computer-configuration--windows-update-policy-settings)

-   [電腦設定 &gt; 維護排程器原則設定](#computer-configuration--maintenance-scheduler-policy-settings)

-   [使用者設定 &gt; Windows Update 原則設定](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> 本主題假設您已經使用，並熟悉群組原則。 如果您不熟悉群組原則，建議您先參閱本檔的[補充資訊](#supplemental-information)一節中的資訊，再嘗試設定 WSUS 的原則設定。

### <a name="computer-configuration--windows-update-policy-settings"></a>Windows Update 原則設定 > 電腦設定
本節提供下列以電腦為基礎的原則設定詳細資料：

-   [允許自動更新立即安裝](#allow-automatic-updates-immediate-installation)

-   [允許非系統管理員接收更新通知](#allow-non-administrators-to-receive-update-notifications)

-   [允許來自內部網路 Microsoft 更新服務位置的已簽署更新](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [自動更新偵測頻率](#automatic-updates-detection-frequency)

-   [設定自動更新](#configure-automatic-updates)

-   [延遲重新開機已排程的安裝](#delay-restart-for-scheduled-installations)

-   [不會調整關閉關閉 Windows 對話方塊中的預設選項來 安裝更新並關機](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [啟用以用戶端為目標](#enable-client-side-targeting)

-   [啟用 Windows Update 電源管理，自動喚醒電腦以安裝排程的更新](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [未針對排程的自動更新安裝自動重新開機已登入的使用者](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [重新提示重新開機已排程的安裝](#re-prompt-for-restart-with-scheduled-installations)

-   [排定自動更新排程的安裝](#reschedule-automatic-updates-scheduled-installations)

-   [指定近端內部網路 Microsoft 更新服務位置](#specify-intranet-microsoft-update-service-location)

-   [透過自動更新開啟建議的更新](#turn-on-recommended-updates-via-automatic-updates)

-   [開啟軟體通知](#turn-on-software-notifications)

在 GPME 中，以電腦為基礎的設定 Windows Update 原則位於下列路徑：*PolicyName* >  部**電腦**設定  >  個**原則** > **系統管理範本** >  個**Windows 元件** > **Windows Update**。

> [!NOTE]
> 根據預設，不會設定這些設定。

#### <a name="allow-automatic-updates-immediate-installation"></a>允許立即安裝自動更新
指定自動更新是否會自動安裝不會中斷 Windows 服務或重新開機 Windows 的更新。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果 [設定自動更新] 原則設定設為 [**停用**]，則此原則不會有任何作用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定不會立即安裝更新。 本機系統管理員可以使用 [本機群組原則編輯器] 來變更此設定。|
|**已啟用**|指定自動更新在下載並準備好安裝之後，立即安裝更新。|
|**已停用**|指定不會立即安裝更新。|

**選項：** 此設定沒有任何選項。

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>允許非系統管理員接收更新通知
指定非系統管理使用者是否將根據 [設定自動更新] 原則設定接收更新通知。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|請參閱下表中的詳細資料。|

> [!NOTE]
> 如果 [設定自動更新] 原則設定已停用或未設定，此原則設定就不會有任何作用。

> [!IMPORTANT]
> 從 Windows 8 和 Windows RT 開始，預設會啟用此原則設定。 在所有先前的 Windows 版本中，預設會停用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定使用者一律會看到 [帳戶控制] 視窗，而且需要更高的許可權才能執行這些工作。 本機系統管理員可以使用 [本機群組原則編輯器] 來變更此設定。|
|**已啟用**|指定在判斷哪些登入的使用者會收到更新通知時，Windows 自動更新和 Microsoft Update 將包含非系統管理員。 非系統管理使用者將能夠安裝他們收到通知的所有選用、建議和重要更新內容。 使用者不會看到 [使用者帳戶控制] 視窗，而且它們不需要較高的許可權即可安裝這些更新，但包含使用者介面、使用者授權合約或 Windows Update 設定變更的情況除外。<br /><br />在兩種情況下，此設定的效果取決於作業系統：<br /><br />1.**隱藏**或**還原**更新<br />2.**取消**更新安裝<br /><br />在 Windows Vista 或 Windows XP 中，如果已啟用此原則設定，使用者將不會看到 [使用者帳戶控制] 視窗，也不需要更高的許可權來隱藏、還原或取消更新。<br /><br />在 Windows Vista 中，如果已啟用此原則設定，使用者將不會看到 [使用者帳戶控制] 視窗，也不需要更高的許可權來隱藏、還原或取消更新。 如果未啟用此原則設定，使用者一律會看到 [帳戶控制] 視窗，而且需要更高的許可權來隱藏、還原或取消更新。<br /><br />在 Windows 7 中，此原則設定沒有作用。 使用者一律會看到 [帳戶控制] 視窗，而且需要更高的許可權才能執行這些工作。<br /><br />在 Windows 8 和 Windows RT 中，此原則設定不會有任何作用。|
|**已停用**|指定只有登入的系統管理員才會收到更新通知。 **注意：** 在 Windows 8 和 Windows RT 中，預設會啟用此原則設定。 在所有先前的 Windows 版本中，預設會停用。|

**選項：** 此設定沒有任何選項。

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>允許來自近端內部網路 Microsoft 更新服務位置的已簽署更新
指定在內部網路 Microsoft 更新服務位置找到更新時，自動更新是否接受由 Microsoft 以外的實體簽署的更新。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 來自內部網路 Microsoft 更新服務以外之服務的更新，必須一律由 Microsoft 簽署，而且不受此原則設定影響。

> [!NOTE]
> Windows RT 不支援此原則。 啟用此原則對執行 Windows RT 的電腦不會有任何影響。

**選項：** 此設定沒有任何選項。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定來自內部網路 Microsoft 更新服務位置的更新必須由 Microsoft 簽署。|
|**已啟用**|指定自動更新接受透過內部網路 Microsoft 更新服務位置接收的更新（如果是由本機電腦的「受信任的發行者」憑證存放區中找到的憑證簽署）。|
|**已停用**|指定來自內部網路 Microsoft 更新服務位置的更新必須由 Microsoft 簽署。|

**選項：** 此設定沒有任何選項。

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>一律在排定的時間自動重新啟動
指定在 Windows Update 安裝重要更新之後，是否一律立即開始重新開機計時器，而不是第一次在登入畫面上通知使用者至少兩天。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果已啟用 [不自動重新開機已登入使用者的排程自動更新] 原則設定，則此原則不會有任何作用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定 Windows Update 不會改變電腦的重新開機行為。|
|**已啟用**|指定在 Windows Update 安裝重要更新之後，一律立即開始重新開機計時器，而不是第一次在登入畫面上通知使用者至少兩天。<br /><br />重新開機計時器可以設定為從15到180分鐘的任何值開始。 當計時器執行時，即使電腦已登入使用者，重新開機仍會繼續。|
|**已停用**|指定 Windows Update 不會改變電腦的重新開機行為。|

**選項：** 如果啟用此設定，您可以指定在強制電腦重新開機之前，安裝更新之後經過的時間量。

#### <a name="automatic-updates-detection-frequency"></a>自動更新偵測頻率
指定 Windows 用來判斷檢查可用更新之前的等待時間，以小時為單位。 確實等待時間的決定方式，是使用這裡指定的時數減去所指定時數的 0 到 20%。 例如，如果此原則用來指定20小時的偵測頻率，套用此原則的所有用戶端都會在16到20小時的任何時間檢查更新。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 必須啟用 [指定近端內部網路 Microsoft 更新服務的位置] 設定，此原則才會生效。
>
> 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

> [!NOTE]
> Windows RT 不支援此原則。 啟用此原則對執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定 Windows 將會在預設的22小時間隔內檢查可用的更新。|
|**已啟用**|指定 Windows 將會在指定的間隔檢查是否有可用的更新。|
|**已停用**|指定 Windows 將會在預設的22小時間隔內檢查可用的更新。|

**選項：** 如果啟用此設定，您可以指定 Windows Update 在檢查更新之前等待的時間間隔（以小時為單位）。

#### <a name="configure-automatic-updates"></a>設定自動更新
指定是否要在這部電腦上啟用自動更新。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

啟用時，您必須選取此群組原則設定中提供的四個選項之一。

若要使用此設定，請選取 [**已啟用**]，然後在 [**設定自動更新**] 底下的 [**選項**] 中，選取其中一個選項（2、3、4或5）。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定在群組原則層級未指定自動更新的使用。 不過，電腦系統管理員仍然可以在 [控制台] 中設定自動更新。|
|**已啟用**|指定 Windows 可辨識電腦何時上線，並使用其網際網路連線搜尋 Windows Update 是否有可用的更新。<br /><br />啟用時，會允許本機系統管理員使用 Windows Update 控制台來選取其選擇的設定選項。 不過，不允許本機系統管理員停用自動更新的設定。<br /><br />-   **2-通知下載並通知安裝**<br />    當 Windows Update 找到適用于電腦的更新時，系統會通知使用者更新已準備就緒可供下載。 然後，使用者可以執行 Windows Update 下載並安裝任何可用的更新。<br />-   **3-自動下載並通知安裝**（預設設定）<br />    Windows Update 會尋找適用的更新，並在背景下載它們。在程式期間，使用者不會收到通知或中斷。 當下載完成時，系統會通知使用者已準備好要安裝的更新。 然後，使用者可以執行 Windows Update 以安裝下載的更新。<br />-   **4-自動下載和排程安裝**<br />    您可以使用此群組原則設定中的選項來指定排程。 如果未指定排程，所有安裝的預設排程會在每天上午3:00 如果有任何更新需要重新開機才能完成安裝，Windows 將會自動重新開機電腦。 （如果使用者在 Windows 準備好重新開機時已登入電腦，則會通知使用者，並提供延遲重新開機的選項。）**注意：** 啟動 Windows 8，您可以將更新設定為在自動維護期間安裝，而不是使用與 Windows Update 系結的特定排程。 自動維護會在電腦未使用時安裝更新，並避免在電腦以電池電源執行時安裝更新。 如果自動維護在天內無法安裝更新，Windows Update 會立即安裝更新。 然後，系統會通知使用者有關暫止的重新開機。 只有在沒有意外資料遺失的情況下，才會進行擱置中的重新開機。    您可以在 [GPME 維護排程器] 設定（位於路徑、 *PolicyName* > **電腦**設定  >  個**原則** > **系統管理範本** >  **）中指定排程選項。Windows 元件** > **維護**排程器 1**自動維護啟用界限**。 請參閱本參考的章節，其標題為：[維護](#computer-configuration--maintenance-scheduler-policy-settings)排程器設定，用於設定詳細資料。    **5-允許本機系統管理員選擇設定**<br />-指定是否允許本機系統管理員使用自動更新控制台來選取其選擇的設定選項，例如本機系統管理員是否可以選擇排程的安裝時間。<br />    本機系統管理員不可停用自動更新的設定。|
|**已停用**|指定可從公用 Windows Update 服務取得的任何用戶端更新，都必須從網際網路手動下載並安裝。|

#### <a name="delay-restart-for-scheduled-installations"></a>延遲重新開機已排程的安裝
指定在繼續執行排程的重新開機之前，自動更新會等待的時間量。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 只有當自動更新設定為執行更新的排程安裝時，才會套用此原則。 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定安裝更新之後，在任何排定的重新開機發生之前，會經過15分鐘的預設等待時間。|
|**已啟用**|指定當安裝完成時，已排程的重新開機會在指定的分鐘數過期後發生。|
|**已停用**|指定安裝更新之後，在任何排定的重新開機發生之前，會經過15分鐘的預設等待時間。|

**選項：** 如果啟用此設定，您可以使用此選項來指定在繼續執行排程的重新開機之前，自動更新等待的時間量（以分鐘為單位）。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>請勿調整預設選項來安裝更新，並在 [關閉 Windows] 對話方塊中關閉
此原則設定可讓您指定是否允許 [**安裝更新和關閉**] 選項做為 [**關閉 Windows** ] 對話方塊中的預設選項。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果*PolicyName*@no__t 1**電腦**設定  >  個**原則** > **系統管理範本** > **Windows 元件** > **Windows Update**@no_，此原則設定就不會有任何影響。_t-11 **[關閉 Windows] 對話方塊原則設定中不會顯示 [安裝更新並關機] 選項**。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 將會是 [**關閉 Windows** ] 對話方塊中的預設選項。|
|**已啟用**|如果您啟用此原則設定，使用者的上次關機選擇（例如 [休眠] 或 [重新開機]）是 [**關閉 Windows** ] 對話方塊中的預設選項，不論是否有 [**安裝更新] 和 [關機**] 選項。**您要電腦執行什麼動作？** 下拉式功能表.|
|**已停用**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 將會是 [**關閉 Windows** ] 對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>不連接到任何 Windows Update 網際網路位置
即使 Windows Update 設定為接收來自內部網路更新服務的更新，它還是會定期從公用 Windows Update 服務取得資訊，以啟用未來與 Windows Update 和其他服務的連線，例如 Microsoft Update或 Microsoft Store。

啟用此原則將會停用從公用 Windows Server Update 服務定期取得資訊的功能，而且可能會導致與公用服務（例如 Microsoft Store）的連線停止運作。

|支援于：|排除|
|---------|-------|
|從 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8.1 開始，仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 只有在電腦設定為使用 [指定內部網路 Microsoft 更新服務位置] 原則設定連線到內部網路更新服務時，才會套用此原則。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|從公用 Windows Server 更新服務取得資訊的預設行為會持續存在。|
|**已啟用**|指定電腦不會從公用 Windows Server 更新服務取得資訊。|
|**已停用**|從公用 Windows Server 更新服務取得資訊的預設行為會持續存在。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新] 和 [關閉] 選項
指定 [**安裝更新] 和 [關機**] 選項是否會顯示在 [**關閉 Windows** ] 對話方塊中。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定當使用者選取 [關機] 選項以關閉電腦時，[**關閉 Windows** ] 對話方塊中的 [**安裝更新] 和 [關機**] 選項可供使用。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定 [**安裝更新] 和 [關機**] 不會顯示為 [**關閉 Windows** ] 對話方塊中的選項，即使當使用者選取 [關機] 選項來關閉電腦時，也會有可用的更新可供安裝。|
|**已停用**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 選項將會是 [**關閉 Windows** ] 對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="enable-client-side-targeting"></a>啟用用戶端目標
指定在 WSUS 主控台中設定用來從 WSUS 接收更新的目標群組名或名稱。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 只有在此電腦設定為支援 WSUS 中指定的目標群組名稱時，才會套用此原則。 如果目標群組名稱不存在於 WSUS 中，將會被忽略，直到建立它為止。 如果 [指定近端內部網路 Microsoft 更新服務的位置] 原則設定已停用或未設定，則此原則不會有任何作用。

> [!NOTE]
> Windows RT 不支援此原則。 啟用此原則對執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定不將目標群組資訊傳送至 WSUS。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定將指定的目標群組資訊傳送至 WSUS，以使用它來判斷應該將哪些更新部署到這部電腦。 如果 WSUS 支援多個目標群組，您可以使用此原則來指定多個組名（以分號分隔），如果您已在 WSUS 的 [電腦群組] 清單中新增目標群組名稱。 否則必須指定單一群組。|
|**已停用**|指定不將目標群組資訊傳送至 WSUS。|

**選項：** 使用此空間來指定一或多個目標群組名。

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>啟用 Windows Update 電源管理，自動喚醒電腦以安裝排程的更新
指定如果有排程安裝的更新，Windows Update 是否會使用 Windows 電源管理或電源選項功能，自動喚醒電腦。

只有在 Windows Update 設定為自動安裝更新時，電腦才會自動喚醒。 如果在排定的安裝時間發生時電腦處於休眠狀態，而且有要套用的更新，Windows Update 將會使用 Windows 電源管理或電源選項功能，自動喚醒電腦以安裝更新。 Windows Update 也會喚醒電腦，並在安裝期限發生時安裝更新。

除非有要安裝的更新，否則電腦不會喚醒。 如果電腦使用電池電源，當 Windows Update 醒來時，就不會安裝更新，而且電腦會在兩分鐘內自動返回休眠狀態。

|支援于：|排除|
|---------|-------|
|從 Windows Vista 和 Windows Server 2008 （Windows 7）開始，仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|Windows Update 不會將電腦從休眠狀態喚醒以安裝更新。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|Windows Update 從休眠狀態喚醒電腦，以在先前列出的條件下安裝更新。|
|**已停用**|Windows Update 不會將電腦從休眠狀態喚醒以安裝更新。|

**選項：** 此設定沒有任何選項。

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>有使用者登入時不自動重新開機以完成排定的自動更新安裝
指定完成排程的安裝時，自動更新會等待登入的任何使用者重新開機電腦，而不是造成電腦自動重新開機。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 只有當自動更新設定為執行更新的排程安裝時，才會套用此原則。 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定自動更新將會通知使用者，電腦會在五分鐘內自動重新開機以完成安裝。|
|**已啟用**|有些更新需要重新開機電腦，更新才會生效。 如果狀態設定為 [已啟用]，當使用者登入電腦時，自動更新不會在排定的安裝期間自動重新開機電腦。 相反地，自動更新會通知使用者重新開機電腦。|
|**已停用**|指定自動更新將會通知使用者，電腦會在五分鐘內自動重新開機以完成安裝。|

**選項：** 此設定沒有任何選項。

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>再次提示排程安裝所需的重新啟動
指定在使用排定的重新開機再次提示之前，自動更新等待的時間量。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!IMPORTANT]
> 只有當自動更新設定為執行更新的排程安裝時，才會套用此原則。 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

> [!NOTE]
> 此原則不會影響執行 Windows RT 的電腦。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|排定的重新開機會在提示重新開機訊息關閉後十分鐘發生。 本機系統管理員可以使用本機原則來變更此設定。|
|**已啟用**|指定在上一個提示重新開機之後延遲之後，排程的重新開機會在經過指定的分鐘數之後發生。|
|**已停用**|排定的重新開機會在提示重新開機訊息關閉後十分鐘發生。|

**選項：** 啟用時，您可以使用此設定選項來指定再次提示使用者關於排定的重新開機之前經過的時間長度（以分鐘為單位）。

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>重新排程已經排程好的自動更新安裝
指定在繼續進行先前錯過的排程安裝之前，自動更新在電腦啟動之後等候的時間量。

如果狀態設為 [**未**設定]，則會在下次啟動電腦後一分鐘執行已錯過的排程安裝。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 只有當自動更新設定為執行更新的排程安裝時，才會套用此原則。 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定在電腦下一次啟動之後，錯過排程的安裝將會發生一分鐘。|
|**已啟用**|指定先前未進行的排程安裝，會在電腦下次啟動後的指定分鐘數發生。|
|**已停用**|指定在下一次排程安裝時，將會發生遺失的排程安裝。|

**選項：** 啟用此原則設定時，您可以使用它來指定下次啟動電腦之後的分鐘數，這會發生先前未進行的排程安裝。

#### <a name="specify-intranet-microsoft-update-service-location"></a>指定近端內部網路 Microsoft 更新服務位置
指定託管來自 Microsoft Update 之更新的內部伺服器。 接著，您可以使用 WSUS 自動更新網路上的電腦。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|Windows RT。|

此設定可讓您在網路上指定將做為內部更新服務運作的 WSUS 伺服器。 WSUS 用戶端不會在網際網路上使用 Microsoft Updates，而是會搜尋此服務是否有適用的更新。

若要使用此設定，您必須設定兩個伺服器名稱值：用戶端偵測並下載更新的伺服器，以及更新的工作站上傳統計資料的伺服器。 如果兩個服務都設定在同一部伺服器上，則值不需要不同。

> [!NOTE]
> 如果 [設定自動更新] 原則設定已停用，則此原則不會有任何作用。

> [!NOTE]
> Windows RT 不支援此原則。 啟用此原則對執行 Windows RT 的電腦不會有任何影響。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|如果原則或使用者喜好設定未停用自動更新，此原則會指定用戶端直接連接到網際網路上的 Windows Update 網站。|
|**已啟用**|指定用戶端連接到指定的 WSUS 伺服器，而不是 Windows Update，以搜尋和下載更新。 啟用這項設定表示貴組織中的使用者不需要通過防火牆即可取得更新，並讓您有機會在部署更新之前先進行測試。|
|**已停用**|如果原則或使用者喜好設定未停用自動更新，此原則會指定用戶端直接連接到網際網路上的 Windows Update 網站。|

**選項：** 啟用此原則設定時，您必須指定 WSUS 用戶端在偵測更新時將使用的內部網路更新服務，以及更新的 WSUS 用戶端上傳統計資料的目標網際網路統計伺服器。 範例值：


|                    設定選項：                    |    範例值：    |
|-------------------------------------------------------|----------------------|
| 設定偵測更新的近端內部網路更新服務 |  http://wsus01:8530  |
|          設定內部網路統計伺服器           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>透過自動更新開啟建議的更新
指定自動更新是否會從 WSUS 提供重要和建議的更新。

|支援于：|排除|
|---------|-------|
|從 Windows Vista 開始，仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定自動更新會繼續傳遞重要的更新（如果已設定的話）。|
|**已啟用**|指定自動更新將會從 WSUS 安裝建議的更新和重要更新。|
|**已停用**|指定自動更新會繼續傳遞重要的更新（如果已設定的話）。|

**選項：** 此設定沒有任何選項。

#### <a name="turn-on-software-notifications"></a>開啟軟體通知
此原則設定可讓您控制使用者是否會看到來自 Microsoft Update 服務之精選軟體的詳細增強通知訊息。 增強的通知訊息會傳達價值，並升級選用軟體的安裝和使用。 此原則設定適用于鬆散管理的環境，您可以在其中允許使用者存取 Microsoft Update 服務。

如果您不是使用 Microsoft Update 服務，[軟體通知] 原則設定就不會有任何作用。

如果 [設定自動更新] 原則設定已停用或未設定，[軟體通知] 原則設定就不會有任何作用。

|支援于：|排除|
|---------|-------|
|從 Windows Server 2008 （Windows Vista）和 Windows 7 開始，仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此原則設定預設為停用。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|在執行 Windows 7 的電腦上的使用者，不會提供選用應用程式的訊息。 在執行 Windows Vista 的電腦上，不會提供選用應用程式或更新的訊息給使用者。 本機系統管理員可以使用 [控制台] 或本機原則來變更此設定。|
|**已啟用**|如果您啟用此原則設定，當精選軟體可供使用時，使用者的電腦上將會出現通知訊息。 使用者可以按一下通知開啟 Windows Update，並取得有關軟體的詳細資訊或安裝它。 使用者也可以按一下 [**關閉此訊息**] 或 [**稍後顯示**]，以適當地延遲通知。<br /><br />在 Windows 7 中，此原則設定只會控制選擇性應用程式的詳細通知。 在 Windows Vista 中，此原則設定可控制選用應用程式和更新的詳細通知。|
|**已停用**|指定執行 Windows 7 的使用者將不會針對選用的應用程式提供詳細的通知訊息，而執行 Windows Vista 的使用者將不會針對選用的應用程式或選用的更新提供詳細的通知訊息。|

**選項：** 此設定沒有任何選項。

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>電腦設定 > 維護排程器原則設定
在 [設定自動更新] 設定中，您已選取 [ **4-自動下載和排程安裝**] 選項，您可以在 GPMC 中為執行 Windows 8 和 Windows RT 的電腦指定排程維護排程器設定。 如果您未在 [設定自動更新] 設定中選取選項4，就不需要針對自動更新的目的來設定這些設定。 維護排程器設定位於下列路徑：*PolicyName* >  部**電腦**設定  >  個**原則** > **系統管理範本** >  個**Windows 元件** >  個**維護**排程器。 群組原則的維護排程器延伸模組包含下列設定：

-   [自動維護啟用界限](#automatic-maintenance-activation-boundary)

-   [自動維護隨機延遲](#automatic-maintenance-random-delay)

-   [自動喚醒原則](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>自動維護啟用界限
此原則可讓您設定「自動維護啟用界限」設定。

維護啟用界限是自動維護開始的每日排程時間。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 這項設定與 [**設定自動更新**] 中的選項4有關。 如果您未在 [**設定自動更新**] 中選取選項4，則不需要進行此設定。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|如果未設定此原則設定，則會套用 [**行動中心**] 中用戶端電腦上所指定的每日排程時間  >  個**自動維護**控制台。|
|**已啟用**|啟用此原則設定會覆寫 [**控制台**] 中用戶端電腦上設定的任何預設或已修改設定  >  [**動作中心**] [@no__t 3**自動維護**] （或在某些用戶端版本中，[**維護**]）。|
|**已停用**|如果您將此原則設定設為 [**停用**]，則會套用 [控制台] 中所指定的每日排程**時間 @no__t-** 2**自動維護**。|

#### <a name="automatic-maintenance-random-delay"></a>自動維護隨機延遲
此原則設定可讓您設定自動維護啟用隨機延遲。

「維護隨機延遲」是自動維護從其啟用界限開始延遲的時間量。 這項設定適用于隨機維護可能是效能需求的虛擬機器。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 這項設定與 [**設定自動更新**] 中的選項4有關。 如果您未在 [**設定自動更新**] 中選取選項4，則不需要進行此設定。

根據預設，啟用時，週期性維護隨機延遲會設定為**PT4H**。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|四小時的隨機延遲會套用至**自動**。|
|**已啟用**|自動維護將從其啟用界限開始延遲到指定的時間量。|
|**已停用**|自動維護不會套用隨機延遲。|

#### <a name="automatic-wakeup-policy"></a>自動喚醒原則
此原則設定可讓您設定自動維護喚醒原則。

維護喚醒原則會指定自動維護是否應該對作業系統進行每日排程維護的喚醒要求。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果已明確停用操作電腦的電源喚醒原則，則此設定不會有任何作用。

> [!NOTE]
> 這項設定與 [**設定自動更新**] 中的選項4有關。 如果您未在 [**設定自動更新**] 中選取選項4，則不需要進行此設定。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|如果您未設定此原則設定，則會套用 [**動作中心**] 中所指定的喚醒設定  >  [**自動維護**] 控制台。|
|**已啟用**|如果您啟用此原則設定，自動維護將會嘗試設定作業系統喚醒原則，並在需要時對每日排定的時間提出喚醒要求。|
|**已停用**|如果您停用此原則設定，則會套用 [**動作中心**] 中所指定的喚醒設定  >  [**自動維護**] 控制台。|

### <a name="user-configuration--windows-update-policy-settings"></a>> Windows Update 原則設定的使用者配置
本節提供下列以使用者為基礎的原則設定詳細資料：

-   [不會在關閉關閉 Windows 對話方塊中顯示 安裝更新並關機 選項](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [請勿在 [關閉 Windows] 對話方塊中，將預設選項調整為 [安裝更新並關機]](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [移除存取權以使用所有 Windows Update 功能](#remove-access-to-use-all-windows-update-features)

在 GPMC 中，自動電腦更新的使用者設定位於下列路徑：*PolicyName* >  個**使用者**設定  >  個**原則** > **系統管理範本** >  個**Windows 元件** > **Windows Update**。 當選取 [Windows Update 原則] 的 [**設定**] 索引標籤以字母順序排序設定時，這些設定的順序會與 [電腦設定] 和 [使用者設定延伸模組] 中的群組原則相同。

> [!NOTE]
> 根據預設，除非另有注明，否則不會設定這些設定。

> [!TIP]
> 針對每個設定，您可以使用下列步驟來啟用、停用或在設定之間流覽：

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>不要在 [關閉 Windows] 對話方塊中顯示 [安裝更新並關機] 選項
指定 [**安裝更新] 和 [關機**] 選項是否會顯示在 [**關閉 Windows** ] 對話方塊中。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 選項會出現在 [**關閉 Windows** ] 對話方塊中。|
|**已啟用**|如果您啟用此原則設定，當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 將不會顯示為 [**關閉 Windows** ] 對話方塊中的選項。|
|**已停用**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 選項會出現在 [**關閉 Windows** ] 對話方塊中。|

**選項：** 此設定沒有任何選項。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>請勿在 [關閉 Windows] 對話方塊中，將預設選項調整為 [安裝更新並關機]
指定是否允許 [**安裝更新] 和 [關機**] 選項做為 [**關閉 Windows** ] 對話方塊中的預設選項。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果*PolicyName* >  個**使用者**設定  >  個**原則** > **系統管理範本** > **Windows 元件** > **Windows Update**，此原則設定就不會有任何影響 @no__ **[關閉 Windows] 對話方塊中的 [t-11] 不會顯示 [安裝更新並關機] 選項**。

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 選項是否會成為 [**關閉 Windows** ] 對話方塊中的預設選項。|
|**已啟用**|指定使用者的上次關機選擇（例如 [休眠] 或 [重新開機]）是否為 [**關閉 Windows** ] 對話方塊中的預設選項，不論 [**安裝更新] 和 [關機] 選項**是否都可在 [**您要如何]要讓電腦執行嗎？** 下拉式功能表.|
|**已停用**|指定當使用者選取 [關機] 選項以關閉電腦時，[**安裝更新] 和 [關機**] 選項是否會成為 [**關閉 Windows** ] 對話方塊中的預設選項。|

**選項：** 此設定沒有任何選項。

#### <a name="remove-access-to-use-all-windows-update-features"></a>移除對使用所有 Windows Update 功能的存取
此設定可讓您移除 Windows Update 的 WSUS 用戶端存取權。

|支援于：|排除|
|---------|-------|
|仍在其 Microsoft 產品內的 Windows 作業系統[支援生命週期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**原則設定狀態**|**表現**|
|**未設定**|使用者可以連接到 Windows Update 網站。|
|**已啟用**|**重要：** 如果啟用，則會移除所有 Windows Update 功能。 這包括封鎖在 Windows Update 網站的存取權 [https://windowsupdate.microsoft.com](https://windowsupdate.microsoft.com )，從開始功能表 或 [開始] 畫面上，同時也會在 Windows Update hyperlink**工具**Internet Explorer 中的功能表。 Windows 自動更新也已停用;系統不會通知使用者，也不會收到來自 Windows Update 的重大更新。 此設定也會防止 Device Manager 從 Windows Update 網站自動安裝驅動程式更新。<br /><br />啟用時，您可以設定下列其中一個通知選項：<br /><br />-   **0-不顯示任何通知**<br />    此設定將會移除 Windows Update 功能的所有存取權，而且不會顯示任何通知。<br />-   **1-顯示 [需要重新開機] 通知**<br />    此設定會顯示完成安裝所需的重新開機通知。 **注意：** 在執行 Windows 8 和 Windows RT 的電腦上，如果已啟用此原則，則只會顯示與重新開機相關的通知，以及無法偵測到更新的情況。 不支援通知選項。 登入畫面上的通知一律會顯示。|
|**已停用**|使用者可以連接到 Windows Update 網站。|

**選項：** 如需此設定，請參閱資料表中的 [**已啟用**]。

## <a name="supplemental-information"></a>補充資訊
本節提供有關在群組原則中使用開啟和儲存 WSUS 設定的額外資訊，以及本指南中使用之詞彙的定義。 對於熟悉舊版 WSUS （WSUS 3.2 和舊版）的系統管理員，有一份表格會簡短摘要說明 WSUS 版本之間的差異。

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>存取群組原則中的 Windows Update 設定
以下程式說明如何在網域控制站上開啟 GPMC。 此程式接著會說明如何開啟現有的網域層級群組原則物件（GPO）進行編輯，或建立新的網域層級 GPO 並開啟以供編輯。

> [!NOTE]
> 您必須是**Domain Admins**群組的成員或同等許可權，才能執行此程式。

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>若要開啟或加入並開啟群組原則物件

1.  在您的網域控制站上，移至 [**伺服器管理員**]、[>**工具**]、[>**群組原則管理**]。 群組原則管理主控台隨即開啟。

2.  在左窗格中，展開您的樹系。 例如，按兩下 [樹系 **： example.com**]。

3.  在左窗格中，按兩下 [**網域**]，然後按兩下您要管理群組原則物件的網域。 例如，按兩下 [ **example.com**]。

4.  執行下列其中一項：

    -  **若要開啟現有的網域層級 GPO 進行編輯**，請按兩下包含您要管理之群組原則物件的網域，以滑鼠右鍵按一下您要管理的網域原則，然後按一下 [**編輯**]。 群組原則管理編輯器（GPME）隨即開啟。

    -  **若要建立新的群組原則物件並開啟以供編輯**：
        1.  在您要建立新群組原則物件的網域上按一下滑鼠右鍵，然後按一下 [**在這個網域中建立 GPO 並連結到**]。

        2.  在 [**新增 GPO**] 的 [**名稱**] 中，輸入新群組原則物件的名稱，然後按一下 **[確定]** 。

        3.  以滑鼠右鍵按一下新的群組原則物件，然後按一下 [**編輯**]。 GPME 隨即開啟。

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>若要開啟群組原則的 Windows Update 或維護排程器延伸模組

1.  在群組原則管理編輯器 中，執行下列其中一項動作：

    -   **開啟 [電腦設定] > 群組原則的 Windows Update 延伸**模組。 瀏覽至：*PolicyName* >  部**電腦**設定  >  個**原則** / **系統管理範本** >  個**Windows 元件** > **Windows Update**。

    -   **開啟群組原則的 使用者設定 > Windows Update 延伸**模組。 瀏覽至：*PolicyName* >  個**使用者**設定  >  個**原則** > **系統管理範本** >  個**Windows 元件** > **Windows Update**。

    -   **開啟 [電腦設定] > [維護**] 排程器延伸模組群組原則。 在 GPOE 中，流覽至*PolicyName* >  部**電腦**設定  >  個**原則** > **系統管理範本** >  個**Windows 元件** >  個**維護**排程器。

如需群組原則的詳細資訊，請參閱[群組原則總覽](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12))。

> [!TIP]
> 開啟您想要的群組原則擴充功能之後，您可以使用下列步驟來啟用、停用或在設定之間流覽：

##### <a name="to-configure-group-policy-settings"></a>設定群組原則設定

1.  在 [ *ExtensionOfGroupPolicy*] 中，按兩下您要查看或修改的設定。

2.  若要進行此設定，請執行下列其中一項動作：

    -   若要保留設定的預設未指定狀態，請選取 [**未設定**]。

    -   若要啟用此設定，請選取 [**已啟用**]。

    -   若要停用此設定，請選取 [**停用**]。

3.  在 [**選項**] 中，如果有列出任何選項，請保留預設值，或視需要加以修改。

4.  執行下列其中一項：

    -   若要儲存您的變更並繼續進行下一個設定，**請按一下 [** 套用]，然後按 **[下一步] 設定**。

    -   若要儲存變更並關閉對話方塊，請按一下 **[確定]** 。

    -   若要捨棄所有未儲存的變更並關閉對話方塊，請按一下 [**取消**]。

### <a name="changes-to-wsus-relevant-to-this-guide"></a>與本指南相關的 WSUS 變更
下表摘要說明與本指南相關的目前和過去版本的 WSUS 之間的主要差異。

|Windows Server 和 WSUS 版本|描述|
|------------------|--------|
| Windows Server 2012 R2 （含 WSUS 6.0）和後續版本|從 Windows Server 2012 開始，WSUS 伺服器角色會與作業系統整合，而 WSUS 用戶端的相關群組原則設定預設會包含在群組原則中。|
| Windows Server 2008 （和舊版的 Windows Server）搭配 WSUS 3.2 和更早版本|在 Windows Server 2008 （和舊版的 Windows Server）中，使用 WSUS 3.2 版（和更早版本），管理 WSUS 用戶端的群組原則設定並不包含在這些 Windows Server 作業系統中。 原則設定位於 WSUS 系統管理範本（ **wuau**）中。 在這些伺服器版本中，必須先將 WSUS 系統管理範本新增到群組原則管理主控台（GPMC）中，才能設定 WSUS 用戶端設定。|

### <a name="terms-and-definitions"></a>詞彙和定義
以下是本指南中使用的字詞清單。

|詞彙|定義|
|----|-------|
|自動更新|**在 Windows 電腦上執行的服務**（自動更新）：指的是內建在 Microsoft Windows Vista、Windows Server 2003、Windows XP 和 Windows 2000 （含 SP3）作業系統的用戶端電腦群組件，以從 Microsoft Update 或 Windows Update 取得更新。<br /><br />一般**參考**（自動更新）：用來描述 Windows Update 代理程式何時會自動排程及下載更新的詞彙。|
|自發伺服器|使用來參照下游 Windows Server Update Services （WSUS）伺服器，系統管理員可以在其上管理 WSUS 元件。|
|下游伺服器|使用來參考 Windows Server Update Services （WSUS）伺服器，以從另一個 WSUS 伺服器取得更新，而不是從 Microsoft Update 或 Windows Update。|
|群組原則延伸模組（和：群組原則的延伸模組|群組原則中的設定集合，可用來控制使用者和電腦（套用原則的方式）如何設定及使用各種 Windows 服務和功能。 系統管理員可以使用 WSUS 搭配群組原則進行自動更新用戶端的用戶端設定，以協助確保使用者無法停用或規避公司更新原則。<br /><br />WSUS 不需要使用 active directory 或群組原則。 您也可以使用本機群組原則或修改 Windows 登錄來套用用戶端設定。|
|內部更新服務|使用一或多個 WSUS 伺服器散發更新之網路基礎結構的一般參考。|
|複本伺服器|使用來參照下游 Windows Server Update Services （WSUS）伺服器，以鏡像其所連線之上游伺服器上的核准和設定。 您無法管理複本伺服器上的 WSUS。|
|Microsoft Update|**以網際網路為基礎的 Microsoft 下載網站：** 儲存和散發 Windows 電腦（設備磁碟機）、Windows 作業系統和其他 Microsoft 軟體產品更新的 Microsoft 網際網路網站。|
|軟體更新服務（SUS）|SUS 是 Windows Server Update Services （WSUS）的前身產品。|
|[更新]|任何一組軟體修訂、修補程式、service pack、feature pack 和設備磁碟機，都可以安裝在電腦上以擴充功能，或改善效能和安全性。|
|更新檔案|在電腦上安裝更新所需的檔案。|
|更新資訊（也稱為更新中繼資料）|更新的相關資訊，而不是更新套件中的更新二進位檔案。 例如，中繼資料提供更新內容的資訊，可讓您找出更新的用途。 中繼資料也包含 Microsoft 軟體授權條款。 下載更新的中繼資料封裝通常遠小於實際的更新檔案封裝。|
|更新來源|Windows Server Update Services （WSUS）伺服器同步處理以取得更新檔案的位置。 此位置可以是 Microsoft Update 或上游 WSUS 伺服器。|
|上游伺服器|Windows Server Update Services （WSUS）伺服器，可將更新檔案提供給另一部 WSUS 伺服器，而後者又稱為下游伺服器。|
|Windows Server Update Services (WSUS)|在公司網路上的一部或多部 Windows Server 電腦上執行的伺服器角色程式。 WSUS 基礎結構可讓您管理網路上要安裝之電腦的更新。<br /><br />您可以使用 WSUS 來核准或拒絕發行前的更新，以強制更新在指定的日期進行安裝，以及取得有關網路上每一部電腦所需更新的詳細報表。 您可以將 WSUS 設定為自動核准特定類別的更新（重大更新、安全性更新、service pack、驅動程式等）。 WSUS 也可讓您只核准「偵測」的更新，如此一來，您就可以查看哪些電腦需要指定的更新，而不需安裝更新。<br /><br />在 WSUS 的執行中，網路中至少要有一部 WSUS 伺服器能夠連線到 Microsoft Update 以取得可用的更新。 系統管理員可以根據網路安全性和設定，判斷有多少其他伺服器直接連接到 Microsoft Update。<br /><br />您可以設定 WSUS 伺服器，透過網際網路從下列位置取得更新：<br /><br />-公用 Microsoft Update<br />-公用 Windows Update<br />-Microsoft Store|
|Windows Update|**以網際網路為基礎的 Microsoft 下載網站：** 儲存和散發 Windows 電腦（設備磁碟機）和 Windows 作業系統更新的 Microsoft 網際網路網站。<br /><br />**電腦服務：** 在電腦上執行的 Windows Update 服務名稱。 Windows Update 會在 Windows 電腦上偵測、下載及安裝更新。<br /><br />視電腦和原則設定而定，Windows Update 代理程式可以從下列來源下載更新：<br /><br />-Microsoft Update<br />-Windows Update<br />-Microsoft Store<br />-網際網路（網路）更新服務（WSUS）<br /><br />在以 WSUS 為基礎的環境中未受管理的電腦，通常會使用 Windows Update 直接透過網際網路連線 Windows Update、Microsoft Update 或 Microsoft Store 來取得更新。|
|WSUS 用戶端|從 WSUS 內部網路更新服務接收更新的電腦。<br /><br />如果群組原則設定可控制使用者與自動更新的互動： WSUS 環境中電腦的使用者。|
