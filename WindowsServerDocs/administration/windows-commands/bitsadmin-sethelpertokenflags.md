---
title: bitsadmin sethelpertokenflags
description: Bitsadmin sethelpertokenflags 命令的參考文章，它會針對與 BITS 傳送工作相關聯的 helper token 設定使用旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d5fc4c1d78fdf7ea6a6bb67cf435c8e71d5a9cbc
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927771"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

設定 [helper token](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)   與 BITS 傳送工作相關聯之 helper token 的使用旗標。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| flags | 可能的 helper token 值，包括：<ul><li>**0x0001.** 用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳-回復作業的回復檔案。</li><li>**0x0002.** 用來開啟伺服器訊息區（SMB）上傳或下載作業的遠端檔案，或回應對隱含 NTLM 或 Kerberos 認證的 HTTP 伺服器或 proxy 挑戰。</li></ul>您必須呼叫  `/setcredentialsjob targetscheme null null`   以透過 HTTP 傳送認證。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
