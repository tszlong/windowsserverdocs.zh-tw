---
title: AllDriverGroups
description: AllDriverGroups 的參考主題，它會顯示伺服器上所有驅動程式群組的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ac3e0c7b05c96383714c3a702cffd6aa8df18a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720894"
---
# <a name="get-alldrivergroups"></a>AllDriverGroups

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有驅動程式群組的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Show： {PackageMetaData &#124; 篩選 &#124; 全部]|顯示指定群組中所有驅動程式套件的中繼資料。 **PackageMetaData**顯示驅動程式群組所有篩選器的相關資訊。 [**篩選**] 會顯示群組之所有驅動程式套件和篩選器的中繼資料。|
## <a name="examples"></a>範例
若要查看驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md) 
[使用 DriverGroup 命令的](using-the-get-drivergroup-command.md)命令列語法索引鍵
