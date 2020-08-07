---
title: ftp ascii
description: Ftp ascii 命令的參考文章，其會將檔案傳輸類型設定為 ASCII。
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76ed369efe992e58304d07e627fdb55bcaa039e0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889655"
---
# <a name="ftp-ascii"></a>ftp ascii

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 ASCII。 **Ftp**命令支援 ASCII (預設) 和二進位圖像檔案傳輸類型，但我們建議您在傳輸文字檔時使用 ascii。 在 ASCII 模式中，會執行與網路標準字元集之間的字元轉換。 例如，根據目標作業系統，會視需要轉換行尾字元。

## <a name="syntax"></a>語法

```
ascii
```

### <a name="examples"></a>範例

若要將檔案傳輸類型設定為 ASCII，請輸入：

```
ascii
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
