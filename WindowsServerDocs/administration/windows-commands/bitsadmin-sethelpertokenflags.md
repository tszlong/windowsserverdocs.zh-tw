---
title: bitsadmin sethelpertokenflags
description: Bitsadmin sethelpertokenflags 命令的參考文章，此命令會針對與 BITS 傳送工作相關聯的 helper token 設定使用旗標。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 23a2626fcd1eb92c388ea1dc24682526bac59af5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630881"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

設定 [helper token](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)   與 BITS 傳送工作相關聯之 helper 權杖的使用旗標。

> [!NOTE]
> BITS 3.0 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /sethelpertokenflags <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| flags | 可能的 helper token 值，包括：<ul><li>**0x0001.** 用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳回復作業的回復檔案。</li><li>**0x0002.** 用來開啟伺服器訊息區的遠端檔案 (SMB) 上傳或下載作業，或回應 HTTP 伺服器或 proxy 挑戰的隱含 NTLM 或 Kerberos 認證。</li></ul>您必須呼叫  `/setcredentialsjob targetscheme null null`   以透過 HTTP 傳送認證。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
