---
title: diskshadow.exe 取消公開
description: 適用于 diskshadow.exe 取消公開的 Windows 命令主題，它會 unexposes 使用公開命令公開的陰影複製。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2f8bbdb3b810ffbf9332608a016fc3b3e188e9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832351"
---
# <a name="unexpose"></a>diskshadow.exe 取消公開

Unexposes 使用**公開**命令公開的陰影複製。 公開的陰影複製可以透過其陰影識別碼、磁碟機號、共用或掛接點來指定。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowID >|Unexposes 給定陰影識別碼所指定的陰影複製。|
|\<磁片磁碟機： >|Unexposes 與指定磁碟機號相關聯的陰影複製（例如，磁片磁碟機 P）。|
|\<共用 >|Unexposes 與指定共用相關聯的陰影複製（例如，\\*MachineName*\)\\。|
|\<掛接點 >|Unexposes 與指定掛接點相關聯的陰影複製（例如，C:\shadowcopy\)。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要 diskshadow.exe 取消公開與 Drive P 相關聯的陰影複製，請輸入：
```
unexpose P:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)