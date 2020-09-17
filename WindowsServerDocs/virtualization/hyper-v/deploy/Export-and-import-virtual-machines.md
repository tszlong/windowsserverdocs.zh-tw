---
title: 匯出和匯入虛擬機器
description: 說明如何使用 Hyper-v 管理員或 Windows PowerShell 來匯出和匯入虛擬機器。
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 8e5faf0b19a4452b4841eb75ced87f76bd58b117
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746073"
---
# <a name="export-and-import-virtual-machines"></a>匯出和匯入虛擬機器

> 適用于： Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

本文說明如何匯出和匯入虛擬機器，這是快速移動或複製虛擬機器的方式。 本文也會討論進行匯出或匯入時的一些選擇。

## <a name="export-a-virtual-machine"></a>匯出虛擬機器

匯出會將所有必要的檔案收集到一個單元中-虛擬硬碟檔案、虛擬機器組態檔案，以及任何檢查點檔案。 您可以在處於 [已啟動] 或 [已停止] 狀態的虛擬機器上進行此動作。

### <a name="using-hyper-v-manager"></a>使用 Hyper-V 管理員

建立虛擬機器匯出：

1. 在 [Hyper-v 管理員] 中，以滑鼠右鍵按一下虛擬機器，然後選取 [ **匯出**]。

2. 選擇要儲存匯出檔案的位置，然後按一下 [ **匯出**]。

匯出完成時，您可以在匯出位置下看到所有匯出的檔案。

### <a name="using-powershell"></a>使用 PowerShell

在取代和之後，以系統管理員身分開啟會話，並執行如下所示的命令 \<vm name\> \<path\> ：

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

如需詳細資訊，請參閱 [匯出 VM](/powershell/module/hyper-v/export-vm)。

## <a name="import-a-virtual-machine"></a>匯入虛擬機器

匯入虛擬機器會在 Hyper-V 主機註冊該虛擬機器。 您可以匯入回主機或新的主機。 如果您要匯入到相同的主機，您就不需要先匯出虛擬機器，因為 Hyper-v 會嘗試從可用的檔案重新建立虛擬機器。 匯入虛擬機器即可註冊它，以便在 Hyper-v 主機上使用。

[匯入虛擬機器] 也可協助您修正從一部主機移至另一部主機時，可能存在的不相容問題。 這通常是實體硬體的差異，例如記憶體、虛擬交換器和虛擬處理器。

### <a name="import-using-hyper-v-manager"></a>使用 Hyper-v 管理員匯入

若要匯入虛擬機器：

1. 從 [Hyper-v 管理員] 的 [ **動作** ] 功能表中，按一下 [匯 **入虛擬機器**]。

2. 按一下 [下一步]。

3. 選取包含匯出檔案的資料夾，然後按 **[下一步]**。

4. 選取要匯入的虛擬機器。

5. 選擇匯入類型，然後按 **[下一步]**。 如需詳細說明，請參閱以下的匯 [入類型](#import-types) (。 ) 

6. 按一下 [完成]  。

### <a name="import-using-powershell"></a>使用 Powershell 進行匯入

使用匯 **入 VM** Cmdlet，並遵循您想要的匯入類型範例。 如需類型的描述，請參閱下方的匯 [入類型](#import-types)。

#### <a name="register-in-place"></a>就地註冊

這種類型的匯入會使用匯入時儲存它們的檔案，並保留虛擬機器的識別碼。 下列命令顯示匯入檔案的範例。 使用您自己的值執行類似的命令。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx'
```

#### <a name="restore"></a>還原

若要匯入虛擬機器，並指定您自己的虛擬機器檔案路徑，請執行如下的命令，將範例取代為您的值：

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>匯入為複本

若要完成複製匯入，並將虛擬機器檔案移至預設 Hyper-v 位置，請執行如下的命令，將範例取代為您的值：

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

如需詳細資訊，請參閱匯 [入 VM](/powershell/module/hyper-v/import-vm)。

### <a name="import-types"></a>匯入類型

Hyper-v 提供三種匯入類型：

- **就地註冊** –此類型假設匯出檔案位於您將儲存和執行虛擬機器的位置。 匯入的虛擬機器具有與匯出時相同的識別碼。 基於這個原因，如果虛擬機器已經向 Hyper-v 註冊，則必須先將其刪除，才能匯入。 匯入完成時，匯出檔案會變成執行中的狀態檔案，而且無法移除。

- **還原虛擬機器** –將虛擬機器還原到您選擇的位置，或使用 hyper-v 的預設值。 這種匯入類型會建立匯出檔案的複本，並將其移至選取的位置。 在匯入時，虛擬機器的識別碼與匯出時的識別碼一樣。 因此，如果虛擬機器已在 Hyper-v 中執行，則必須先將其刪除，才能完成匯入。 匯入完成時，匯出的檔案會保持不變，而且可以再次移除或匯入。

- **複製虛擬機器** –這類似于您選取檔案位置的還原類型。 差別在於，匯入的虛擬機器具有新的唯一識別碼，這表示您可以多次將虛擬機器匯入相同的主機。