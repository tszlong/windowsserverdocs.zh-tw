---
title: nfsshare
description: Nfsshare 命令的參考文章，可控制網路檔案系統（NFS）共用。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9db04752ad7982f78dc72c02108fe706cdf2fa04
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956760"
---
# <a name="nfsshare"></a>nfsshare

控制網路檔案系統（NFS）共用。 使用時不含參數，此命令會顯示 Server for NFS 所匯出的所有網路檔案系統（NFS）共用。

## <a name="syntax"></a>語法

```
nfsshare <sharename>=<drive:path> [-o <option=value>...]
nfsshare {<sharename> | <drive>:<path> | * } /delete
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -o anon =`{yes|no}` | 指定匿名（未對應的）使用者是否可以存取共用目錄。 |
| -o rw =`[<host>[:<host>]...]` | 提供由*host*指定的主機或用戶端群組對共用目錄的讀寫存取。 您必須以冒號（**：**）分隔主機和組名。 如果未指定*host* ，所有主機和用戶端群組（除了以**ro**選項指定的主機和用戶端群組）都會取得讀寫存取權。 如果**ro**或**rw**選項皆未設定，則所有用戶端都具有共用目錄的讀寫存取權。 |
| -o ro =`[<host>[:<host>]...]` | 提供由*host*指定的主機或用戶端群組對共用目錄的唯讀存取權。 您必須以冒號（**：**）分隔主機和組名。 如果未指定*host* ，則所有用戶端（除了以**rw**選項指定的用戶端除外）都會取得唯讀存取權。 如果為一個或多個用戶端設定**ro**選項，但未設定**rw**選項，則只有使用**ro**選項指定的用戶端可以存取共用目錄。 |
| -o encoding =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | 指定要在 NFS 共用上設定的語言編碼。 您只能在共用上使用一種語言。 此值可以包含下列任何值：<ul><li>**euc-jp：** 日語</li><li>**euc-幼圓：** 中文</li><li>**euc-kr：** 朝鮮語</li><li>**shift-jis：** 日語</li><li>**Big5：** 中文</li><li>**Ksc5601：** 朝鮮語</li><li>**Gb2312-80：** 簡體中文</li><li>**Ansi：** ANSI 編碼</li></ul> |
| -o anongid =`<gid>` | 指定匿名（未對應的）使用者使用*gid*作為其群組識別碼（gid）來存取共用目錄。 預設值為 **-2**。 當報告未對應使用者所擁有之檔案的擁有者時，即使已停用匿名存取，也會使用匿名 GID。 |
| -o anonuid =`<uid>` | 指定匿名（未對應的）使用者使用*uid*作為其使用者識別碼（uid）來存取共用目錄。 預設值為 **-2**。 當報告未對應使用者所擁有之檔案的擁有者時，即使已停用匿名存取，也會使用匿名 UID。 |
| -o 根 =`[<host>[:<host>]...]` | 提供*主機*所指定的主機或用戶端群組對共用目錄的根目錄存取。 您必須以冒號（**：**）分隔主機和組名。 如果未指定*host* ，所有用戶端都會取得根存取權。 如果未設定**root**選項，則沒有任何用戶端具有共用目錄的根目錄存取權。 |
| /delete | 如果指定了*共用名稱*或 `<drive>:<path>` ，此參數會刪除指定的共用。 如果指定萬用字元（*），此參數會刪除所有 NFS 共用。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果將*共用名稱*當做唯一的參數，此命令會列出由*共用*區所識別之 NFS 共用的屬性。

- 如果使用了*共用名稱*和 `<drive>:<path>` ，此命令會將所識別的資料夾匯出 `<drive>:<path>` 為「*共用名稱*」。 如果您使用 **/delete**選項，指定的資料夾將會停止供 NFS 用戶端使用。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)

- [NFS Cmdlet 參考](/powershell/module/nfs)
