---
title: break
description: '**Break_2**的 Windows 命令主題-將陰影複製磁片區與 VSS 解除對應，使其可作為一般磁片區存取。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a5789e3442152c705b3197bf1ce5e63dc782a15c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379782"
---
# <a name="break"></a>break



將陰影複製磁片區與 VSS 解除，使其可做為一般磁片區存取。 然後可以使用磁碟機號（若已指派）或磁片區名稱來存取該磁片區。 如果在沒有參數的情況下使用， **break**會在命令提示字元中顯示說明。

> [!NOTE]
> 此命令只與匯入後的硬體陰影複製相關。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
break [writable] <SetID>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|314.cs|啟用磁片區上的讀取/寫入存取。|
|\<SetID >|指定陰影複製組的識別碼。|

## <a name="remarks"></a>備註

-   公開的磁片區（例如它們源自的陰影複製）預設為唯讀。
-   「**載入中繼資料**」命令儲存為環境變數的陰影複製識別碼別名，可用於*SetID*參數。

## <a name="BKMK_examples"></a>典型

若要在作業系統中，以可寫入磁片區的別名名稱 Alias1 來進行陰影複製，請輸入：
```
break writable %Alias1%
```

> [!NOTE]
> 磁片區的存取權會直接對硬體提供者進行，而不會記錄具有陰影複製的磁片區。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)