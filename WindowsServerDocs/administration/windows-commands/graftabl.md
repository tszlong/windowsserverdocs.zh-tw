---
title: graftabl
description: Graftabl 命令的參考文章，可讓 Windows 作業系統以圖形模式顯示擴充字元集。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9259833856ec5c6de402b0db0a4de4636a66f508
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924595"
---
# <a name="graftabl"></a>graftabl

可讓 Windows 作業系統以圖形模式顯示擴充字元集。 如果使用時不含參數， **graftabl**會顯示上一個和目前的字碼頁。

## <a name="syntax"></a>語法

```
graftabl <codepage>
graftabl /status
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<codepage>` | 指定字碼頁，以定義圖形模式中擴充字元的外觀。 有效的字碼頁識別編號為：<ul><li>**437** -美國</li><li>**850** -多語系（拉丁 I）</li><li>**852** -斯拉夫語（拉丁 II）</li><li>**855** -斯拉夫文（俄文）</li><li>**857** -土耳其文</li><li>**860** -葡萄牙文</li><li>**861** -冰島文</li><li>**863** -加拿大-法文</li><li>**865** -北歐</li><li>**866** -俄文</li><li>**869** -新式希臘文</li></ul> |
| /status | 顯示此命令目前使用的字碼頁。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Graftabl**命令只會影響您指定之字碼頁擴充字元的監視顯示。 它不會變更實際的主控台輸入字碼頁。 若要變更控制台輸入字碼頁，請使用[mode](mode.md)或[chcp](chcp.md)命令。

- 每個結束代碼和其簡短描述：

    | 結束碼 | 說明 |
    | --------- | ----------- |
    | 0 | 字元集已成功載入。 未載入先前的字碼頁。 |
    | 1 | 指定了不正確的參數。 未採取任何動作。 |
    | 2 | 發生檔案錯誤。 |

- 您可以在 batch 程式中使用 ERRORLEVEL 環境變數來處理**graftabl**所傳回的結束代碼。

### <a name="examples"></a>範例

若要查看**graftabl**所使用的目前字碼頁，請輸入：

```
graftabl /status
```

若要將字碼頁437（美國）的圖形字元集載入記憶體中，請輸入：

```
graftabl 437
```

若要將字碼頁850（多語系）的圖形字元集載入記憶體中，請輸入：

```
graftabl 850
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [freedisk 命令](freedisk.md)

- [模式命令](mode.md)

- [chcp 命令](chcp.md)
