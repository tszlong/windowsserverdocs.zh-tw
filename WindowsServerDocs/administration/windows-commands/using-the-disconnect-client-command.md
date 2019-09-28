---
title: 使用 Disconnect-Client 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbb96d64b47ec72ff0710bfb3684257c1bda2d04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363466"
---
# <a name="using-the-disconnect-client-command"></a>使用 Disconnect-Client 命令



中斷用戶端與多播傳輸或命名空間的連線。 除非您指定 **/force**，否則用戶端會切換回另一個傳輸方法（如果用戶端支援）。

## <a name="syntax"></a>語法

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ClientId： \<Client ID >|指定要中斷連線之用戶端的識別碼。 若要查看用戶端的識別碼，請輸入**WDSUTIL/get-multicasttransmission/show： clients**。|
|[/Server： \<Server 名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Force|會完全停止安裝，而且不會使用 fallback 方法。 請注意，Wdsmcast.exe 不支援任何回退機制。 如果您未使用此選項，預設行為如下所示：</br>-如果您使用 Windows 部署服務用戶端，用戶端會使用單播繼續安裝。</br>-如果您不是使用 Windows 部署服務用戶端，則安裝會失敗。</br>重要事項：您應該謹慎使用此選項，因為安裝將會失敗，而且電腦可能會處於無法使用的狀態。|

## <a name="BKMK_examples"></a>典型

若要中斷用戶端連線，請輸入：
```
WDSUTIL /Disconnect-Client /ClientId:1
```
若要中斷用戶端連線並強制安裝失敗，請輸入：
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)