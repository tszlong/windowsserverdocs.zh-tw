---
title: 關閉 manage-bde
description: Manage-bde off 命令的參考文章，它會解密磁片磁碟機並關閉 BitLocker。
ms.topic: reference
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85fec912802c6f0bac7494c9a3192623ac55686
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037656"
---
# <a name="manage-bde-off"></a>關閉 manage-bde

解密磁片磁碟機並關閉 BitLocker。 解密完成時，將會移除所有金鑰保護裝置。

## <a name="syntax"></a>語法

```
manage-bde -off [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<volume>` | 指定磁碟機號，後面接著冒號、磁片區 GUID 路徑或已載入的磁片區。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要關閉磁片磁碟機 C 上的 BitLocker，請輸入：

```
manage-bde –off C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [命令上的 manage-bde](manage-bde-on.md)

- [manage-bde pause 命令](manage-bde-pause.md)

- [manage-bde resume 命令](manage-bde-resume.md)

- [manage-bde 命令](manage-bde.md)
