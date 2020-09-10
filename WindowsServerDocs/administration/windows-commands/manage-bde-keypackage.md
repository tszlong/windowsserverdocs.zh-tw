---
title: manage-bde keypackage
description: Manage-bde keypackage 命令的參考文章，它會產生磁片磁碟機的金鑰套件。
ms.topic: reference
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b183c06dfa73d20f7dc3236b88d5346a8694ec61
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639301"
---
# <a name="manage-bde-keypackage"></a>manage-bde keypackage

產生磁片磁碟機的金鑰套件。 金鑰封裝可與修復工具搭配使用，以修復損毀的磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde -keypackage [<drive>] [-ID <keyprotectoryID>] [-path <pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -ID | 使用金鑰保護裝置，並搭配此識別碼值指定的識別碼來建立金鑰套件。 **秘訣：** 使用 manage-bde **–保護裝置– get** 命令，以及您想要用來建立金鑰套件的磁碟機號，以取得可用的 guid 清單來作為識別碼值。 |
| -path | 指定要儲存已建立金鑰封裝的位置。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要根據 GUID 所識別的金鑰保護裝置來建立磁片磁碟機 C 的金鑰套件，並將金鑰套件儲存至 F:\Folder，請輸入：

```
manage-bde -keypackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
