---
title: 使用 get 伺服器命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870979"
---
# <a name="using-the-get-server-command"></a>使用 get 伺服器命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從指定的 Windows 部署服務伺服器擷取資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/ 顯示: {組態&#124;映像&#124;所有}|指定要傳回資訊的類型。<br /><br />-   **設定**傳回組態資訊。<br />-   **映像**傳回映像群組、 開機映像和安裝映像的相關資訊。<br />-   **所有**傳回組態資訊和映像資訊。|
|[/ 詳細]|您可以使用這個選項搭配 **/Show:Images**或是 **/Show:All**可指示應該傳回所有從每個映像的映像中繼資料。 如果 **/ 詳細**未使用選項，預設行為是傳回映像名稱、 描述和檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要檢視伺服器的相關資訊，請輸入：
```
wdsutil /Get-Server /Show:Config
```
若要檢視有關伺服器的詳細的資訊，請輸入：
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用伺服器命令](using-the-disable-server-command.md)
[使用 啟用伺服器命令](using-the-enable-server-command.md)
[使用初始化伺服器命令](using-the-initialize-server-command.md)
[子命令： 設定伺服器](subcommand-set-server.md)
[子命令： 啟動 Server](subcommand-start-server.md) 
 [子命令： 停止伺服器](subcommand-stop-server.md)
[取消初始化伺服器選項](the-uninitialize-server-option.md)
