---
title: 使用 AllDriverPackages 子命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8290a95dd53718b200d10b6804d312abe95e257
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363895"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>使用 AllDriverPackages 子命令



將儲存在資料夾中的所有驅動程式套件新增至伺服器。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>參數

|          參數           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath： \<Folder 路徑 >  |                      指定資料夾的完整路徑，其中包含驅動程式套件的 .inf 檔案。                      |
|   [/Server： \<Server 名稱 >]   | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
|     [/Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group 名稱 >] |                             指定要新增封裝的驅動程式組名。                             |

## <a name="BKMK_examples"></a>典型

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[新增-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)