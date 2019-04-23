---
title: break
description: 適用於 Windows 命令主題**break_2** -解除關聯從 VSS 陰影複製磁碟區，並使其可為一般磁碟區。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886329"
---
# <a name="break"></a>break



取消關聯從 VSS 陰影複製磁碟區，使其可為一般磁碟區。 可以存取磁碟區使用 （如果有指定） 的磁碟機代號或磁碟區名稱。 如果未指定參數，使用**中斷**在命令提示字元中顯示說明。

> [!NOTE]
> 匯入之後，這個命令是僅針對硬體陰影複製相關。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
break [writable] <SetID>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[writable]|啟用讀取/寫入存取磁碟區上。|
|\<SetID>|指定陰影複製組的識別碼。|

## <a name="remarks"></a>備註

-   公開的磁碟區，例如，它們是來自，陰影複製是唯讀的預設。
-   陰影複製識別碼，它會儲存為環境變數的別名**載入中繼資料**命令，可用於*SetID*參數。

## <a name="BKMK_examples"></a>範例

若要進行陰影複製的別名名稱 Alias1 存取為可寫入的磁碟區，在作業系統中，輸入：
```
break writable %Alias1%
```

> [!NOTE]
> 存取磁碟區直接對硬體提供者，而不需要記錄已陰影複製的磁碟區。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)