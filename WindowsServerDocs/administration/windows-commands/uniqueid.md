---
title: uniqueid
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cad6607e13d2657433e4e78ce8e65beff73aa9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857509"
---
# <a name="uniqueid"></a>uniqueid



顯示或設定 GUID 磁碟分割表格 (GPT) 識別項或主要開機記錄 (MBR) 簽章具有焦點的磁碟。

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用這個 DiskPart 命令。

## <a name="syntax"></a>語法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|id={\<dword> | <GUID>}|MBR 磁碟指定簽章的十六進位格式的四個位元組 (DWORD) 值。</br>若為 GPT 磁碟，指定的 GUID 識別碼。|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   此命令適用於基本和動態磁碟。
-   磁碟必須選取這個命令才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要顯示具有焦點的 MBR 磁碟的簽章，請輸入：
```
uniqueid disk
```
若要設定 5f1b2c36 具有焦點的 MBR 磁碟的簽章，請輸入：
```
uniqueid disk id=5f1b2c36
```
若要設定 baf784e7-6bbd-4cfb-aaac-e86c96e166ee 具有焦點的 GPT 磁碟的識別碼，請輸入：
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>其他參考資料

