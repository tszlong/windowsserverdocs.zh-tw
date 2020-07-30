---
title: 中斷（陰影複製磁片區）
description: '[中斷] 命令的參考文章，這會解除陰影複製磁片區與 VSS 的對應，並使其可做為一般磁片區存取。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6eb97ff1c539d8c372b4ae0837c41479c5a0f214
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409749"
---
# <a name="break-shadow-copy-volume"></a>中斷（陰影複製磁片區）

將陰影複製磁片區與 VSS 解除，使其可做為一般磁片區存取。 然後可以使用磁碟機號（若已指派）或磁片區名稱來存取該磁片區。 如果在沒有參數的情況下使用， **break**會在命令提示字元中顯示說明。

> [!NOTE]
> 此命令只與匯入後的硬體陰影複製相關。
>
> 公開的磁片區（例如它們源自的陰影複製）預設為唯讀。 磁片區的存取權會直接對硬體提供者進行，而不會記錄具有陰影複製的磁片區。

## <a name="syntax"></a>語法

```
break [writable] <setid>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 314.cs | 啟用磁片區上的讀取/寫入存取。 |
| \<setid> | 指定陰影複製組的識別碼。 「**載入中繼資料**」命令儲存為環境變數的陰影複製識別碼別名，可用於*SetID*參數。 |

## <a name="examples"></a>範例

若要使用別名名稱 Alias1 來建立陰影複製，可在作業系統中作為可寫入的磁片區：

```
break writable %Alias1%
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)