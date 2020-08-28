---
title: mmc
description: Mmc 命令的參考文章，此命令可讓您開啟特定 mmc 主控台、在作者模式中開啟 mmc，或指定以開啟32位或64位版本的 mmc。
ms.topic: reference
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8705cf2e2cd7eced344bcc412283dc88c829849a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037806"
---
# <a name="mmc"></a>mmc

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用 mmc 命令列選項，您可以開啟特定 **mmc** 主控台、在作者模式中開啟 **mmc** ，或指定開啟32位或64位版本的 **mmc** 。

## <a name="syntax"></a>語法

```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<path>\<filename>.msc` | 啟動 **mmc** 並開啟已儲存的主控台。 您必須指定已儲存主控台檔案的完整路徑及檔名。 如果您沒有指定主控台檔案， **mmc** 會開啟新的主控台。 |
| /a | 以作者模式開啟已儲存的主控台。  用來對儲存的主控台進行變更。 |
| /64 | 開啟64位版本的 **mmc** (mmc64) 。 僅當您在執行 Microsoft 64 位元作業系統，且想要使用 64 位元嵌入式管理單元時，才使用此選項。 |
| /32 | 開啟32位版本的 **mmc** (mmc32) 。 執行 Microsoft 64 位作業系統時，您可以使用此命令列選項開啟 mmc，以執行32位嵌入式管理單元（當您有僅限32位的嵌入式管理單元時）。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 您可以使用環境變數來建立不依賴主控台檔案明確位置的命令列或快捷方式。 比方說，如果主控台檔案的路徑是在 [系統] 資料夾中 (例如 **mmc c:\winnt\system32\ console_name services.msc**) ，您可以使用可擴充的資料字串 **% systemroot%** 來指定位置 (**mmc% systemroot% \ system32 \ console_name.** s) 。 如果您要將工作委派給組織中正在使用不同電腦的人員，這可能會很有用。

- 使用 **/a** 選項開啟主控台時，它們會在作者模式中開啟，不論其預設模式為何。 這不會永久變更檔案的預設模式設定;當您省略這個選項時，mmc 會根據其預設模式設定來開啟主控台檔案。

- 在作者模式中開啟**mmc**或主控台檔案之後，您可以按一下**主控台**功能表上的 [**開啟**]，開啟任何現有的主控台。

- 您可以使用命令列來建立快捷方式，以開啟 **mmc** 和儲存的主控台。 命令列命令可搭配 [**開始**] 功能表上的 [**執行**] 命令、在任何命令提示字元視窗、快捷方式，或在呼叫命令的任何批次檔或程式中使用。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
