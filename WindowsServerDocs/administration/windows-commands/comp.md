---
title: comp
description: Comp 命令的參考文章，此命令會比較兩個檔案或一組檔案的內容（以位元組為單位）。
ms.topic: reference
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd796aef8ef5794e4d8c09a995cb39a9756fb444
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027776"
---
# <a name="comp"></a>comp

比較兩個檔案或一組檔案的內容，以位元組為單位。 這些檔案可以儲存在相同的磁片磁碟機或不同的磁片磁碟機上，也可以儲存在相同的目錄或不同的目錄中。 當此命令比較檔案時，會顯示其位置和檔案名。 如果使用時不含 **參數，則** 會提示您輸入要比較的檔案。

## <a name="syntax"></a>語法

```
comp [<data1>] [<data2>] [/d] [/a] [/l] [/n=<number>] [/c]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<data1>` | 指定您要比較之第一個檔案或一組檔案的位置和名稱。 您可以使用萬用字元 (**&#42;** 和 **？**) 指定多個檔案。 |
| `<data2>` | 指定要比較的第二個檔案或一組檔案的位置和名稱。 您可以使用萬用字元 (**&#42;** 和 **？**) 指定多個檔案。 |
| /d | 以十進位格式顯示差異。  (預設格式為十六進位。 )  |
| /a | 以字元顯示差異。 |
| /l | 顯示發生差異的行號，而不是顯示位元組位移。 |
| /n =`<number>` | 只會比較為每個檔案指定的行數，即使檔案的大小不同也是一樣。 |
| /C | 執行不區分大小寫的比較。 |
| /off [行] | 處理已設定 offline 屬性的檔案。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="remarks"></a>備註

- 在比較期間，[ **複合** ] 會顯示訊息，以識別檔案之間不相等資訊的位置。 除非) 指定 **/a** 或 **/d** 命令列參數，否則每個訊息都會指出不等位元組的位移記憶體位址，以及位元組的位元組內容 (十六進位標記法。 訊息會以下列格式顯示：

    ```
    Compare error at OFFSET xxxxxxxx
    file1 = xx
    file2 = xx
    ```

    在十個不相等的比較之後， **comp** 會停止比較檔案並顯示下列訊息：

    `10 Mismatches - ending compare`

- 如果您省略 *data1* 或 *data2*的必要元件，或完全省略 *data2* ，此命令會提示您輸入遺漏的資訊。

- 如果 *data1* 只包含磁碟機號或不含檔案名的目錄名稱，則此命令會比較指定目錄中的所有檔案與 *data1*中指定的檔案。

- 如果 *data2* 只包含磁碟機號或目錄名稱，則預設的 *data2* 檔案名會與 *data1*的名稱相同。

- 如果 **comp** 命令找不到指定的檔案，則會提示您是否要比較其他檔案的相關訊息。

- 您比較的檔案可能會有相同的檔案名，但前提是它們位於不同的目錄或不同的磁片磁碟機上。 您可以使用萬用字元 (**&#42;** 和 **？**) 來指定檔案名。

- 您必須指定 **/n** 來比較不同大小的檔案。 如果檔案大小不同且未指定 **/n** ，則會顯示下列訊息：

    ```
    Files are different sizes
    Compare more files (Y/N)?
    ```

    若要比較這些檔案，請按 **N** 以停止命令。 然後，再次執行 **comp** 命令，並使用 **/n** 選項來比較每個檔案的第一個部分。

- 如果您使用萬用字元 (**&#42;** 和 **？**) 指定多個檔案，則 **comp** 會尋找符合 *data1* 的第一個檔案，並與 *data2*中的對應檔案（如果存在）進行比較。 **Comp**命令會針對符合*data1*的每個檔案報告比較結果。 完成時，[ **複合** ] 會顯示下列訊息：

    `Compare more files (Y/N)?`

    若要比較更多檔案，請按 **Y**鍵。 **Comp** 命令會提示您輸入新檔案的位置和名稱。 若要停止比較，請按 **N**。當您按下 **Y**鍵時，系統會提示您輸入要使用的命令列選項。 如果您未指定任何命令列選項，則 **comp** 會使用您先前指定的選項。

## <a name="examples"></a>範例

若要比較目錄 *c:\reports* 與備份目錄的內容 `\\sales\backup\april` ，請輸入：

```
comp c:\reports \\sales\backup\april
```

若要比較 *\invoice* 目錄中文字檔的前十行，並以十進位格式顯示結果，請輸入：

```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)