---
title: winsat mfmedia
description: Winsat mfmedia 的參考，它會測量使用媒體基礎架構 (播放) 的影片解碼效能。
ms.topic: reference
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fca6bbce2bca22f4fcb7907fff2a4818fd0a5fb9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641096"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



使用媒體基礎 framework 測量影片解碼 (播放) 的效能。



## <a name="syntax"></a>語法

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>參數

|參數|描述|
|----------|-----------|
|-輸入 \<file name>|必要：指定包含要播放或編碼之影片剪輯的檔案。 檔案可以媒體基礎所轉譯的任何格式。|
|-dumpgraph|指定在開始評估之前，應該將篩選圖形儲存至與 GraphEdit 相容的檔案。|
|-ns|指定篩選圖形應以輸入檔的正常播放速度執行。 依預設，篩選圖形會以最快的速度執行，並忽略呈現時間。|
|-play|在解碼模式中執行評量，並使用預設 DirectSound 裝置，在 **輸入** 中指定的檔案中播放任何提供的音訊內容。 預設會停用音訊播放。|
|-nopmp|在評量期間，請不要使用媒體基礎受保護的媒體管線 (MFPMP) 處理常式。|
|-pmp|在評量期間，請一律使用 MFPMP 流程。</br>注意：如果未指定 **-pmp** 或 **-nopmp** ，只有在必要時才會使用 MFPMP。|
|-v|傳送詳細資訊輸出至 STDOUT，包括狀態和進度資訊。 任何錯誤也會寫入至命令視窗。|
|-xml \<file name>|將評量的輸出儲存為指定的 XML 檔案。 如果指定的檔案存在，則會覆寫該檔案。|
|-idiskinfo|將實體磁片區和邏輯磁片的相關資訊儲存為 **\<SystemConfig>** XML 輸出中區段的一部分。|
|-iguid|在 XML 輸出檔中建立 (GUID) 的全域唯一識別碼。|
|-附注附注文字|將附注文字加入至 **\<note>** XML 輸出檔中的區段。|
|-icn|在 XML 輸出檔中包含本機電腦名稱稱。|
|-eef|列舉 XML 輸出檔中的額外系統資訊。|

## <a name="examples"></a>範例

- 若要使用在 **winsat 正式** 評估期間所用的輸入檔來執行評量，請在 C：\windows 是 windows 資料夾位置的電腦上，使用媒體基礎保護的媒體管線 (MFPMP) 。
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>備註

-   若要使用 **winsat**，至少需要本機 Administrators 群組的成員資格或同等許可權。 您必須從提升許可權的命令提示字元視窗中執行此命令。
-   若要開啟提高許可權的命令提示字元視窗，請按一下 [**開始**]，按一下 [**附屬**應用程式]，以滑鼠右鍵按一下 [**命令提示**字元]，**然後按一下 [**

## <a name="additional-references"></a>其他參考資料

