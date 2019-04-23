---
title: convert
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9fcb7b2190c3359b78145a2ef79a28e30639a89c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873159"
---
# <a name="convert"></a>convert



將檔案配置表 (FAT) 與 FAT32 磁碟區到 NTFS 檔案系統，讓現有的檔案和目錄保持不變。 轉換為 NTFS 檔案系統的磁碟區無法轉換回 FAT 或 FAT32。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁碟區 >|指定磁碟機代號 （後面接著冒號） 的情況下，掛接點或磁碟區名稱，轉換為 NTFS。|
|/fs:ntfs|必要。 將磁碟區轉換為 NTFS。|
|/v|回合**轉換**以詳細資訊模式，其中顯示在轉換過程中的所有訊息。|
|/cvtarea:\<FileName>|指定主檔案表格 (MFT) 與其他 NTFS 中繼資料檔案會寫入現有的連續預留位置檔案。 此檔案必須是要轉換的檔案系統的根目錄中。 利用 **/cvtarea**參數在轉換之後，可以產生較分散的檔案系統中。 為了獲得最佳結果，這個檔案的大小應為 1 KB 乘上的檔案和目錄中檔案系統中，雖然**轉換**公用程式可接受任何大小的檔案。</br>重要事項：您必須使用建立預留位置檔案**fsutil 檔案 createnew**命令，然後才執行**轉換**。 **轉換**不會為您建立此檔案。 **轉換**NTFS 中繼資料以覆寫此檔案。 在轉換之後，會釋放任何未使用的空間，此檔案中。|
|/nosecurity|指定已轉換的檔案和目錄上的安全性設定允許所有使用者的存取。|
|/x|如有必要，轉換之前，卸載磁碟區。 磁碟區的任何開啟控制代碼將不再有效。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果**轉換**無法鎖定該磁碟機 （例如，磁碟機是系統磁碟區或目前的磁碟機），您可以選擇將磁碟機下一次重新啟動電腦。 如果您無法重新啟動電腦，立即以完成轉換，計劃重新啟動電腦，並允許額外的時間來完成轉換程序的時間。
-   磁碟區從 FAT 或 FAT32 轉換為 NTFS:

    現有的磁碟使用量，因為 MFT 被建立在不同的位置，比在原來是格式化為 NTFS 磁碟區，因此磁碟區的效能可能不會說原來是格式化為 NTFS 的磁碟區上。 為了達到最佳效能，請考慮重新建立這些磁碟區並對其使用 NTFS 檔案系統格式化。

    從 FAT 或 FAT32 的磁碟區轉換為 NTFS，讓檔案保持不變，但磁碟區可能會缺少一些效能優點，相較於一開始是以 NTFS 格式化的磁碟區。 比方說，MFT 可能會變得分散在轉換後的磁碟區上。 此外，在轉換後的開機磁碟區中，**轉換**適用於相同的預設安全性，在執行 Windows 安裝程式時套用。

## <a name="BKMK_examples"></a>範例

若要將磁碟機 E 上的磁碟區轉換為 NTFS，並在轉換過程中顯示所有訊息，請輸入：
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)