---
title: 受防護的 Vm，可供租用戶-建立範本磁碟-選擇性
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874629"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>受防護的 Vm，可供租用戶-建立範本磁碟 （選擇性）

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

若要建立新的受防護的 VM，您必須使用特殊備妥、 帶正負號的範本磁碟。 從已簽署的範本磁碟的中繼資料可協助確保已建立之後，不會修改磁碟，並可讓您與租用戶，以限制哪些磁碟可用來建立受防護的 Vm。 若要提供此磁碟的方法之一是為您的租用戶，若要建立它，如本主題中所述。 

> [!IMPORTANT]
> 如果想要的話，您可以改為使用裝載的服務提供者所提供的範本磁碟。 如果您這麼做時，務必部署的測試，以驗證磁碟的 VM 使用該範本磁碟，並執行您自己的工具 （例如防毒軟體、 弱點掃描器等等），事實上是，在您信任的狀態。

## <a name="prepare-an-operating-system-vhdx"></a>準備作業系統 VHDX

若要建立受防護的範本磁碟，您需要先準備的 OS 磁碟，將會透過範本磁碟精靈 的方式來執行。 此磁碟將用於為受防護的 Vm 的 OS 磁碟。 您可以使用任何現有的工具來建立此磁碟，例如 Microsoft 桌面映像服務管理員 」 (DISM)，或以手動方式設定具有空白的 VHDX 的 VM 並安裝到該磁碟上的作業系統。 當設定磁碟，它必須遵守下列需求，專屬於第 2 代及/或受防護的 Vm: 

| Vhdx 的需求 | 原因 |
|-----------|----|
|必須是 GUID 磁碟分割表格 (GPT) 磁碟 | 所需的第 2 代虛擬機器，才能支援 UEFI|
|磁碟類型必須是**基本**相對於**動態**。 <br>注意：這是指邏輯磁碟類型，而不"動態擴充的 」 VHDX 支援之功能的 HYPER-V。 | BitLocker 不支援動態磁碟。|
|磁碟會有至少兩個資料分割。 一個磁碟分割必須包含 Windows 安裝所在的磁碟機。 這是 BitLocker 將要加密的磁碟機。 另一個磁碟分割為作用中的磁碟分割，其中包含開機載入器並不會加密，以便可以啟動電腦。|所需的 BitLocker|
|檔案系統是 NTFS | 所需的 BitLocker|
|安裝在 VHDX 上的作業系統可以是下列其中一項：<br>Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 <br>-Windows 10，Windows 8.1，Windows 8| 需要支援第 2 代虛擬機器和 Microsoft 安全開機範本|
|作業系統必須一般化 (執行的 sysprep.exe) | 範本佈建包含專門針對特定租用戶的工作負載的 Vm| 

> [!NOTE]
> 請勿在這個階段中複製到 VMM 程式庫的範本磁碟。 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>必要封裝以建立 Nano Server 的範本磁碟

如果您打算執行 Nano 伺服器作為受防護的 Vm 將客體作業系統，您必須確定您的 Nano Server 映像包含下列封裝：

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>範本作業系統上執行 Windows Update

範本磁碟上，確認作業系統具有所有最新安裝的 Windows 更新。 最近發行的更新，改善端對端防護程序的可靠性-可能無法完成如果範本作業系統的程序不是最新狀態。

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>登入，並保護範本磁碟精靈 」 使用 VHDX

若要使用受防護的 Vm 範本磁碟，磁碟必須簽署並使用 BitLocker 加密。 若要這樣做，您將使用受防護的範本磁碟建立精靈。 此精靈會產生磁碟的雜湊值，並將它新增至磁碟區簽章目錄 (VSC)。 VSC 使用您指定，並在佈建程序期間用來確保要部署的租用戶之磁碟不已改變或取代的磁碟，租用戶不信任的憑證來簽署。 最後，BitLocker 安裝磁碟的作業系統上 （如果它已經不存在） 來準備 VM 佈建期間加密的磁碟。

> [!NOTE]
> 範本磁碟精靈 將會修改您指定位置中的範本磁碟。 若要再執行對磁碟進行更新，稍後在精靈中建立一份未受保護的 VHDX。 您無法修改範本磁碟精靈 與受保護的磁碟。

執行 Windows Server 2016 （不需要是受守護的主機或 VMM 伺服器） 的電腦上執行下列步驟：

1. 複製在中建立一般化的 VHDX[準備作業系統 VHDX](#prepare-an-operating-system-vhdx)到伺服器，如果它已經不存在。

2. 安裝**受防護的 VM 工具**功能**遠端伺服器管理工具**機器上。

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. 取得或建立憑證來簽署將會成為新的受防護 Vm 的範本磁碟的 VHDX。 此憑證的詳細資訊將會合併到授權做為受信任的磁碟磁碟防護資料檔案中。 因此，務必以取得您和您的託管服務，服務提供者信任此憑證從憑證授權單位。 在您所在的主機服務提供者和租用戶的企業案例，您可以考慮從您的 PKI 發行此憑證。

    如果您要設定測試環境，而只想要使用自我簽署的憑證來簽署範本磁碟，請在您的電腦上執行類似下列的命令：

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. 開始**範本磁碟精靈**從**系統管理工具**資料夾，在 [開始] 功能表上，或輸入**TemplateDiskWizard.exe**在命令提示字元。

5. 在 **憑證**頁面上，按一下**瀏覽**顯示一份憑證。 選取用來簽署磁碟範本的憑證。 按一下 **[確定]**，然後按 **[下一步]**。

6. 在虛擬磁碟 頁面上，按一下**瀏覽**，選取您已備妥的 VHDX，然後按一下**下一步**。

7. 在簽章目錄] 頁面上，提供 [易記**磁碟名稱**和**版本。** 這些欄位會出現以協助您識別磁碟，一旦已簽署項目。

    例如，對於**磁碟名稱**您可以輸入_WS2016_至於**版本**， _1.0.0.0_

8. 檢閱您在精靈的 [檢閱設定] 頁面的選項。 當您按一下 **產生**，精靈會的範本磁碟上啟用 BitLocker、 計算的雜湊的磁碟，並建立磁碟區簽章目錄，其儲存於 VHDX 中繼資料。

    等候簽署程序完成之前嘗試掛接或移動的範本磁碟。 此程序可能需要一段時間才能完成，視您的磁碟大小而定。 

9. 在 **摘要**頁面上，磁碟 範本，用來簽署的範本的憑證資訊，而且會顯示憑證簽發者。 按一下 **[關閉]** 以結束精靈。


提供主機服務提供者，以及您建立時，虛擬機器防護資料檔案的受防護的磁碟範本中所述[建立防護資料，以便定義受防護的 VM](guarded-fabric-tenant-creates-shielding-data.md)。

## <a name="see-also"></a>另請參閱

- [部署受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
