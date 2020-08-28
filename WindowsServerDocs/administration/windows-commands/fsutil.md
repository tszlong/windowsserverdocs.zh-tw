---
title: fsutil
description: Fsutil 命令的參考文章，會執行與檔案配置表 (FAT) 和 NTFS 檔案系統相關的工作。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
ms.topic: reference
ms.date: 08/21/2018
ms.openlocfilehash: d8ad17d65d2f16a16393e71b707bcd6478de0052
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037766"
---
# <a name="fsutil"></a>fsutil

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

執行與檔案配置表 (FAT) 和 NTFS 檔案系統相關的工作，例如管理重新剖析點、管理稀疏檔案或卸載磁片區。 如果使用時不含參數， **fsutil** 會顯示支援的子命令清單。

> [!NOTE]
> 您必須以系統管理員或 Administrators 群組成員的身分登入，才能使用 **fsutil**。 此命令的功能相當強大，而且只有具備 Windows 作業系統徹底瞭解的 advanced 使用者才能使用。
>
>您必須先啟用 Windows 子系統 Linux 版，才能執行 **fsutil**。 以系統管理員身分在 PowerShell 中執行下列命令，以啟用此選用功能：
>
> `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`
>
> 安裝之後，系統會提示您重新開機電腦。 電腦重新開機之後，您就能夠以系統管理員身分執行 **Fsutil** 。

### <a name="parameters"></a>參數

| 子命令 | 描述 |
| ---------- | ----------- |
| [fsutil 8dot3name](fsutil-8dot3name.md) | 例如，在系統上查詢或變更簡短名稱行為的設定，就會產生8.3 個字元長度的檔案名。 移除目錄中所有檔案的簡短名稱。 如果從目錄中的檔案移除簡短名稱，則掃描目錄並識別可能會受到影響的登錄機碼。 |
| [fsutil dirty](fsutil-dirty.md) | 查詢是否已設定磁片區的中途位，或設定磁片區的中途位。 設定磁片區的中途位時， **autochk** 會在下次電腦重新開機時自動檢查磁片區是否有錯誤。 |
| [fsutil file](fsutil-file.md) | 依使用者名稱尋找檔案 (如果已啟用磁片配額) 、查詢配置的檔案範圍、設定檔案的簡短名稱、設定檔案的有效資料長度、為檔案設定零資料、建立指定大小的新檔案、建立指定大小的檔案，或尋找指定檔案識別碼的檔案連結名稱。 |
| [fsutil fsinfo](fsutil-fsinfo.md) | 列出所有磁片磁碟機，並查詢磁片磁碟機類型、磁片區資訊、NTFS 特定磁片區資訊或檔案系統統計資料。 |
| [fsutil hardlink](fsutil-hardlink.md) | 列出檔案的永久連結，或建立永久連結， (檔案) 的目錄專案。 每個檔案都可以被視為至少有一個永久連結。 在 NTFS 磁片區上，每個檔案都可以有多個永久連結，因此單一檔案可以出現在多個目錄中， (或甚至在相同的目錄中，) 不同的名稱。 因為所有連結都會參考相同的檔案，所以程式可以開啟任何連結並修改檔案。 只有在刪除檔案的所有連結之後，檔案才會從檔案系統中刪除。 建立永久連結之後，程式可以像任何其他檔案名一樣使用它。 |
| [fsutil objectid](fsutil-objectid.md) | 管理物件識別碼，Windows 作業系統會使用這些識別碼來追蹤檔案和目錄之類的物件。 |
| [fsutil quota](fsutil-quota.md) | 管理 NTFS 磁片區上的磁片配額，以提供更精確的網路型存放裝置控制。 磁片配額會以每個磁片區為基礎來執行，並可讓您以每個使用者為基礎來實行「硬式儲存」和「軟儲存體」限制。 |
| [fsutil repair](fsutil-repair.md) | 查詢或設定磁片區的自我修復狀態。 自我修復 NTFS 會嘗試在線上修正 NTFS 檔案系統損毀，而不需要執行 **Chkdsk.exe** 。 包括起始磁片驗證，以及等待修復完成。 |
| [fsutil reparsepoint](fsutil-reparsepoint.md) | 查詢或刪除重新分析點 (NTFS 檔案系統物件，這些物件具有可定義的屬性，其中包含使用者控制的資料) 。 重新分析點可用來擴充輸入/輸出 (i/o) 子系統中的功能。 它們用於目錄連接點和磁片區掛接點。 檔案系統篩選器驅動程式也會使用這些檔案，將某些檔案標示為該驅動程式的特殊檔案。 |
| [fsutil resource](fsutil-resource.md) | 建立次要交易式 Resource Manager、啟動或停止交易式 Resource Manager、顯示交易式 Resource Manager 的相關資訊，或修改其行為。 |
| [fsutil sparse](fsutil-sparse.md) | 管理稀疏檔。 「稀疏檔案」（sparse file）是一種檔案，其中包含一或多個未配置資料的區域。 程式會將這些未配置的區域視為包含值為零的位元組，但不會使用磁碟空間來代表這些零。 配置了所有有意義或非零的資料，而所有不具意義的資料 (由零) 組成的大型字串資料，則不會配置給。 讀取稀疏檔案時，會傳回已配置的資料，並根據預設，將未配置的資料傳回為零 (預設為符合 C2 安全性需求規格) 。 稀疏檔案支援可讓您將資料從檔案中的任何位置解除配置。 |
| [fsutil tiering](fsutil-tiering.md) | 可管理儲存層功能，例如設定和停用旗標和階層清單。 |
| [fsutil transaction](fsutil-transaction.md)   | 認可指定的交易、回復指定的交易，或顯示交易的相關資訊。 |
| [fsutil usn](fsutil-usn.md) | 管理更新序號 (USN) 變更日誌，以提供對磁片區上檔案進行之所有變更的持續記錄。 |
| [fsutil volume](fsutil-volume.md) | 管理磁片區。 卸載磁片區、查詢以查看磁片上可用的可用空間量，或尋找使用指定叢集的檔案。 |
| [fsutil wim](fsutil-wim.md) | 提供用來探索和管理 WIM 支援檔案的功能。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)