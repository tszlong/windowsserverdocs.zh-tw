---
title: ftp prompt
description: Ftp 提示字元命令的參考文章，它會切換開啟和關閉提示模式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dad75921ce6878d5a255edf92c5877238ffe899a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957590"
---
# <a name="ftp-prompt"></a>ftp prompt

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

切換開啟和關閉提示模式。 預設會開啟 [提示模式]。 如果開啟 [提示模式]，ftp 命令會在多個檔案傳輸期間提示，讓您選擇性地抓取或儲存檔案。

> [!NOTE]
> 當提示模式關閉時，您可以使用[ftp mget](ftp-mget.md)和[ftp mput](ftp-mput_1.md)命令來傳輸所有檔案。

## <a name="syntax"></a>語法

```
prompt
```

### <a name="examples"></a>範例

若要開啟和關閉提示模式，請輸入：

```
prompt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
