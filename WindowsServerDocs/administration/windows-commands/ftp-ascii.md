---
title: ftp ascii
description: Ftp ascii 命令的參考文章，此命令會將檔案傳輸類型設定為 ASCII。
ms.topic: reference
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 75b7918451836c1cda67fe5b5f6d8d8d3b73fde0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638569"
---
# <a name="ftp-ascii"></a>ftp ascii

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 ASCII。 **Ftp**命令支援 ascii (預設) 和二進位影像檔案傳輸類型，但建議您在傳輸文字檔時使用 ascii。 在 ASCII 模式中，會執行從網路標準字元集到和轉換的字元。 例如，系統會根據目標作業系統，視需要轉換行尾字元。

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

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
