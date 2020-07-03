---
title: manage-bde 狀態
description: Manage-bde status 命令的參考文章，它會提供電腦上所有磁片磁碟機的相關資訊，不論它們是否受 BitLocker 保護。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20430899b8259207f228219cf0d2ac516866714a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922202"
---
# <a name="manage-bde-status"></a>manage-bde 狀態

提供電腦上所有磁片磁碟機的相關資訊;是否受 BitLocker 保護，包括：

- 大小

- BitLocker 版本

- 轉換狀態

- 已加密百分比

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

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -protectionaserrorlevel | 如果磁片區受到保護，則會導致 manage-bde 命令列工具傳送傳回碼**0** ; 如果磁片區未受保護，則傳回**1** ;最常用於批次腳本，用來判斷磁片磁碟機是否受 BitLocker 保護。 您也可以使用 **-p**作為此命令的縮寫版本。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要顯示磁片磁碟機 C 的狀態，請輸入：

```
manage-bde –status C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
