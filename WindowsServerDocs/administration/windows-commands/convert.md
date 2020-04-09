---
title: convert
description: 轉換的 Windows 命令主題，它會將檔案分配表（FAT）和 FAT32 磁片區轉換為 NTFS 檔案系統，讓現有的檔案和目錄保持不變。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fb2981d6cd5a54737700b64b28f7a8a52de72b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847171"
---
# <a name="convert"></a>convert

將檔案分配表（FAT）和 FAT32 磁片區轉換為 NTFS 檔案系統，讓現有的檔案和目錄保持不變。 轉換成 NTFS 檔案系統的磁片區無法轉換回 FAT 或 FAT32。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁片區 >|指定要轉換成 NTFS 的磁碟機號（後面接著冒號）、掛接點或磁片區名稱。|
|/fs： ntfs|必要。 將磁片區轉換為 NTFS。|
|/v|會在詳細資訊模式下執行**convert** ，這會在轉換過程中顯示所有訊息。|
|/cvtarea：\<FileName >|指定主要檔案資料表（MFT）和其他 NTFS 中繼資料檔案會寫入至現有的連續預留位置檔案。 這個檔案必須在要轉換之檔案系統的根目錄中。 使用 **/cvtarea**參數可能會在轉換後產生較不分散的檔案系統。 為了獲得最佳結果，雖然**convert**公用程式接受任何大小的檔案，但此檔案的大小應該是 1 KB 乘以檔案系統中的檔案和目錄數目。</br>重要事項：您必須先使用**fsutil file createnew**命令建立預留位置檔案，然後再執行**轉換**。 [**轉換**] 不會為您建立此檔案。 **Convert**會以 NTFS 中繼資料覆寫此檔案。 轉換之後，就會釋放此檔案中任何未使用的空間。|
|/nosecurity|指定轉換的檔案和目錄上的安全性設定允許所有使用者存取。|
|/x|視需要在轉換之前卸載磁片區。 磁碟區的任何開啟控制代碼將不再有效。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果**convert**無法鎖定磁片磁碟機（例如，磁片磁碟機是系統磁碟區或目前的磁片磁碟機），您可以選擇在下次重新開機電腦時轉換磁片磁碟機。 如果您無法立即重新開機電腦以完成轉換，請規劃重新開機電腦的時間，並允許額外的時間來完成轉換程式。
-   針對從 FAT 或 FAT32 轉換成 NTFS 的磁片區：

    由於現有的磁片使用量，MFT 會建立在不同的位置，而不是原先使用 NTFS 格式化的磁片區，因此磁片區效能可能不如原先使用 NTFS 格式化的磁片區。 為了達到最佳效能，請考慮重新建立這些磁片區，並使用 NTFS 檔案系統將它們格式化。

    從 FAT 或 FAT32 到 NTFS 的磁片區轉換會讓檔案保持不變，但相較于一開始以 NTFS 格式化的磁片區，磁片區可能會缺乏一些效能優勢。 例如，MFT 可能會在轉換後的磁片區上被分割。 此外，在轉換後的開機磁碟區上， **convert**會套用 Windows 安裝程式期間所套用的相同預設安全性。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將磁片磁碟機 E 上的磁片區轉換為 NTFS，並在轉換過程中顯示所有訊息，請輸入：
```
convert e: /fs:ntfs /v
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)