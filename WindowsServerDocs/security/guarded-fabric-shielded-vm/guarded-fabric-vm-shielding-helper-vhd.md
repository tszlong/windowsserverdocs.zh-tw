---
title: 受防護的 Vm-準備 VM 防護協助程式 VHD
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 81e6ed7950fe13c5bed4a3f8850d64e7185b8ddd
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469643"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>受防護的 Vm-準備 VM 防護協助程式 VHD

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

> [!IMPORTANT]
> 開始這些程序之前, 請確定您已安裝最新的 Windows Server 2016 累積更新，或使用最新的 Windows 10[遠端伺服器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)。 否則，程序將無法運作。 

本節說明可支援將現有 Vm 轉換成受防護的 Vm 的主機服務提供者所執行的步驟。

若要了解本主題如何放入部署受防護的 Vm 的整體程序，請參閱[裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="which-vms-can-be-shielded"></a>可以防護的 Vm？

針對現有的 Vm 防護的程序僅適用於 Vm 符合下列必要條件：

- 客體作業系統是 Windows Server 2012、 2012 R2、 2016年或半年通道發行。 現有的 Linux Vm 無法轉換成受防護的 Vm。
- VM 是第 2 代 VM （UEFI 韌體）
- VM 不會使用其作業系統磁碟區的差異磁碟。

## <a name="prepare-helper-vhd"></a>準備協助程式 VHD

1.  HYPER-V 和遠端伺服器管理工具功能的電腦上**受防護的 VM 工具**安裝，請建立新的層代 2 具有空白的 VHDX 和其使用 Windows Server ISO 的安裝上安裝 Windows Server 2016 VM媒體。 此 VM 應該不會受防護的而且必須執行 Server Core 或 Server 含桌面體驗。

    > [!IMPORTANT]
    > VM 防護協助程式 VHD**不得**跟您在中建立的範本磁碟[裝載服務提供者會建立受防護的 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)。 如果您重複使用的範本磁碟時，會有防護的程序期間的磁碟簽章衝突因為這兩個磁碟會有相同的 GPT 磁碟識別碼。 您可以避免這藉由建立新的 （空白） VHD，並安裝到其使用您 ISO 安裝媒體上的 Windows Server 2016。

2.  啟動 VM，完成任何安裝程式的步驟，並登入桌面。 一旦確認 VM 處於運作狀態，請將 VM 關機。

3.  在提升權限的 Windows PowerShell 視窗中，執行下列命令來準備稍早建立成為 VM 防護協助程式磁碟的 VHDX。 將路徑更新為您的環境的正確路徑。

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  命令已順利完成後，請將 VHDX 複製到您的 VMM 程式庫共用。 **不這麼做**再度啟動作怪步驟 1 中的 VM。 如此一來，會損毀的協助程式磁碟。

5.  您現在可以在 HYPER-V 中的步驟 1 中刪除 VM。

## <a name="configure-vmm-host-guardian-server-settings"></a>設定 VMM 主機守護者伺服器設定

在 VMM 主控台中，開啟 [設定] 窗格，然後**主機守護者服務設定**下方**一般**。 在此視窗的底部，沒有要設定您的協助程式 VHD 的位置的欄位。 您可以使用瀏覽按鈕來選取的 VHD，從您的程式庫共用。 如果看不到您在共用中的磁碟，您可能需要手動重新整理中，才會出現的 VMM 程式庫。

![VMM 主機守護者服務設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>另請參閱

- [裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
