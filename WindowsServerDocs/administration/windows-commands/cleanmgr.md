---
title: '[cleanmgr]'
description: 了解如何使用命令列選項來設定 「 磁碟清理 」 工具 (Cleanmgr.exe) 可自動清除某些檔案。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fd722dda8476891860773126c84ed10f125715f0
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407885"
---
# <a name="cleanmgr"></a>[cleanmgr]

> 適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012、windows Server 2008 r2，Windows Server （半年通道）

清除不必要的檔案，從您的電腦的硬碟。 您可以使用命令列選項來指定 [cleanmgr] 清除暫存檔案、 網際網路檔案、 下載的檔案和資源回收筒的檔案。 然後，您可以排程要使用工作排程器工具在特定時間執行的工作。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>參數

|      參數      |    描述     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | 指定您想要清除的磁碟清理的磁碟機。<br>**注意：** 使用 /sagerun:n，不使用 /d 選項。 |
| /sageset:n | 顯示 [磁碟清理設定] 對話方塊中，並也會建立登錄機碼，儲存您所選取的設定。 `n`值，也就儲存在登錄中，可讓您指定執行磁碟清理工作。 `n`值可以是任何的整數值從 0 到 65535 之間。 當您使用 /sageset 選項時，請將所有可用的選項，指定已安裝 Windows 的磁碟機。  |
|  /sagerun:n  |  如果您使用 \sageset 選項 n 的值指派給指定的工作會執行。 列舉電腦上的所有磁碟機，並針對每個磁碟機中執行選取的設定檔。           |
| /TUNEUP:n    | 執行相同 /sageset 和 /sagerun `n` 。 |
| /LOWDISK     | 執行以預設設定。 |
| /VERYLOWDISK | 使用預設設定，執行任何使用者提示。 |
| /?           | 顯示說明。 |

## <a name="options"></a>選項。

您可以使用 /sageset 和 /sagerun 指定磁碟清理的檔案的選項包括：

- **安裝程式暫存檔**-這些是無法再執行安裝程式所建立的檔案。

- **下載 Program Files** -下載的程式檔案是 ActiveX 控制項和 Java 程式，會自動從網際網路下載時您可以檢視特定頁面。 這些檔案會暫時儲存在硬碟上的 [下載 Program Files] 資料夾中。 此選項會包含檢視檔案 按鈕，以便先清理磁碟 移除它們，您可以看到檔案。 按鈕會開啟 C:\Winnt\Downloaded Program Files 資料夾。

- **網際網路暫存檔**-Temporary Internet Files 資料夾包含儲存在您的硬碟，以快速檢視的網頁。 磁碟清理會移除這些網頁，但會保留您的 Web 網頁的個人化的設定。 這個選項也會包含檢視檔案 按鈕，這會開啟 C:\Documents and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5 資料夾。 

- **舊的 Chkdsk 檔案**-當 Chkdsk 會檢查磁碟是否有錯誤、 Chkdsk 可能會儲存為檔案的根資料夾中遺失的檔案片段在磁碟上。 這些檔案是不必要的。

- **資源回收筒**-「 資源回收筒包含您已從電腦刪除的檔案。 除非您清空資源回收筒，不會永久移除這些檔案。 這個選項會包含檢視檔案 按鈕，開啟資源回收筒。 **注意：** 資源回收筒 可能會出現在多個磁碟機，比方說，不只是在 %systemroot%。

- **暫存檔**-程式有時會將暫存的資訊儲存在 Temp 資料夾。 在結束程式之前，程式通常會刪除這項資訊。 您可以放心刪除過去一週內尚未經過修改的暫存檔案。

- **離線暫存檔**-離線檔案是暫存的最近使用的網路檔案的本機複本。 這些檔案會自動快取，以便您可以從網路中斷連線後使用它們。 檢視檔案 按鈕會開啟 離線檔案 資料夾。

- **離線檔案**-離線檔案是您特別想要可以使用離線，以便您可以從網路中斷連線後將它們的網路檔案的本機複本。 檢視檔案 按鈕會開啟 離線檔案 資料夾。

- **將舊的檔案壓縮**-Windows 可以壓縮近期未使用的檔案。 壓縮檔案可以節省磁碟空間，但您仍然可以使用檔案。 會不刪除任何檔案。 因為不同的速率，會壓縮檔案，顯示您將取得的磁碟空間數量是估計值。 選項按鈕可讓您指定的磁碟清理會壓縮未使用的檔案之前要等待的天數。

- **內容的索引子類別目錄檔案**-索引服務加速並改善檔案搜尋維護索引的磁碟上的檔案。 這些目錄檔案會保留先前的編製索引作業，並可以安全地刪除。 **注意：** 類別目錄檔案可能會出現在多個磁碟機，比方說，不只是在 %systemroot%。

**請注意**如果您指定要清理包含 Windows 安裝的磁碟機，所有這些選項可在 [磁碟清理] 索引標籤上。如果您指定任何其他磁碟機，只有資源回收筒和內容的索引選項的類別目錄檔案可在 [磁碟清理] 索引標籤上。 

## <a name="examples"></a>範例

若要執行磁碟清理 應用程式，以便您可以使用它的對話方塊來指定選項，用於更新版本中，將設定儲存至集合**1**，輸入下列命令：

```
cleanmgr /sageset:1
```

執行磁碟清理，並且包含 [cleanmgr] /sageset:1 命令中所指定的選項，請輸入：

```
cleanmgr /sagerun:1
```

若要執行```cleanmgr /sageset:1```和```cleanmgr /sagerun:1```放在一起，輸入：

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>其他參考資料

[釋出磁碟空間，在 Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
