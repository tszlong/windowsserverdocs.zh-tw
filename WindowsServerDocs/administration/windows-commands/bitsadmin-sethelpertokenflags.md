---
title: bitsadmin sethelpertokenflags
description: 適用于 bitsadmin sethelpertokenflags 的 Windows 命令主題，它會針對與 BITS 傳送工作相關聯的 helper token 設定使用旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c644e82026cfc1d62f3fb5d20e3925002b871036
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849491"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

設定與 BITS 傳送工作相關聯之 [helper token](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) 的使用旗標。

**BITS 3.0 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID。|
|Flags|可能的值包括下列各項。 0x0001&mdash;協助程式權杖用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳-回復作業的回復檔案。 0x0002&mdash;協助程式權杖可用來開啟伺服器訊息區（SMB）上傳或下載作業的遠端檔案，或回應 HTTP 伺服器或 proxy 挑戰的隱含 NTLM 或 Kerberos 認證。 您必須呼叫 `/SetCredentialsJob TargetScheme NULL NULL` ，以允許透過 HTTP 傳送認證。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
