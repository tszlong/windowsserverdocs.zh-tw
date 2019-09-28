---
title: 使用 AllDriverGroups 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bed6c784b2fafa30f2beb0394b64fe570ddd8ff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363385"
---
# <a name="using-the-get-alldrivergroups-command"></a>使用 AllDriverGroups 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有驅動程式群組的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Show： {PackageMetaData &#124; Filters &#124; All}]|顯示指定群組中所有驅動程式套件的中繼資料。 **PackageMetaData**顯示驅動程式群組所有篩選器的相關資訊。 [**篩選**] 會顯示群組之所有驅動程式套件和篩選器的中繼資料。|
## <a name="BKMK_examples"></a>典型
若要查看驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 DriverGroup 命令](using-the-get-drivergroup-command.md)
