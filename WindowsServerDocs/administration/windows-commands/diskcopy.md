---
title: diskcopy
description: Diskcopy 命令的參考文章，此命令會將來源磁片磁碟機中的磁片內容複寫到目的地磁片磁碟機中格式化或未格式化的磁片。
ms.topic: reference
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/07/2018
ms.openlocfilehash: 5ed6c3eee6f096e6069e5752441229015db8ed86
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634954"
---
# <a name="diskcopy"></a>diskcopy

將來源磁片磁碟機中磁片的內容複寫到目的地磁片磁碟機中格式化或未格式化的磁片。 如果使用時不含參數，則 **diskcopy** 會使用來源磁片和目的地磁片的目前磁片磁碟機。

## <a name="syntax"></a>語法

```
diskcopy [<drive1>: [<drive2>:]] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive1>` | 指定包含來源磁片的磁片磁碟機。 |
| /v | 確認已正確複製資訊。 此選項會減緩複製程式。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Diskcopy** 只適用于抽取式磁碟，例如磁片磁碟機，這必須是相同的類型。 您無法搭配硬碟使用 **diskcopy** 。 如果您為 *drive1* 或 *drive2*指定硬碟，則 **diskcopy** 會顯示下列錯誤訊息：

    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```

    **Diskcopy**命令會提示您插入來源和目的地磁片，並等候您在鍵盤上按下任何鍵後再繼續。

    在複製磁片之後， **diskcopy** 會顯示下列訊息：

    ```
    Copy another diskette (Y/N)?
    ```

    如果您按下 **Y**， **diskcopy** 會提示您插入來源和目的地磁片，以進行下一個複製作業。 若要停止 **diskcopy** 進程，請按 **N**。

    如果您要在 *drive2*中複製到未格式化的磁片磁碟機，則 **diskcopy** 會以 *drive1*中磁片上的相同邊數和磁區數目來格式化磁片。 **Diskcopy** 在格式化磁片並複製檔案時，會顯示下列訊息：

    ```
    Formatting while copying
    ```

- 如果來源磁片具有磁片區序號，則 **diskcopy** 會建立目的地磁片的新磁片區序號，並顯示覆製作業完成時的號碼。

- 如果您省略 *drive2* 參數，則 **diskcopy** 會使用目前磁片磁碟機做為目的地磁片磁碟機。 如果您省略兩個磁片磁碟機 **參數，則** 在這兩個磁片磁碟機上都使用當前磁片磁碟機。 如果目前磁片磁碟機與*drive1***相同，則**會提示您視需要交換磁片。

- 從軟碟磁碟機以外的磁片磁碟機執行 **diskcopy** ，例如 C 磁片磁碟機。 如果磁片 *drive1* 和磁片 *drive2* 相同，則 **diskcopy** 會提示您切換磁片。 如果磁片包含的資訊超過可用記憶體可容納的數量，則 **diskcopy** 無法一次讀取所有資訊。 **Diskcopy** 從來源磁片讀取、寫入目的地磁片，並提示您再次插入來源磁片。 此程式會繼續進行，直到您複製整個磁片為止。

- 分割是指磁片上現有檔案之間的未使用磁碟空間的小型區域。 分散的來源磁片可能會使尋找、讀取或寫入檔案的程式變慢。

    由於 **diskcopy** 會在目的地磁片上建立來源磁片的完整複本，因此來源磁片上的任何片段都會傳送至目的地磁片。 若要避免將片段從某個磁片傳送到另一個磁片，請使用 [copy 命令](copy.md) 或 [xcopy 命令](xcopy.md) 複製您的磁片。 因為 **copy** 和 **xcopy** 會依序複製檔案，所以不會分割新的磁片。

    > [!NOTE]
    > 您無法使用 **xcopy** 來複製啟動磁片。

- **diskcopy** 結束代碼：

    | 結束碼 | 描述 |
    | --------- | ----------- |
    | 0 | 複製作業成功 |
    | 1 | 發生非嚴重的讀取/寫入錯誤 |
    | 3 | 發生嚴重的硬性錯誤 |
    | 4 | 發生初始化錯誤 |

    若要處理**diskcomp**傳回的結束代碼，您可以在 batch 程式的**if**命令列上使用*ERRORLEVEL*環境變數。

## <a name="examples"></a>範例

若要將磁片磁碟機 B 中的磁碟複製到磁片磁碟機 A 中的磁片，請輸入：

```
diskcopy b: a:
```

若要使用軟碟磁碟機 A 將磁片磁碟機複製到另一個磁片磁碟機，請先切換至 C 磁片磁碟機，然後輸入：

```
diskcopy a: a:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [xcopy 命令](xcopy.md)

- [複製命令](copy.md)
