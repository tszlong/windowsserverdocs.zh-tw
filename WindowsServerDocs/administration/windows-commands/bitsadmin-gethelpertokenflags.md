---
title: bitsadmin gethelpertokenflags
description: Bitsadmin gethelpertokenflags 命令的參考主題，它會傳回與 BITS 傳送工作相關聯之 helper token 的使用旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 23e93ca71915fc369a940a21ce856b14deced004
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717903"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

傳回與 BITS 傳送工作相關聯之 [helper token](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) 的使用旗標。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

可能的傳回值，包括：

- **0x0001.** 協助程式權杖可用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳-回復作業的回復檔案。

- **0x0002.** 協助程式權杖是用來開啟伺服器訊息區（SMB）上傳或下載作業的遠端檔案，或是為了回應 HTTP 伺服器或 proxy 挑戰的隱含 NTLM 或 Kerberos 認證。 您必須呼叫 `/SetCredentialsJob TargetScheme NULL NULL` 以允許透過 HTTP 傳送認證。
  
## <a name="examples"></a>範例

若要取得與名為*myDownloadJob*的 BITS 傳送工作相關聯的協助程式權杖的使用旗標：

```
bitsadmin /gethelpertokenflags myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
