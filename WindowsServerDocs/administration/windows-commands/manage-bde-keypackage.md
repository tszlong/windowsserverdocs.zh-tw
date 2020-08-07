---
title: manage-bde keypackage
description: Manage-bde keypackage 命令的參考文章，它會產生磁片磁碟機的金鑰封裝。
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 969b9fc85959d137ec8b6bfc6b377f48e02e5157
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886877"
---
# <a name="manage-bde-keypackage"></a>manage-bde keypackage

產生磁片磁碟機的金鑰封裝。 金鑰套件可以與修復工具搭配使用，以修復損毀的磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde -keypackage [<drive>] [-ID <keyprotectoryID>] [-path <pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -ID | 使用金鑰保護裝置，並搭配此識別碼值所指定的識別碼，建立金鑰封裝。 **秘訣：** 使用 manage-bde **–保護裝置– get**命令，以及您要為其建立金鑰套件的磁碟機號，以取得可用的 guid 清單做為識別碼值。 |
| -path | 指定要儲存所建立金鑰封裝的位置。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要根據 GUID 所識別的金鑰保護裝置來建立磁片磁碟機 C 的金鑰封裝，並將金鑰套件儲存至 F:\Folder，請輸入：

```
manage-bde -keypackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
