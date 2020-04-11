---
title: bitsadmin sethelpertokenflags
description: 適用于**bitsadmin sethelpertokenflags**的 Windows 命令主題，它會針對與 BITS 傳送工作相關聯的 helper token 設定使用旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 77fac03dba2bb0686a98206405622e2eb398953e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122979"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

設定與 BITS 傳送工作相關聯之 [helper token](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) 的使用旗標。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| flags | 可能的 helper token 值，包括：<ul><li>**0x0001.** 用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳-回復作業的回復檔案。</li><li>**0x0002.** 用來開啟伺服器訊息區（SMB）上傳或下載作業的遠端檔案，或回應對隱含 NTLM 或 Kerberos 認證的 HTTP 伺服器或 proxy 挑戰。</li></ul>您必須呼叫 `/setcredentialsjob targetscheme null null` ，才能透過 HTTP 傳送認證。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
