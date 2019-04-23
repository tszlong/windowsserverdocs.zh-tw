---
title: 匯出和匯入虛擬機器
description: 示範如何匯出和匯入虛擬機器使用 HYPER-V 管理員或 Windows PowerShell。
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844319"
---
>適用於：Windows 10，Windows Server 2016、 Microsoft HYPER-V Server 2016、windows Server 2019，Microsoft HYPER-V Server 2019

# <a name="export-and-import-virtual-machines"></a>匯出和匯入虛擬機器

這篇文章會示範如何匯出和匯入虛擬機器時，這是快速移動或複製它們。 這篇文章也會討論一些時進行的匯出或匯入的選項。

## <a name="export-a-virtual-machine"></a>匯出虛擬機器

匯出會將所有必要的檔案集合成一個單位，虛擬硬碟檔案、 虛擬機器設定檔，以及任何檢查點檔案。 您可以在處於啟動或停止狀態的虛擬機器上執行這項操作。

### <a name="using-hyper-v-manager"></a>使用 Hyper-V 管理員

建立虛擬機器匯出：

1. 在 HYPER-V 管理員 中，以滑鼠右鍵按一下 虛擬機器，然後選取**匯出**。

2. 選擇要儲存匯出的檔案，並按一下**匯出**。

匯出完成時，您可以看到在匯出位置下的所有匯出的檔案。

### <a name="using-powershell"></a>使用 PowerShell

開啟系統管理員身分工作階段和執行命令，如下所示，取代後\<vm 名稱\>並\<路徑\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

如需詳細資訊，請參閱 < [EXPORT-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm)。

## <a name="import-a-virtual-machine"></a>匯入虛擬機器 

匯入虛擬機器會在 Hyper-V 主機註冊該虛擬機器。 您可以匯入回主應用程式或新的主機。 如果您正在匯入相同的主機，您不需要匯出虛擬機器第一次，因為 HYPER-V 會嘗試重新建立虛擬機器，從可用的檔案。 所以它可以使用 HYPER-V 主機上匯入虛擬機器註冊它。

匯入虛擬機器精靈 也可協助您修正移到另一部主機時，可以存在的不相容情況。 這通常是在實體硬體，例如記憶體、 虛擬交換器和虛擬處理器的差異。

### <a name="import-using-hyper-v-manager"></a>使用 HYPER-V 管理員匯入

若要匯入虛擬機器：

1. 從**動作**功能表中 HYPER-V 管理員 中，按一下 **匯入虛擬機器**。

2. 按一下 [下一步] 。

3. 選取包含匯出的檔案的資料夾，然後按一下**下一步**。

4. 選取要匯入的虛擬機器。

5. 選擇 匯入類型，然後按一下 **下一步**。 (如需說明，請參閱[匯入類型](#import-types)下面的。)

6. 按一下 **[完成]**。

### <a name="import-using-powershell"></a>使用 PowerShell 匯入

使用**IMPORT-VM** cmdlet，依照您想要匯入類型的範例。 如需類型的描述，請參閱 <<c0> [ 匯入類型](#import-types)底下。 

#### <a name="register-in-place"></a>就地註冊

這種類型的匯入使用在匯入時的儲存位置的檔案，並保留虛擬機器的識別碼。 下列命令會顯示匯入檔案的範例。 執行類似的命令，以您自己的值。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>還原

若要匯入虛擬機器，請指定您自己的虛擬機器檔案的路徑，請執行如下，以您的值取代範例命令：

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>複本形式匯入

若要完成複製匯入，並將虛擬機器檔案移至預設 HYPER-V 位置，請執行如下的值取代範例命令：

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

如需詳細資訊，請參閱 < [IMPORT-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm)。

### <a name="import-types"></a>匯入類型

HYPER-V 提供三種匯入類型：

- **就地註冊**– 此類型會假設匯出檔案位於您將在其中儲存並執行虛擬機器的位置。 在匯出時所顯示的一樣，匯入的虛擬機器會具有相同的識別碼。 因為這個緣故，如果虛擬機器已向 HYPER-V，它必須先刪除才能匯入的運作方式。 匯出檔案匯入完成時，會變成執行狀態，且無法移除。

- **將虛擬機器還原**– 將虛擬機器還原到您選擇的位置，或使用 HYPER-V 的預設。 此匯入類型會建立匯出檔案的複本，並將它們移到選取的位置。 在匯入時，虛擬機器的識別碼與匯出時的識別碼一樣。 因為這個緣故，如果虛擬機器已在執行 hyper-v，它必須刪除才能匯入完成。 匯入完成時，匯出的檔案仍維持不變，可以移除或再次匯入。

- **複製虛擬機器**– 這是類似於 「 還原 」 類型，因為您選取檔案的位置。 差別在於，匯入的虛擬機器有新的唯一識別碼，這表示您可以匯入虛擬機器相同的主機數次。

