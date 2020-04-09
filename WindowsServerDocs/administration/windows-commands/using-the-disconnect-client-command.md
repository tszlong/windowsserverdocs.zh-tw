---
title: 中斷連線-用戶端
description: 中斷連線用戶端的 Windows 命令主題，這會中斷用戶端與多播傳輸或命名空間的連接。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ba40a7e885cfa3e42065b939d3ddb21ead2f866
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831601"
---
# <a name="disconnect-client"></a>中斷連線-用戶端

中斷用戶端與多播傳輸或命名空間的連線。 除非您指定 **/force**，否則用戶端會切換回另一個傳輸方法（如果用戶端支援）。

## <a name="syntax"></a>語法

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ClientId：\<用戶端識別碼 >|指定要中斷連線之用戶端的識別碼。 若要查看用戶端的識別碼，請輸入**WDSUTIL/get-multicasttransmission/show： clients**。|
|[/Server：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Force|會完全停止安裝，而且不會使用 fallback 方法。 請注意，Wdsmcast.exe 不支援任何回退機制。 如果您未使用此選項，預設行為如下所示：</br>-如果您使用 Windows 部署服務用戶端，用戶端會使用單播繼續安裝。</br>-如果您不是使用 Windows 部署服務用戶端，則安裝會失敗。</br>重要事項：您應該謹慎使用此選項，因為安裝將會失敗，而且電腦可能會處於無法使用的狀態。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要中斷用戶端連線，請輸入：
```
WDSUTIL /Disconnect-Client /ClientId:1
```
若要中斷用戶端連線並強制安裝失敗，請輸入：
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)