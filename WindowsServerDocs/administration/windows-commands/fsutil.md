---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: a12647908811066293772ab1e9354a0d67874d88
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720053"
---
# <a name="fsutil"></a>Fsutil

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

執行與檔案配置表（FAT）和 NTFS 檔案系統相關的工作，例如管理重新分析點、管理稀疏檔案或卸載磁片區。 如果使用時不含參數， **Fsutil**會顯示一份支援的子命令清單。 

> [!NOTE] 
> 您必須以系統管理員或 Administrators 群組成員的身分登入，才能使用 Fsutil。 Fsutil 命令的功能非常強大，只應供具備 Windows 作業系統全面知識的先進使用者使用。
>
>您必須先啟用適用于 Linux 的 Windows 子系統，才能執行**Fsutil**。 以系統管理員身分在 PowerShell 中執行下列命令，以啟用此選擇性功能：
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> 安裝後，系統會提示您重新開機電腦。 在您的電腦重新開機之後，您將能夠以系統管理員身分執行**Fsutil** 。

### <a name="parameters"></a>參數

|子命令 |描述|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | 例如，查詢或變更系統上的簡短名稱行為設定時，會產生8.3 字元長度的檔案名。 移除目錄中所有檔案的簡短名稱。 掃描目錄，並識別從目錄中的檔案移除簡短名稱時可能會受到影響的登錄機碼。|
|[Fsutil 行為](fsutil-behavior.md) |查詢或設定磁片區行為。|
|[Fsutil dirty](fsutil-dirty.md)| 查詢磁片區的中途位是否已設定，或是否設定磁片區的中途位。 設定磁片區的中途位時， **autochk**會在下一次電腦重新開機時，自動檢查磁片區是否有錯誤。|
|[Fsutil 檔案](fsutil-file.md)|依使用者名稱尋找檔案（如果已啟用磁片配額）、設定檔案範圍的查詢、設定檔案的簡短名稱、設定檔案的有效資料長度、設定檔案的零資料、建立指定大小的新檔案、在指定的名稱時尋找檔案識別碼，或尋找指定檔案識別碼的檔案連結名稱。|
|[Fsutil fsinfo](fsutil-fsinfo.md)|列出所有磁片磁碟機，並查詢磁片磁碟機類型、磁片區資訊、NTFS 特定磁片區資訊或檔案系統統計資料。|
|[Fsutil 硬連結](fsutil-hardlink.md)|列出檔案的硬連結，或建立硬連結（檔案的目錄專案）。 每個檔案都可以被視為至少有一個硬連結。 在 NTFS 磁片區上，每個檔案都可以有多個永久連結，因此單一檔案可能會出現在多個目錄（或甚至在相同的目錄中，名稱不同）。 由於所有連結都參考相同的檔案，因此程式可以開啟任何連結並修改檔案。 只有在刪除檔案的所有連結之後，檔案才會從檔案系統中刪除。 建立硬式連結之後，程式就可以像任何其他檔案名一樣使用它。|
|[Fsutil objectid](fsutil-objectid.md)|管理物件識別碼，由 Windows 作業系統用來追蹤檔案和目錄等物件。|
|[Fsutil 配額](fsutil-quota.md)|管理 NTFS 磁片區上的磁片配額，以更精確地控制以網路為基礎的存放裝置。 磁片配額是以每個磁片區為基礎來執行，並可讓您以每個使用者為基礎來執行硬式和軟儲存體限制。|
|[Fsutil repair](fsutil-repair.md)|查詢或設定磁片區的自我修復狀態。 自我修復 NTFS 會嘗試在線上更正 NTFS 檔案系統的損毀，而不需要執行**chkdsk.exe** 。 包括起始磁片上的驗證，以及等待修復完成。|
|[Fsutil reparsepoint](fsutil-reparsepoint.md)|查詢或刪除重新分析點（具有可定義屬性的 NTFS 檔案系統物件，其中包含使用者控制的資料）。 重新分析點是用來擴充輸入/輸出（i/o）子系統中的功能。 它們用於目錄連接點和磁片區掛接點。 檔案系統篩選器驅動程式也會使用這些檔案，將特定檔案標記為該驅動程式的特殊檔案。|
|[Fsutil 資源](fsutil-resource.md)|建立次要交易式 Resource Manager、啟動或停止交易式 Resource Manager、顯示交易式 Resource Manager 的相關資訊，或修改其行為。|
|[Fsutil sparse](fsutil-sparse.md)|管理稀疏檔案。 「稀疏檔案」（sparse file）是一個檔案，其中包含一或多個未配置資料的區域。 程式會將這些未配置的區域視為包含值為零的位元組，但不會使用磁碟空間來表示這些零。 會配置所有有意義或非零的資料，而不會配置所有不具意義的資料（由零組成的大型資料字串）。 當讀取稀疏檔案時，會傳回所配置的資料，因為已儲存，且未配置的資料會以零傳回（根據預設，會依照 C2 安全性需求規格）。 稀疏檔案支援可讓資料從檔案中的任何位置解除配置。|
|[Fsutil 分層](fsutil-tiering.md)|啟用儲存層功能的管理，例如設定和停用旗標和層級清單。|
|[Fsutil transaction](fsutil-transaction.md)|認可指定的交易、復原指定的交易，或顯示交易的相關資訊。|
|[Fsutil usn](fsutil-usn.md)|管理更新序號（USN）變更日誌，以提供磁片區上檔案之所有變更的持續記錄。|
|[Fsutil volume](fsutil-volume.md)|管理磁片區。 卸載磁片區、查詢以查看磁片上有多少可用空間，或尋找使用指定叢集的檔案。|
|[Fsutil wim](fsutil-wim.md)|提供探索和管理 WIM 支援檔案的功能。|

## <a name="see-also"></a>另請參閱
- [命令列語法關鍵](command-line-syntax-key.md)