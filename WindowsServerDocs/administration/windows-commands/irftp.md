---
title: irftp
description: Irftp 命令的參考主題，它會透過紅外線連結傳送檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d19e4f0c325baf46c0a92bf04e39bc63863fe90
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818308"
---
# <a name="irftp"></a>irftp

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過紅外線連結傳送檔案。

> [!IMPORTANT]
> 請確定要透過紅外線連結進行通訊的裝置已啟用紅外線功能且運作正常。 此外，請確定裝置之間已建立紅外線連結。

## <a name="syntax"></a>語法

```
irftp [<drive>:\] [[<path>] <filename>] [/h][/s]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>:\` | 指定包含您要透過紅外線連結傳送之檔案的磁片磁碟機。 |
| `[path]<filename>` | 指定您想要透過紅外線連結傳送的檔案或一組檔案的位置和名稱。 如果您指定一組檔案，就必須指定每個檔案的完整路徑。 |
| /h | 指定隱形模式。 使用隱形模式時，會傳送檔案，而不會顯示 [無線連結] 對話方塊。 |
| /s | 開啟 [**無線連結**] 對話方塊，讓您可以選取要傳送的檔案或一組檔案，而不需使用命令列來指定磁片磁碟機、路徑和檔案名。 如果您在沒有任何參數的情況下使用此命令，則也會開啟 [**無線連結**] 對話方塊。 |

### <a name="examples"></a>範例

若要透過紅外線連結傳送*c:\example.txt* ，請輸入：

```
irftp c:\example.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
