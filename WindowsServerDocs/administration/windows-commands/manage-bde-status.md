---
title: manage-bde 狀態
description: Manage-bde status 命令的參考文章，可提供電腦上所有磁片磁碟機的相關資訊，不論它們是否受 BitLocker 保護。
ms.topic: reference
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ad22cf542aaf3f61d9fe861d20ad25087adfe0a1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639290"
---
# <a name="manage-bde-status"></a>manage-bde 狀態

提供電腦上所有磁片磁碟機的相關資訊;是否受 BitLocker 保護，包括：

- Size

- BitLocker 版本

- 轉換狀態

- 加密百分比

- 加密方法

- 保護狀態

- 鎖定狀態

- 識別欄位

- 金鑰保護裝置

## <a name="syntax"></a>語法

```
manage-bde -status [<drive>] [-protectionaserrorlevel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -protectionaserrorlevel | 如果磁片區受到保護，則會導致 manage-bde 命令列工具傳送傳回碼 **0** ，如果磁片區未受保護，則為 **1** 。最常用於批次腳本，以判斷磁片磁碟機是否受 BitLocker 保護。 您也可以使用 **-p** 作為此命令的縮寫版本。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要顯示磁片磁碟機 C 的狀態，請輸入：

```
manage-bde –status C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
