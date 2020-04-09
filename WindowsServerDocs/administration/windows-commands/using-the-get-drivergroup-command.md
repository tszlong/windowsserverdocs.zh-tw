---
title: DriverGroup
description: DriverGroup 的 Windows 命令主題，它會顯示伺服器上驅動程式群組的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a0649e98a0af0cbbd12db9a07f97dade66344c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831091"
---
# <a name="get-drivergroup"></a>DriverGroup

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上驅動程式群組的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/DriverGroup：<Group Name>|指定驅動程式群組的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。  如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Show： {PackageMetaData &#124; Filters &#124; All}]|顯示指定群組中所有驅動程式套件的中繼資料。 **PackageMetaData**顯示驅動程式群組所有篩選器的相關資訊。 [**篩選**] 會顯示群組之所有驅動程式套件和篩選器的中繼資料。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要查看驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>其他參考資料
- [使用 AllDriverGroups 命令](using-the-get-alldrivergroups-command.md)
[命令列語法金鑰](command-line-syntax-key.md)
