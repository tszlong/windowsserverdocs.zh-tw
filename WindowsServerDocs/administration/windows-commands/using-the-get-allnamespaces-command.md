---
title: 使用 get AllNamespaces 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a907da52e773f85f0495681c21a55b3a8013e34e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889819"
---
# <a name="using-the-get-allnamespaces-command"></a>使用 get AllNamespaces 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示伺服器上的所有命名空間的相關資訊。
## <a name="syntax"></a>語法
Windows Server 2008：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
## <a name="parameters"></a>參數
|參數|Windows Server 2008|Windows Server 2008 R2|
|-------|------------|-------------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。||
|[/ ContentProvider:<name>]|指定內容提供者只會顯示命名空間。||
|[/Show:Clients]|僅支援 Windows Server 2008。 顯示用戶端電腦連線到命名空間的相關資訊。||
|[/details:Clients]|僅支援 Windows Server 2008 R2。 顯示用戶端電腦連線到命名空間的相關資訊。||
|[/ExcludedeletePending]|排除清單中的任何已停用的傳輸。||
## <a name="BKMK_examples"></a>範例
若要檢視的所有命名空間，請輸入：
```
wdsutil /Get-AllNamespaces
```
若要檢視停用以外的所有命名空間，請輸入：
-   Windows Server 2008
    ```
    wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
    ```
-   Windows Server 2008 R2
    ```
    wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
    ```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用的新命名空間命令](using-the-new-namespace-command.md)
[使用 移除命名空間命令](using-the-remove-namespace-command.md)
 [子命令： 開始命名空間](subcommand-start-namespace.md)
