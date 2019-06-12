---
title: mmc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4bdc093bd16ea08153b7dbc4a0e3380251f2ed7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437329"
---
# <a name="mmc"></a>mmc

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用 mmc 命令列選項，您可以開啟特定**mmc**主控台中，開啟**mmc**在作者模式中，或指定的 32 位元或 64 位元版本**mmc**開啟。
## <a name="syntax"></a>語法
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>參數

|       參數        |                                                                                                 描述                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        啟動**mmc**並開啟已儲存的主控台。 您需要指定儲存的主控台檔案的完整路徑和檔案名稱。 如果您未指定主控台檔案， **mmc**開啟新的主控台。         |
|           /a           |                                                               以作者模式中開啟已儲存的主控台。  用來對已儲存的主控台中的變更。                                                                |
|          /64           |                         開啟 64 位元版本**mmc** (mmc64)。 只有當您執行 Microsoft 64 位元作業系統，並想要使用 64 位元嵌入式管理單元，請使用此選項。                          |
|          /32           | 開啟 32 位元版本**mmc** (mmc32)。 當執行 Microsoft 64 位元作業系統，您可以執行 32 位元的嵌入式管理單元開啟 mmc，此命令列選項與當您有 32 位元唯一的嵌入式管理單元。 |
|           /?           |                                                                                    在命令提示字元顯示說明。                                                                                     |

## <a name="remarks"></a>備註
- 使用<path> **\\** <filename> **.msc**命令列選項，您可以建立命令列或捷徑，而不相依於明確使用環境變數主控台檔案的位置。 比方說，主控台檔案的路徑是否在 [system] 資料夾 (例如**mmc c:\winnt\system32\console_name.msc**)，您可以使用可延伸的資料字串 **%systemroot%** 指定的位置(**mmc%systemroot%\system32\console_name.msc**)。 這可能是很有用，如果您正在將工作委派給組織中使用不同的電腦的人員。
- 使用 **/a**命令列選項開啟時使用此選項開啟主控台，在作者模式中，不論其預設模式。 這不會永久變更檔案的預設模式設定當您省略這個選項時，mmc 就會開啟主控台檔案，根據其預設模式設定。
- 在您開啟之後**mmc**或主控台檔案在作者模式中，您可以開啟任何現有的主控台，依序按一下**開啟**上**主控台**功能表。
- 您可以使用命令列建立捷徑，開啟**mmc**和已儲存的主控台。 命令列命令搭配**執行**命令**開始**功能表上，在任何命令提示字元 視窗中，在捷徑中，或任何批次檔或呼叫指令程式。
  ## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

