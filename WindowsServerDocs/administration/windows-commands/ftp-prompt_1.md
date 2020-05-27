---
title: ftp 提示字元
description: Ftp 提示字元命令的參考主題，它會切換開啟和關閉提示模式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 57f0e1eead36665c19845944bf22325b1aecebbb
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820378"
---
# <a name="ftp-prompt"></a>ftp 提示字元

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

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
