---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866809"
---
# <a name="fsutil"></a>Fsutil

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

執行檔案配置表 (FAT) 與 NTFS 檔案系統，例如管理重新分析點，管理疏鬆檔案，或卸載磁碟區相關的工作。 如果未使用任何參數， **Fsutil**會顯示一份支援的子命令。 

> [!Note] 
> 您必須以系統管理員或 Administrators 群組的成員，使用 Fsutil 登入。 Fsutil 命令是相當強大，並只供 Windows 作業系統的完整知識的進階使用者。
>
>您必須啟用適用於 Linux 的 Windows 子系統，才能執行**Fsutil**。 在 PowerShell 中啟用此選擇性功能的系統管理員身分執行下列命令：
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> 系統會提示您安裝好後，重新啟動電腦。 您的電腦重新啟動之後，您可以執行**Fsutil**身為系統管理員。

## <a name="parameters"></a>參數

|子命令 |描述|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | 查詢或變更系統上的簡短名稱行為的設定，例如，會產生 8.3 的字元長度的檔案名稱。 移除目錄中的所有檔案的簡短名稱。 掃描目錄，並識別可能會受到影響，如果從目錄中的檔案已移除的短檔名的登錄機碼。|
|[fsutil 行為](fsutil-behavior.md) |查詢或設定磁碟區的行為。|
|[fsutil 已變更](fsutil-dirty.md)| 查詢磁碟區的已變更位元是否設定，或設定磁碟區的已變更位元。 磁碟區的已變更時設定位元**autochk**會自動檢查是否有錯誤的磁碟區在下次重新啟動電腦。|
|[Fsutil 檔案](fsutil-file.md)|（如果已啟用磁碟配額），依使用者名稱找到的檔案、 查詢的檔案配置的範圍、 設定檔案的簡短名稱、 設定檔案的有效的資料長度、 設定檔案的零資料、 建立指定大小的新檔案、 尋找檔案識別碼，如果給定的名稱或尋找是連結的檔案名稱指定的檔案識別碼。|
|[fsutil fsinfo](fsutil-fsinfo.md)|列出所有磁碟機，並查詢磁碟機類型、 磁碟區資訊、 NTFS 特定磁碟區的資訊或檔案系統的統計資料。|
|[Fsutil hardlink](fsutil-hardlink.md)|列出的檔案，永久連結，或建立永久連結 （檔案的目錄項目）。 每個檔案會被視為可以擁有一個以上的永久連結。 在 NTFS 磁碟區，每個檔案可以有多個的永久連結，因此單一檔案可以出現在多個目錄 （或甚至是在相同目錄中，有不同的名稱）。 由於所有的連結參考相同的檔案，程式就可以開啟任何一個連結，並修改檔案。 從檔案系統會刪除檔案，才會刪除所有連結到它。 建立永久連結之後，程式可以使用它與任何其他的檔案名稱。|
|[Fsutil objectid](fsutil-objectid.md)|管理物件識別項，由 Windows 作業系統用來追蹤物件，例如檔案和目錄。|
|[fsutil 配額](fsutil-quota.md)|管理磁碟配額，以提供更精確地控制網路型存放區的 NTFS 磁碟區上。 磁碟配額以每個磁碟區上實作，並讓每個使用者為基礎來實作這兩個硬與軟-儲存體限制。|
|[fsutil 修復](fsutil-repair.md)|查詢或設定磁碟區的自我修復的狀態。 自我修復 NTFS 會嘗試更正線上 NTFS 檔案系統損毀，而不需要**Chkdsk.exe**執行。 包含起始磁碟上的驗證，並等候修復完成。|
|[Fsutil reparsepoint](fsutil-reparsepoint.md)|查詢或刪除重新分析點 （NTFS 檔案系統物件具有可定義屬性包含使用者控制的資料）。 重新分析點用來擴充功能中的輸入/輸出 (I/O) 子系統。 它們用於目錄掛接點以及磁碟區掛接點。 它們也會使用檔案系統篩選器驅動程式將標示為特殊的驅動程式特定檔案。|
|[Fsutil 資源](fsutil-resource.md)|建立次要交易式資源管理員、 啟動或停止交易式資源管理員，會顯示交易式資源管理員的相關資訊或修改其行為。|
|[fsutil 疏鬆](fsutil-sparse.md)|管理疏鬆檔案。 疏鬆檔案是具有一或多個區域中的未配置資料的檔案。 程式會查看這些未配置的區域，為包含零值的位元組，但沒有磁碟空間用來代表這些零。 配置有意義，或非零值的所有資料，而不被都配置所有無意義的資料 （資料的大型字串組成的零）。 疏鬆檔案讀取時，所儲存，則會傳回已配置的資料，並為零 （藉由根據 C2 安全性需求規格的預設值） 會傳回未配置的資料。 疏鬆檔案支援可取消配置從任何位置在檔案中的資料。|
|[fsutil 階層處理](fsutil-tiering.md)|可管理的儲存體層功能，例如設定和停用旗標和層級的清單。|
|[Fsutil 交易](fsutil-transaction.md)|認可指定的交易，會回復指定的交易，或顯示與交易相關的資訊。|
|[fsutil usn](fsutil-usn.md)|管理提供永續性磁碟區上的檔案所做的所有變更的記錄更新序列號碼 (USN) 變更日誌。|
|[fsutil 磁碟區](fsutil-volume.md)|管理磁碟區。 卸載磁碟區，以查看在磁碟上，有多少可用空間的查詢，或發現檔案正在使用指定的叢集。|
|[Fsutil wim](fsutil-wim.md)|提供探索及管理 WIM 備份檔案的函式。|

## <a name="see-also"></a>另請參閱
[命令列語法關鍵](Command-Line-Syntax-Key.md)