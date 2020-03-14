---
title: 移除儲存空間直接存取中的伺服器
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: 如何在 Windows Server 移除儲存空間直接存取集區中的伺服器。
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: ce8caef2b51279c97cc012045750b7a73d97a4ba
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322310"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>移除儲存空間直接存取中的伺服器

>適用于： Windows Server 2019、Windows Server 2016

本主題說明如何使用 PowerShell 移除[儲存空間直接存取](storage-spaces-direct-overview.md)中的伺服器。

## <a name="remove-a-server-but-leave-its-drives"></a>移除伺服器，但保留其磁碟機

如果您想要很快將伺服器加回至叢集，或如果您想要保留其磁碟機來將它們移到另一個伺服器，您可以從集區中移除伺服器，但*不*從儲存集區中移除其磁碟機。 如果您使用容錯移轉叢集管理員移除伺服器，這是預設行為。

使用 PowerShell 中的 [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) cmdlet：

```PowerShell
Remove-ClusterNode <Name>
```

這個 cmdlet 快速成功，無論任何容量考量，因為儲存集區會「記住」遺失的磁碟機，並期待它們回來。 遺失的磁碟機沒有資料移動。 當它們遺失時，其 **OperationalStatus** 會顯示為「通訊中斷」，而且您的磁碟區會顯示 \[不完整\]。

當磁碟機返回時，它們會被自動偵測及與集區重新關聯，即使它們已在新的伺服器。

   >[!WARNING]
   > 請勿將具有集區資料的磁碟機從一部伺服器分散到其他多部伺服器。 例如，如果有 10 個磁碟機的伺服器故障（例如因為它的主機板或開機磁碟機故障），您**可以**將所有 10 個磁碟機移到一部新伺服器，但您**無法**將每個分別移到其他不同的伺服器。

## <a name="remove-a-server-and-its-drives"></a>移除伺服器和其磁碟機

如果您想要從叢集永久移除伺服器（有時稱為向內延展），您可以從叢集移除伺服器*和*從儲存集區中移除其磁碟機。

使用 **Remove-ClusterNode** cmdlet 搭配選擇性的 **-CleanUpDisks** 旗標：

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

這個 cmdlet 可能需要很長的時間（有時候很多小時）執行，因為 Windows 必須將該伺服器上儲存的所有資料移至叢集中的其他伺服器。 完成後，磁碟機會從儲存集區永久移除，讓受影響的磁碟區回到良好狀態。

### <a name="requirements"></a>需求

若要永久向內延展 (移除伺服器*和*其磁碟機)，您的叢集必須符合下列兩個要求。 如果不符合，**Remove-ClusterNode -CleanUpDisks** cmdlet 會立即傳回錯誤，然後才會開始任何的資料移動，以減少干擾。

#### <a name="enough-capacity"></a>足夠的容量

首先，其餘伺服器中必須有足夠的儲存容量，以容納所有的磁片區。

例如，如果您有四部伺服器，每部伺服器都有 10 x 1 TB 磁碟機，您有 40 TB 實體儲存總容量。 移除一部伺服器和其所有磁碟機之後，您將剩下 30 TB 的容量。 如果您的磁碟區使用量總計超過 30 TB，它們無法裝入剩餘的伺服器，因此 cmdlet 將會傳回錯誤並不會移動任何資料。

#### <a name="enough-fault-domains"></a>足夠的容錯網域

第二，您必須有足夠的容錯網域（通常是伺服器）以提供磁碟區復原功能。

例如，如果您的磁碟區在伺服器層級使用三向鏡像復原類型，除非您有 3 個伺服器，否則磁碟區無法完全正常。 如果您擁有三個伺服器，並嘗試移除其中一個伺服器和其所有磁碟機，cmdlet 會傳回錯誤並不會移動任何資料。

下表顯示每個復原類型所需的最少容錯網域數目。

|    復原          |    最小必要容錯網域   |
|------------------------|-------------------------------------|
|    雙向鏡像      |    2                                |
|    三向鏡像    |    3                                |
|    雙同位         |    4                                |

   >[!NOTE]
   > 短時間像是故障或維護期間可以有較少的伺服器。 不過，若要讓磁碟區恢復完全良好的狀態，您必須有上面所列的伺服器數目下限。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
