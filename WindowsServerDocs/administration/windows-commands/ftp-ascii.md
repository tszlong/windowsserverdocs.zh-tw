---
title: ftp ascii
description: Ftp ascii 命令的參考文章，其會將檔案傳輸類型設定為 ASCII。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87a10d209203057c4e2c132accadbd3877fc07cf
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958050"
---
# <a name="ftp-ascii"></a>ftp ascii

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 ASCII。 **Ftp**命令同時支援 ASCII （預設值）和二進位圖像檔案傳輸類型，但我們建議您在傳輸文字檔時使用 ascii。 在 ASCII 模式中，會執行與網路標準字元集之間的字元轉換。 例如，根據目標作業系統，會視需要轉換行尾字元。

## <a name="syntax"></a>語法

```
ascii
```

### <a name="examples"></a>範例

若要將檔案傳輸類型設定為 ASCII，請輸入：

```
ascii
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
