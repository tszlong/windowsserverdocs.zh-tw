---
title: chcp
description: 適用于 chcp 命令的參考文章，此命令會變更活動主控台字碼頁。
ms.topic: reference
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ef70d73253782528bcd54f7cfd6f98de9d941702
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629831"
---
# <a name="chcp"></a>chcp

變更活動主控台字碼頁。 如果使用時不含 **參數，則** 會顯示作用中主控台字碼頁的數目。

## <a name="syntax"></a>語法

```
chcp [<nnn>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<nnn>` | 指定字碼頁。 |
| /? | 在命令提示字元顯示說明。 |

下表列出每個支援的字碼頁以及其國家/地區或語言：

| 字碼頁 | 國家/地區或語言 |
| --------- | -------------------------- |
| 437 | 美國 |
| 850 | 多語系 (拉丁)  |
| 852 | 斯拉夫 (拉丁 II)  |
| 855 | 斯拉夫文 (俄文)  |
| 857 | 土耳其文 |
| 860 | 葡萄牙文 |
| 861 | 冰島文 |
| 863 | 加拿大（法文） |
| 865 | 北歐 |
| 866 | 俄文 |
| 869 | 新式希臘文 |
| 936 | 中文 |

#### <a name="remarks"></a>備註

- 使用點陣字型的命令提示字元視窗中，只有原始設備製造商 (OEM) 與 Windows 一起安裝的字碼頁會正確出現。 在全螢幕模式或使用 TrueType 字型的命令提示字元視窗中，會正確地顯示其他字碼頁。

- 您不需要像在 MS-DOS) 中一樣準備字碼頁 (。

- 您在指派新字碼頁之後啟動的程式會使用新的字碼頁。 不過，除了您在指派新的字碼頁之前所啟動的 Cmd.exe) 之外，程式 (會繼續使用原始字碼頁。

## <a name="examples"></a>範例

若要查看作用中的字碼頁設定，請輸入：

```
chcp
```

會出現類似下列的訊息： `Active code page: 437`

若要將使用中字碼頁變更為 850 (多語系) ，請輸入：

```
chcp 850
```

如果指定的字碼頁無效，則會出現下列錯誤訊息： `Invalid code page`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
