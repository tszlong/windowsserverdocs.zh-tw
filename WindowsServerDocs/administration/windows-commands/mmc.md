---
title: mmc
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1bf9efe257e9e2b6cf20c28c1e6c0cf27230a6bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373605"
---
# <a name="mmc"></a>mmc

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用 mmc 命令列選項，您可以開啟特定**mmc**主控台、在作者模式中開啟**mmc** ，或指定開啟32位或64位版本的**mmc** 。
## <a name="syntax"></a>語法
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parameters

|       參數        |                                                                                                 描述                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        啟動**mmc**並開啟儲存的主控台。 您必須指定已儲存主控台檔案的完整路徑和檔案名。 如果您未指定主控台檔案， **mmc**會開啟新的主控台。         |
|           /a           |                                                               在作者模式中開啟已儲存的主控台。  用來對已儲存的主控台進行變更。                                                                |
|          /64           |                         開啟64位版本的**mmc** （mmc64）。 只有當您執行的是 Microsoft 64 位作業系統，而且想要使用64位嵌入式管理單元時，才使用此選項。                          |
|          /32           | 開啟32位版本的**mmc** （mmc32）。 執行 Microsoft 64 位作業系統時，如果您只有32位的嵌入式管理單元，您可以在使用此命令列選項的情況下開啟 mmc，以執行32位嵌入式管理單元。 |
|           /?           |                                                                                    在命令提示字元顯示說明。                                                                                     |

## <a name="remarks"></a>備註
- 使用 <path> **\\** <filename> **.msc**命令列選項，您可以使用環境變數來建立不依賴主控台檔案明確位置的命令列或快捷方式。 例如，如果主控台檔案的路徑位於系統資料夾（例如**mmc c:\winnt\system32\ console_name .msc**）中，您可以使用可擴充的資料字串 **% systemroot%** 來指定位置（**mmc% systemroot% \ system32 \ console_name services.msc**）。 如果您要將工作委派給組織中負責不同電腦的人員，這可能會很有用。
- 當主控台以這個選項開啟時，使用 **/a**命令列選項，不論其預設模式為何，都會在作者模式中開啟。 這不會永久變更檔案的預設模式設定;當您省略此選項時，mmc 會根據其預設模式設定來開啟主控台檔案。
- 在 [作者] 模式中開啟**mmc**或主控台檔案之後，您可以按一下**主控台**功能表上的 [**開啟**] 來開啟任何現有的主控台。
- 您可以使用命令列來建立開啟**mmc**和已儲存主控台的快捷方式。 命令列命令適用于 [**開始**] 功能表上的 [**執行**] 命令、任何命令提示字元視窗、快捷方式，或是呼叫命令的任何批次檔或程式。
  ## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)

