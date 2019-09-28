---
title: winsat mfmedia
description: Windows 命令
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3684d4b23ba6d34febe226f54b8b2ab2204f610c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361905"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



使用媒體基礎架構來測量影片解碼（播放）的效能。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>參數

|參數|描述|
|----------|-----------|
|-輸入 \<file 名稱 >|必要：指定包含要播放或編碼之影片剪輯的檔案。 檔案可以是媒體基礎可轉譯的任何格式。|
|-dumpgraph|指定在評估開始之前，應該將篩選圖形儲存至與 GraphEdit 相容的檔案。|
|-ns|指定篩選圖表應以輸入檔的正常播放速度執行。 根據預設，篩選圖表會盡可能快速地執行，而忽略呈現時間。|
|-play|以解碼模式執行評量，並使用預設的 DirectSound 裝置，在 **-input**指定的檔案中播放任何提供的音訊內容。 根據預設，會停用音訊播放。|
|-nopmp|請不要在評估期間使用受媒體基礎保護的媒體管線（MFPMP）程式。|
|-pmp|在評估期間，請一律使用 MFPMP 流程。</br>注意：如果未指定 **-pmp**或 **-nopmp** ，則只有在必要時才會使用 MFPMP。|
|-v|將詳細資訊輸出傳送至 STDOUT，包括狀態和進度資訊。 任何錯誤也會寫入至命令視窗。|
|-xml \<檔案名 >|將評量的輸出儲存為指定的 XML 檔案。 如果指定的檔案存在，則會覆寫該檔案。|
|-idiskinfo|將實體磁片區和邏輯磁片的相關資訊儲存為 XML 輸出中 **\<SystemConfig >** 區段的一部分。|
|-iguid|在 XML 輸出檔中建立全域唯一識別碼（GUID）。|
|-注意「附注文字」|將附注文字加入至 XML 輸出檔中的 **\<note >** 區段。|
|-icn|在 XML 輸出檔中包含本機電腦名稱稱。|
|-eef|列舉 XML 輸出檔中的額外系統資訊。|

## <a name="BKMK_examples"></a>典型

- 下列範例會使用在**winsat 正式**評估期間使用的輸入檔來執行評估，而不會在 C：\windows 是 windows 資料夾位置的電腦上採用媒體基礎保護的媒體管線（MFPMP）。  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>備註

-   若要使用**winsat**，至少需要本機 Administrators 群組的成員資格或同等許可權。 命令必須從提高許可權的命令提示字元視窗執行。
-   若要開啟提高許可權的命令提示字元視窗，請依序按一下 **開始**、**附屬**應用程式、**命令提示**字元 和 **以系統管理員身分執行**

#### <a name="additional-references"></a>其他參考資料

