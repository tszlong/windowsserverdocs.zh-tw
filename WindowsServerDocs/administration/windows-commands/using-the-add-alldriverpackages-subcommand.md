---
title: 使用新增 AllDriverPackages 子命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6c7a9e95ebd36209d5729f81b7eae9e2660b3606
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890689"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>使用新增 AllDriverPackages 子命令



將會儲存到伺服器資料夾中的所有驅動程式套件。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ FolderPath:\<資料夾路徑 >|指定包含的驅動程式套件的.inf 檔案的資料夾的完整路徑。|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|[/ 架構: {x86 | ia64 | x64}]|指定要新增的驅動程式套件的架構。 其他架構的驅動程式套件會被忽略。|
|[/ DriverGroup:\<群組名稱 >]|指定應加入之封裝的驅動程式群組的名稱。|

## <a name="BKMK_examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)