---
description: '深入瞭解：租使用者的受防護 Vm-建立範本磁片 (選用) '
title: 租使用者的受防護 Vm-建立範本磁片-選用
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 610b0a8d91564249ca03f57e8957bbe85770b74d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043788"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>租使用者的受防護 Vm-建立範本磁片 (選用) 

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

若要建立新的受防護 VM，您將需要使用特別備妥的已簽署範本磁片。 來自已簽署之範本磁片的中繼資料可協助確保磁片在建立之後不會修改，並可讓您作為租使用者，以限制可使用哪些磁片來建立受防護的 Vm。 提供此磁片的其中一種方式是讓您（租使用者）建立此磁片，如本主題中所述。

> [!IMPORTANT]
> 如果您想要的話，可以改為使用裝載服務提供者所提供的範本磁片。 如果您這樣做，請務必使用該範本磁片來部署測試 VM，並執行您自己的工具 (的防毒軟體、弱點掃描程式等) 來驗證磁片實際上是在您信任的狀態。

## <a name="prepare-an-operating-system-vhdx"></a>準備作業系統 VHDX

若要建立受防護的範本磁片，您必須先準備將透過範本磁片嚮導執行的 OS 磁片。 此磁片將用來作為受防護 Vm 中的 OS 磁片。 您可以使用任何現有的工具來建立此磁片，例如 Microsoft 桌面映射 Service Manager (DISM) ，或手動設定具有空白 VHDX 的 VM，並將作業系統安裝到該磁片。 設定磁片時，必須遵守第2代和/或受防護 Vm 的特定需求：

| VHDX 的需求 | 原因 |
|-----------|----|
|必須是 (GPT) 磁片的 GUID 磁碟分割表格 | 第2代虛擬機器需要支援 UEFI|
|磁片類型必須是「 **基本** 」，而非「 **動態**」。 <br>注意：這是指邏輯磁片類型，而不是 Hyper-v 支援的「動態擴充」 VHDX 功能。 | BitLocker 不支援動態磁碟。|
|磁片至少有兩個磁碟分割。 一個磁碟分割必須包含安裝 Windows 的磁片磁碟機。 此為 BitLocker 將會加密的磁碟機。 另一個磁碟分割是使用中的磁碟分割，其中包含開機載入器並保持未加密狀態，讓電腦可以啟動。|BitLocker 所需|
|檔案系統是 NTFS | BitLocker 所需|
|VHDX 上安裝的作業系統是下列其中一項：<br>-Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10、Windows 8.1、Windows 8| 支援第2代虛擬機器和 Microsoft 安全開機範本所需|
|作業系統必須一般化 (執行 sysprep.exe)  | 範本布建牽涉到專用 Vm 的特定租使用者工作負載|

> [!NOTE]
> 請勿在此階段將範本磁碟複製到 VMM 程式庫。

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>建立 Nano 伺服器範本磁片所需的套件

如果您打算以受防護的 Vm 中的來賓 OS 形式執行 Nano 伺服器，您必須確定 Nano Server 映射包含下列套件：

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>在範本作業系統上執行 Windows Update

在範本磁片上，確認作業系統已安裝所有最新的 Windows 更新。 最近發行的更新改善了端對端防護程式的可靠性，也就是如果範本作業系統不是最新版本，可能無法完成的進程。

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>使用範本磁片 wizard 簽署和保護 VHDX

若要使用包含受防護 Vm 的範本磁片，必須使用 BitLocker 簽署和加密磁片。 若要這樣做，您將使用受防護的範本磁片建立嚮導。 此 wizard 將會產生磁片的雜湊，並將其新增至磁片區簽章目錄， (VSC) 。 VSC 會使用您指定的憑證進行簽署，並在布建程式期間使用，以確保為租使用者所部署的磁片未被變更或取代為租使用者不信任的磁片。 最後，BitLocker 會安裝在磁片的作業系統 (如果) 尚未安裝，請在 VM 布建期間將磁片準備好進行加密。

> [!NOTE]
> 範本磁片嚮導將會修改您就地指定的範本磁片。 您可能會想要在執行嚮導之後，先複製未受保護的 VHDX，稍後再進行磁片更新。 您將無法修改使用範本磁片嚮導保護的磁片。

在執行 Windows Server 2016 (不需要是受防護主機或 VMM 伺服器) 的電腦上執行下列步驟：

1. 將在「 [準備作業系統 VHDX](#prepare-an-operating-system-vhdx) 」中建立的一般化 VHDX 複製到伺服器（如果尚未存在）。

2. 從機器上的 **遠端伺服器管理工具** 安裝 **受防護的 VM 工具** 功能。

    ```
    Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
    ```

3. 取得或建立憑證來簽署將成為新受防護 Vm 之範本磁片的 VHDX。 此憑證的詳細資料將會併入防護資料檔案中，而此檔案會將磁片授權為受信任的磁片。 因此，請務必從您和主機服務提供者信任的憑證授權單位單位取得此憑證。 在您同時是主機服務提供者和租使用者的企業案例中，您可以考慮從您的 PKI 發行此憑證。

    如果您要設定測試環境，而只想要使用自我簽署憑證來簽署您的範本磁片，請在您的電腦上執行類似下列的命令：

    ```
    New-SelfSignedCertificate -DnsName publisher.fabrikam.com
    ```

4. 從 [開始] 功能表上的 [系統 **管理工具**] 資料夾，或在命令提示字元中輸入 **TemplateDiskWizard.exe** 來啟動 **範本磁片 Wizard** 。

5. 在 [ **憑證** ] 頁面上，按一下 **[流覽]** 以顯示憑證清單。 選取用來簽署磁片範本的憑證。 按一下 **[確定]**，然後按 **[下一步]**。

6. 在 [虛擬磁片] 頁面上，按一下 **[流覽]** 以選取您已備妥的 VHDX，然後按 **[下一步]**。

7. 在 [簽章目錄] 頁面上，提供易記的 **磁片名稱** 和 **版本。** 這些欄位會出現，可協助您在磁片經過簽署之後識別該磁片。

    例如，您可以針對 **磁片名稱** 輸入 _WS2016_ 和 _1.0.0.0_ **版**

8. 在嚮導的 [審核設定] 頁面上，檢查您的選擇。 當您按一下 [ **產生**] 時，嚮導會在範本磁片上啟用 BitLocker、計算磁片的雜湊，並建立儲存在 VHDX 中繼資料中的磁片區簽章目錄。

    等到簽署程式完成之後，再嘗試掛接或移動範本磁片。 此程式可能需要一段時間才能完成，視您磁片的大小而定。

9. 在 [ **摘要** ] 頁面上，會顯示磁片範本的相關資訊、用來簽署範本的憑證，以及憑證簽發者。 按一下 [關閉] 結束精靈。


提供受防護的磁片範本給主機服務提供者，以及您建立的防護資料檔案，如 [建立防護資料以定義受防護的 VM](guarded-fabric-tenant-creates-shielding-data.md)所述。

## <a name="additional-references"></a>其他參考資料

- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
