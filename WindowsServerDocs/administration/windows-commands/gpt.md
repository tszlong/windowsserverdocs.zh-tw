---
title: gpt
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 753ad622456280f22e8c4c209452cb17af75ce24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873469"
---
# <a name="gpt"></a>gpt

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在基本的 GUID 磁碟分割表格 (gpt) 磁碟，會將指派給具有焦點的磁碟分割的 gpt 屬性。  gpt 磁碟分割屬性提供分割區使用的其他的資訊。 某些屬性是資料分割類型 GUID 所特有。

> [!CAUTION]
> 變更 gpt 屬性可能會導致您的基本資料磁碟區指派磁碟機代號，或若要避免檔案系統掛接失敗。 除非您是原始設備製造商 (OEM) 或身為 gpt 磁碟經驗的 IT 專業人員，您不應該變更 gpt 屬性。
## <a name="syntax"></a>語法
```
gpt attributes=<n>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|attributes=<n>|指定您想要套用至具有焦點的磁碟分割之屬性的值。 Gpt 屬性欄位是包含兩個子 64 位元欄位。 只在資料分割識別碼，內容中解譯較高的欄位，而較低的欄位是通用於所有資料分割識別碼。 接受的值包括：<br /><br />-   **0x0000000000000001**。 指定資料分割需要的電腦，才能正確運作。<br />-   **0x8000000000000000**。 指定的分割區將不會收到磁碟機代號預設磁碟移到其他電腦或磁碟時，會出現的第一次上，依電腦時。<br />-   **0x4000000000000000**. 隱藏磁碟分割的磁碟區。 也就是資料分割不會掛接管理員偵測到。<br />-   **0x2000000000000000**。 指定資料分割是另一個分割區的陰影複製。<br />-   **0x1000000000000000**。 指定分割區處於唯讀狀態。 這個屬性可防止大量寫入。<br /><b />如需有關這些屬性的詳細資訊，請參閱 「 屬性 」 一節[create_PARTITION_PARAMETERS 結構](https://go.microsoft.com/fwlink/?LinkId=203812)(https://go.microsoft.com/fwlink/?LinkId=203812)。|
## <a name="remarks"></a>備註
-   EFI 系統磁碟分割包含只有這些二進位檔才能啟動作業系統。 這可輕鬆 OEM 二進位檔或二進位檔的特定作業系統要置於其他磁碟分割。
-   基本的 gpt 磁碟分割必須選取此作業才會成功。 使用**選取資料分割**命令來選擇一個基本的 gpt 磁碟分割，並將焦點移到它。
## <a name="BKMK_examples"></a>範例
如果您要移動到新電腦的 gpt 磁碟，而且想要防止該電腦會自動將磁碟機代號指派給具有焦點，類型的磁碟分割：
```
gpt attributes=0x8000000000000000
```

