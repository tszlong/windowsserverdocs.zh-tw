---
description: 深入瞭解：受防護的 Vm-準備 VM 防護協助程式 VHD
title: 受防護的 Vm-準備 VM 防護協助程式 VHD
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 79c28bb51abb0bd05a4302f392d8be288b9899d5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049106"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>受防護的 Vm-準備 VM 防護協助程式 VHD

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

> [!IMPORTANT]
> 開始這些程式之前，請確定您已安裝適用于 Windows Server 2016 的最新累積更新，或使用最新的 Windows 10 [遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。 否則，程式將無法運作。

本節概述主機服務提供者所執行的步驟，以啟用將現有 Vm 轉換為受防護 Vm 的支援。

若要瞭解本主題如何納入部署受防護 Vm 的整體程式，請參閱 [主機服務提供者設定步驟，以瞭解受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="which-vms-can-be-shielded"></a>哪些 Vm 可以受到防護？

現有 Vm 的防護程式僅適用于符合下列必要條件的 Vm：

- 客體作業系統是 Windows Server 2012、2012 R2、2016或半年通道發行。 現有的 Linux Vm 無法轉換成受防護的 Vm。
- VM 是 (UEFI 固件的第2代 VM) 
- VM 不會針對其 OS 磁片區使用差異磁片。

## <a name="prepare-helper-vhd"></a>準備 Helper VHD

1.  在已安裝 Hyper-v 和遠端伺服器管理工具功能 **防護 Vm 工具** 的電腦上，使用空白 VHDX 建立新的第2代 VM，並使用 WINDOWS server ISO 安裝媒體在其上安裝 windows server 2016。 此 VM 不應受到防護，且必須執行 Server Core 或具有桌面體驗的伺服器。

    > [!IMPORTANT]
    > VM 防護協助程式 VHD **必須** 與您在 [主機服務提供者建立受防護 VM 範本](guarded-fabric-create-a-shielded-vm-template.md)中建立的範本磁片無關。 如果您重複使用範本磁片，在防護程式期間會發生磁片簽章衝突，因為這兩個磁片都有相同的 GPT 磁片識別碼。 您可以建立新的 (空白) VHD，並使用您的 ISO 安裝媒體將 Windows Server 2016 安裝到該 VHD，以避免發生此情況。

2.  啟動 VM、完成所有設定步驟，然後登入桌面。 一旦您確認 VM 處於正常運作狀態，請關閉 VM。

3.  在提高許可權的 Windows PowerShell 視窗中，執行下列命令以準備稍早建立的 VHDX 成為 VM 防護協助程式磁片。 使用您環境的正確路徑來更新路徑。

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  命令順利完成後，請將 VHDX 複製到 VMM 程式庫共用。 **請勿** 重新開機步驟1中的 VM。 這麼做將會損毀協助程式磁片。

5.  您現在可以在 Hyper-v 中刪除步驟1中的 VM。

## <a name="configure-vmm-host-guardian-server-settings"></a>設定 VMM 主機守護者伺服器設定

在 VMM 主控台中，開啟 [設定] 窗格，然後在 **[一般**] 下 **裝載守護者服務設定**。 在此視窗的底部，有一個欄位可以設定協助程式 VHD 的位置。 使用 [流覽] 按鈕，從您的程式庫共用中選取 VHD。 如果您在共用中沒有看到您的磁片，您可能需要手動重新整理 VMM 中的程式庫，以使其顯示。

![VMM-主機守護者服務設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="additional-references"></a>其他參考資料

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
