---
title: irftp
description: Irftp 命令的參考文章，此命令會透過紅外線連結傳送檔案。
ms.topic: reference
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9546dad7bcba60ab73a9bef50c8d4347ef6dc97
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634415"
---
# <a name="irftp"></a>irftp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過紅外線連結傳送檔案。

> [!IMPORTANT]
> 請確定要透過紅外線連結進行通訊的裝置已啟用紅外線功能且正常運作。 也請確定裝置之間已建立紅外線連結。

## <a name="syntax"></a>語法

```
irftp [<drive>:\] [[<path>] <filename>] [/h][/s]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>:\` | 指定包含您想要透過紅外線連結傳送之檔案的磁片磁碟機。 |
| `[path]<filename>` | 指定您想要透過紅外線連結傳送之檔案或檔案集的位置和名稱。 如果您指定一組檔案，則必須指定每個檔案的完整路徑。 |
| /h | 指定隱形模式。 使用隱形模式時，不會顯示 [無線連結] 對話方塊來傳送檔案。 |
| /s | 開啟 [ **無線連結** ] 對話方塊，讓您可以選取要傳送的檔案或一組檔案，而不需要使用命令列來指定磁片磁碟機、路徑和檔案名。 如果您在不使用任何參數的情況下使用此命令，[ **無線連結** ] 對話方塊也會開啟。 |

### <a name="examples"></a>範例

若要透過紅外線連結傳送 *c:\example.txt* ，請輸入：

```
irftp c:\example.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
