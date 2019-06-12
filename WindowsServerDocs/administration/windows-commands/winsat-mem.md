---
title: winsat 記憶體最佳化
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cdae81694a916905f36cdd9e941015e3ce5f15c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440078"
---
# <a name="winsat-mem"></a>winsat 記憶體最佳化



測試系統記憶體頻寬，大到記憶體緩衝區的反射的方式複製，因為用於處理多媒體。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
winsat mem <parameters>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-up|強制測試只有一個執行緒的記憶體。 預設值是執行每個實體 CPU 或核心一個執行緒。|
|-rn|指定應該以一般優先權執行評量的執行緒。 預設值為 15 的優先權執行。|
|-nc|指定評估應該配置記憶體，並將它標示為非快取。 這表示處理器的快取將會略過複製作業。 預設值是在快取的空間中執行。|
|-do \<n>|指定距離，以位元組為單位，會與來源緩衝區結尾的目的緩衝區的開頭。 預設值為 64 個位元組。 最大允許的目的地位移為 16 MB。 指定無效的目的位移，會導致錯誤。</br>注意:0 是有效的值，如 **\<n >** ，但不是負數。|
|-mint \<n >|指定執行階段，以秒為單位進行評量的最小值。 預設值為 2.0。 最小值為 1.0。 30.0 最大值。</br>注意:指定 **-mint**大於 **-maxt**時的兩個參數搭配使用的值將會產生錯誤。|
|-maxt \<n>|指定以秒為單位的評估作業執行時間上限。 預設值是 5.0。 最小值為 1.0。 30.0 最大值。 如果搭配 **-mint**參數，執行定期統計檢查其結果以指定的時間週期之後，就會開始評定 **-mint**。 如果統計檢查都通過，則評估將完成的等候一段時間中指定 **-maxt**已過。 如果評估執行中指定的時間段 **-maxt**收集沒有滿足統計的檢查，然後評估會在該時間完成，並傳回結果。|
|-buffersize \<n >|指定記憶體複製測試應該使用的緩衝區大小。 兩次這個數量，將會配置每個 CPU，這會決定從一個緩衝區複製到其他的資料量。 預設值為 16 MB。 這個值會四捨五入為最接近的 4 KB 界限。 最大值為 32 MB。 最小值是 4 KB。 指定無效的緩衝區大小，會導致錯誤。|
|-v|傳送至 STDOUT，包括狀態和進度的相關資訊的詳細資訊輸出。 任何錯誤也會寫入至命令視窗。|
|xml\<檔案名稱 >|將評估的輸出儲存為指定的 XML 檔案中。 如果指定的檔案存在，它會覆寫。|
|-idiskinfo|儲存實體的磁碟區和邏輯磁碟的相關資訊的一部分 **\<SystemConfig >** XML 輸出中的區段。|
|-iguid|在 XML 輸出檔中的全域唯一識別碼 (GUID)。|
|-請注意 「 附註文字 」|新增註解文字 **\<注意 >** XML 輸出檔中的一節。|
|-icn|在 XML 輸出檔中包含本機電腦名稱。|
|-eef|列舉中的 XML 輸出檔的額外的系統資訊。|

## <a name="BKMK_examples"></a>範例

- 下列範例會執行最少的 4 秒，而不再是 12 秒，使用 32 MB 的緩衝區大小，並將結果儲存在 XML 格式檔案比評定**memtest.xml**。  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>備註

-   在本機的 Administrators 群組或同等權限的成員資格會使用所需的最小**winsat**。 必須從提升權限的命令提示字元 視窗中執行命令。
-   若要開啟提升權限的命令提示字元視窗，請按一下**開始**，按一下**附屬應用程式**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**的系統管理員身分執行**.

#### <a name="additional-references"></a>其他參考資料

