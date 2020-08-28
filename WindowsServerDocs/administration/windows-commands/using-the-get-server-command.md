---
title: 取得伺服器
description: 取得伺服器的參考文章，此文章會從指定的 Windows 部署服務伺服器抓取資訊。
ms.topic: reference
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13bceebfb619fc93cbab7b2b45d3dd34838760ed
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029496"
---
# <a name="get-server"></a>取得伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從指定的 Windows 部署服務伺服器抓取資訊。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Config &#124; Images &#124; All}|指定要傳回的資訊類型。<p>-   **Config** 會傳回設定資訊。<br />-   **映射** 會傳回映射群組、開機映射和安裝映射的相關資訊。<br />-   **全部** 會傳回設定資訊和映射資訊。|
|[/detailed]|您可以使用此選項搭配 **/show： Images** 或 **/show： All** ，表示應該傳回每個影像的所有影像中繼資料。 如果未使用 **/detailed** 選項，則預設行為是傳回映射名稱、描述和檔案名。|
## <a name="examples"></a>範例
若要查看伺服器的相關資訊，請輸入：
```
wdsutil /Get-Server /Show:Config
```
若要查看有關伺服器的詳細資訊，請輸入：
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 enable-Server 命令](using-the-enable-server-command.md) 
[使用 Initialize-Server 命令](using-the-initialize-server-command.md) 
[子命令：設定伺服器](subcommand-set-server.md) 
[子命令：啟動-伺服器](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md) 
解除[初始化伺服器選項](the-uninitialize-server-option.md)
