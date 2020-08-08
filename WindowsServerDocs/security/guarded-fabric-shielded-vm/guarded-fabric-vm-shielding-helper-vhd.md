---
title: 受防護的 Vm-準備 VM 防護協助程式 VHD
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 2c6a74b3dd18465534a662e6c1afa37ac197aca6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943988"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>受防護的 Vm-準備 VM 防護協助程式 VHD

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

> [!IMPORTANT]
> 開始這些程式之前，請確定您已安裝最新的 Windows Server 2016 累積更新，或使用最新的 Windows 10[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。 否則，程式將無法正常執行。

本節概述主機服務提供者執行的步驟，以啟用將現有 Vm 轉換為受防護 Vm 的支援。

若要瞭解本主題如何適用于部署受防護 Vm 的整體程式，請參閱[主機服務提供者設定步驟中的受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="which-vms-can-be-shielded"></a>哪些 Vm 可以受到防護？

現有 Vm 的防護程式僅適用于符合下列必要條件的 Vm：

- 「虛擬作業系統」是 Windows Server 2012、2012 R2、2016或半年通道版本。 無法將現有的 Linux Vm 轉換成受防護的 Vm。
- VM 是 (UEFI 固件的第2代 VM) 
- VM 不會針對其作業系統磁片區使用差異磁片。

## <a name="prepare-helper-vhd"></a>準備 Helper VHD

1.  在裝有 Hyper-v 的電腦上，以及已安裝的遠端伺服器管理工具功能**受防護的 Vm 工具**上，建立具有空白 VHDX 的新第2代 VM，並使用 WINDOWS server ISO 安裝媒體在其上安裝 windows server 2016。 此 VM 不應受防護，且必須執行 Server Core 或具有桌面體驗的伺服器。

    > [!IMPORTANT]
    > VM 防護協助程式 VHD**不得**與您在主機服務提供者中建立的範本磁片相關聯，會[建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)。 如果您重複使用範本磁片，在防護過程中會發生磁片簽章衝突，因為這兩個磁片都有相同的 GPT 磁片識別碼。 若要避免這種情況，您可以建立新的 (空白) VHD，並使用您的 ISO 安裝媒體將 Windows Server 2016 安裝到它。

2.  啟動 VM，完成任何設定步驟，並登入桌面。 一旦您確認 VM 處於正常運作狀態，請關閉 VM。

3.  在提高許可權的 Windows PowerShell 視窗中，執行下列命令以準備稍早建立的 VHDX 成為 VM 防護協助程式磁片。 使用您環境的正確路徑來更新路徑。

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  成功完成命令之後，請將 VHDX 複製到 VMM 程式庫共用。 **不要再啟動步驟**1 中的 VM。 這麼做將會損毀協助程式磁片。

5.  您現在可以在 Hyper-v 中刪除步驟1中的 VM。

## <a name="configure-vmm-host-guardian-server-settings"></a>設定 VMM 主機守護者伺服器設定

在 VMM 主控台中，開啟 [設定] 窗格，然後 **[一般**] 底下的 [**主機守護者服務設定**]。 在此視窗底部，有一個欄位可設定協助程式 VHD 的位置。 使用 [流覽] 按鈕，從您的程式庫共用中選取 VHD。 如果您在共用中看不到您的磁片，可能需要在 VMM 中手動重新整理程式庫，才能顯示。

![VMM-主機守護者服務設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="additional-references"></a>其他參考資料

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
