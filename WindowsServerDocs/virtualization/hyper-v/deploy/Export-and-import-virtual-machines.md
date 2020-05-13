---
title: 匯出和匯入虛擬機器
description: 說明如何使用 Hyper-v 管理員或 Windows PowerShell 匯出和匯入虛擬機器。
ms.prod: windows-server
author: kbdazure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: f1b321c04ad0b7541f21b444499b13fd2b4e4a6d
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203561"
---
# <a name="export-and-import-virtual-machines"></a>匯出和匯入虛擬機器

> 適用于： Windows 10、Windows Server 2016、Microsoft Hyper-v Server 2016、Windows Server 2019、Microsoft Hyper-v Server 2019

本文說明如何匯出和匯入虛擬機器，這是一種移動或複製它們的快速方式。 本文也會討論進行匯出或匯入時的一些選擇。

## <a name="export-a-virtual-machine"></a>匯出虛擬機器

匯出會將所有必要的檔案收集到一個單元中--虛擬硬碟檔案、虛擬機器組態檔案，以及任何檢查點檔案。 您可以在處於 [已啟動] 或 [已停止] 狀態的虛擬機器上執行這項操作。

### <a name="using-hyper-v-manager"></a>使用 Hyper-V 管理員

建立虛擬機器匯出：

1. 在 [Hyper-v 管理員] 中，以滑鼠右鍵按一下虛擬機器，然後選取 [**匯出**]。

2. 選擇要儲存匯出檔案的位置，然後按一下 [**匯出**]。

匯出完成時，您可以在匯出位置下看到所有匯出的檔案。

### <a name="using-powershell"></a>使用 PowerShell

以系統管理員身分開啟會話，並在取代 \< vm 名稱和路徑之後執行如下所示的命令 \> \< \> ：

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

如需詳細資訊，請參閱[匯出-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm)。

## <a name="import-a-virtual-machine"></a>匯入虛擬機器

匯入虛擬機器會在 Hyper-V 主機註冊該虛擬機器。 您可以匯入回主機或新的主機。 如果您要匯入至相同的主機，則不需要先匯出虛擬機器，因為 Hyper-v 會嘗試從可用的檔案重新建立虛擬機器。 匯入虛擬機器可將它登錄，以便在 Hyper-v 主機上使用。

[匯入虛擬機器] wizard 也會協助您修正從一部主機移至另一部主機時可能存在的不相容問題。 這通常是實體硬體的差異，例如記憶體、虛擬交換器和虛擬處理器。

### <a name="import-using-hyper-v-manager"></a>使用 Hyper-v 管理員匯入

若要匯入虛擬機器：

1. 從 [Hyper-v 管理員] 中的 [**動作**] 功能表，按一下 [匯**入虛擬機器**]。

2. 按一下 [下一步]  。

3. 選取包含匯出檔案的資料夾，然後按 **[下一步]**。

4. 選取要匯入的虛擬機器。

5. 選擇 [匯入類型]，然後按 **[下一步]**。 （如需說明，請參閱下方的匯[入類型](#import-types)）。

6. 按一下 [完成]  。

### <a name="import-using-powershell"></a>使用 Powershell 進行匯入

使用匯入**VM** Cmdlet，遵循您想要的匯入類型範例。 如需類型的描述，請參閱下方的匯[入類型](#import-types)。

#### <a name="register-in-place"></a>就地註冊

這種類型的匯入會使用在匯入時儲存它們的檔案，並保留虛擬機器的識別碼。 下列命令會顯示匯入檔案的範例。 以您自己的值執行類似的命令。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx'
```

#### <a name="restore"></a>還原

若要匯入虛擬機器，並指定您自己的虛擬機器檔案路徑，請執行如下所示的命令，並以您的值取代範例：

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>匯入為複本

若要完成複製匯入，並將虛擬機器檔案移至預設 Hyper-v 位置，請執行類似以下的命令，並以您的值取代範例：

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

如需詳細資訊，請參閱匯[入 VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm)。

### <a name="import-types"></a>匯入類型

Hyper-v 提供三種匯入類型：

- **就地註冊**–此類型假設匯出檔案位於您要儲存和執行虛擬機器的位置。 匯入的虛擬機器具有與匯出時相同的識別碼。 因此，如果虛擬機器已經向 Hyper-v 註冊，則必須先將它刪除，然後才能匯入。 當匯入完成時，匯出檔案會變成執行中狀態檔案，且無法移除。

- **還原虛擬機器**–將虛擬機器還原至您選擇的位置，或使用 hyper-v 的預設值。 這種匯入類型會建立匯出檔案的複本，並將它們移到選取的位置。 在匯入時，虛擬機器的識別碼與匯出時的識別碼一樣。 因此，如果虛擬機器已在 Hyper-v 中執行，則必須先將它刪除，然後才能完成匯入。 當匯入完成時，匯出的檔案會維持不變，而且可以再次移除或匯入。

- **複製虛擬機器**–這類似于您選取檔案位置的還原類型。 差別在於匯入的虛擬機器具有新的唯一識別碼，這表示您可以多次將虛擬機器匯入相同的主機。

