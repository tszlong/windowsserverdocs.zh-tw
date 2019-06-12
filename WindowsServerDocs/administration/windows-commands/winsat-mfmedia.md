---
title: winsat mfmedia
description: Windows 命令
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c63682e474311a49b01dc8078b023547e1fb170
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440019"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



測量效能的視訊解碼 （播放） 使用的媒體基礎架構。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>參數

|參數|描述|
|----------|-----------|
|-輸入\<檔案名稱 >|必要：指定包含要播放或編碼的視訊剪輯的檔案。 檔案可以是任何可以轉譯的媒體基礎的格式。|
|-dumpgraph|指定的篩選圖形應該會儲存至 GraphEdit 相容的檔案開始評估之前。|
|-ns|指定的篩選圖形，應該執行的輸入檔正常播放速度。 根據預設，篩選圖形會以最快速度，忽略簡報時間執行。|
|-play|執行中的評估解碼模式與任何提供音訊內容中指定的檔案中的播放 **-輸入**使用預設的 DirectSound 裝置。 根據預設，會停用音訊的播放。|
|-nopmp|請勿使用的媒體基礎保護媒體管線 (MFPMP) 處理序在評估期間。|
|-pmp|請使用 MFPMP 在評估期間的處理序。</br>注意:如果 **-pmp**或是 **-nopmp**未指定，只在必要時，會使用 MFPMP。|
|-v|傳送至 STDOUT，包括狀態和進度的相關資訊的詳細資訊輸出。 任何錯誤也會寫入至命令視窗。|
|xml\<檔案名稱 >|將評估的輸出儲存為指定的 XML 檔案中。 如果指定的檔案存在，它會覆寫。|
|-idiskinfo|儲存實體的磁碟區和邏輯磁碟的相關資訊的一部分 **\<SystemConfig >** XML 輸出中的區段。|
|-iguid|在 XML 輸出檔中的全域唯一識別碼 (GUID)。|
|-請注意 「 附註文字 」|新增註解文字 **\<注意 >** XML 輸出檔中的一節。|
|-icn|在 XML 輸出檔中包含本機電腦名稱。|
|-eef|列舉中的 XML 輸出檔的額外的系統資訊。|

## <a name="BKMK_examples"></a>範例

- 下列範例會執行與輸入檔案期間所使用的評量**winsat 正式**評估，而不採用 Media Foundation 受保護的媒體管線 (MFPMP)，在 c:\windows 所在位置的電腦上[Windows] 資料夾中。  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>備註

-   在本機的 Administrators 群組或同等權限的成員資格會使用所需的最小**winsat**。 必須從提升權限的命令提示字元 視窗中執行命令。
-   若要開啟提升權限的命令提示字元視窗，請按一下**開始**，按一下**附屬應用程式**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**的系統管理員身分執行**.

#### <a name="additional-references"></a>其他參考資料

