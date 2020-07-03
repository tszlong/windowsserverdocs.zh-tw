---
title: 取得-伺服器
description: 取得伺服器的參考文章，它會從指定的 Windows 部署服務伺服器抓取資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f035462de8966756e4b47ca6ba04b7d30a9cb1c6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932185"
---
# <a name="get-server"></a>取得-伺服器

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從指定的 Windows 部署服務伺服器抓取資訊。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Config &#124; 映射 &#124; 全部}|指定要傳回的資訊類型。<p>-   **Config**會傳回設定資訊。<br />-   **映射**會傳回映射群組、開機映射和安裝映射的相關資訊。<br />-   **全部都會**傳回設定資訊和影像資訊。|
|[/detailed]|您可以搭配使用此選項與 **/show： Images**或 **/show： All**來表示應傳回每個影像中的所有影像中繼資料。 如果未使用 **/detailed**選項，則預設行為是傳回映射名稱、描述和檔案名。|
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
[子命令： set-Server](subcommand-set-server.md) 
[子命令： start-Server](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md) 
解除[初始化-伺服器選項](the-uninitialize-server-option.md)
