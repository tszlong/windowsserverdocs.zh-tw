---
title: winsat 記憶體
description: Winsat 記憶體的參考主題，它會以反映記憶體緩衝區複本的方式來測試系統記憶體頻寬，如同用於多媒體處理。
ms.prod: windows-server
ms.technology: manage-windows-commands
winms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 158757d4449b288469b52d2d62e7cfb53d57a7d2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720666"
---
# <a name="winsat-mem"></a>winsat 記憶體



以反射記憶體緩衝區複本的方式來測試系統記憶體頻寬，如同用於多媒體處理。



## <a name="syntax"></a>語法

```
winsat mem <parameters>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-向上|強制執行只有一個執行緒的記憶體測試。 預設為每個實體 CPU 或核心執行一個執行緒。|
|-rn|指定評估的執行緒應該以一般優先權執行。 預設值是以優先順序15執行。|
|-nc|指定評估應配置記憶體，並將其標示為未快取。 這表示將會略過處理器的快取來進行複製作業。 預設值是在快取空間中執行。|
|-do \<n>|指定來源緩衝區結尾與目的地緩衝區開頭之間的距離（以位元組為單位）。 預設值為64個位元組。 可允許的目的地位移上限為16MB。 指定不正確目的地位移會導致錯誤。</br>注意：零是** \<n>** 的有效值，但負數不是。|
|-mint \<n>|指定評量的最小執行時間（以秒為單位）。 預設值為2.0。 最小值為1.0。 最大值為30.0。</br>注意：當使用兩個參數組合時，指定大於 **-maxt**值的 **-mint**值會導致錯誤。|
|-maxt \<n>|指定評量的執行時間上限（以秒為單位）。 預設值為5.0。 最小值為1.0。 最大值為30.0。 如果搭配 **-mint**參數使用，評估會在 **-mint**中指定的一段時間之後，開始定期統計檢查其結果。 如果統計檢查通過，則評量會在 **-maxt**中指定的一段時間過後完成。 如果評估會在 **-maxt**中指定的期間內執行，而不符合統計檢查，則評估會在該時間完成，並傳回其所收集的結果。|
|-buffersize \<n>|指定記憶體複製測試應該使用的緩衝區大小。 系統會為每個 CPU 配置此數量兩次，以決定從一個緩衝區複製到另一個緩衝區的資料量。 預設值為16MB。 這個值會舍入到最接近的 4 KB 界限。 最大值為32MB。 最小值為 4 KB。 指定不正確緩衝區大小會導致錯誤。|
|-v|將詳細資訊輸出傳送至 STDOUT，包括狀態和進度資訊。 任何錯誤也會寫入至命令視窗。|
|-xml \<檔案名>|將評量的輸出儲存為指定的 XML 檔案。 如果指定的檔案存在，則會覆寫該檔案。|
|-idiskinfo|將實體磁片區和邏輯磁片的相關資訊儲存為 XML 輸出中** \<SystemConfig>** 區段的一部分。|
|-iguid|在 XML 輸出檔中建立全域唯一識別碼（GUID）。|
|-注意附注文字|將附注文字加入至 XML 輸出檔中的** \<note>** 區段。|
|-icn|在 XML 輸出檔中包含本機電腦名稱稱。|
|-eef|列舉 XML 輸出檔中的額外系統資訊。|

## <a name="examples"></a>範例

- 使用32MB 緩衝區大小，並將結果以 XML 格式儲存至檔案**memtest**，以執行最少4秒且不超過12秒的評量。  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>備註

-   若要使用**winsat**，至少需要本機 Administrators 群組的成員資格或同等許可權。 命令必須從提高許可權的命令提示字元視窗執行。
-   若要開啟提高許可權的命令提示字元視窗，請依序按一下 [**開始**]、[**附屬**應用程式]、[**命令提示**字元] 和 [**以系統管理員身分執行**

## <a name="additional-references"></a>其他參考

