---
title: bitsadmin gethelpertokenflags
description: '**Bitsadmin gethelpertokenflags**的 Windows 命令主題-傳回與 BITS 傳送工作相關聯的協助程式權杖的使用方式旗標。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 25d667736d5fdcd018f557b2a5565b94898f6e51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381575"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

傳回與 BITS 傳送工作相關聯之 [helper token](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) 的使用旗標。

**BITS 3.0 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

可能的傳回值包括下列各項。

- 0x0001. 協助程式權杖可用來開啟上傳作業的本機檔案、建立或重新命名下載作業的暫存檔案，或建立或重新命名上傳-回復作業的回復檔案。
- 0x0002. 協助程式權杖是用來開啟伺服器訊息區（SMB）上傳或下載作業的遠端檔案，或是為了回應 HTTP 伺服器或 proxy 挑戰的隱含 NTLM 或 Kerberos 認證。 您必須呼叫/SetCredentialsJob TargetScheme Null Null，以允許透過 HTTP 傳送認證。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
