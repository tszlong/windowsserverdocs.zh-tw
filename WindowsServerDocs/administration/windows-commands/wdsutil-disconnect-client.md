---
title: Disconnect-Client
description: 中斷連線用戶端的參考文章-用戶端會中斷用戶端與多播傳輸或命名空間的連線。
ms.topic: reference
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cc0d9b21ee7f7a400e363d978ba77778db4eb38a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524453"
---
# <a name="disconnect-client"></a>Disconnect-Client

中斷用戶端與多播傳輸或命名空間的連線。 除非您指定 **/force**，否則用戶端會切換回另一個傳輸方法 (如果用戶端) 支援此方法。

## <a name="syntax"></a>語法

```
wdsutil /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|ClientId\<Client ID>|指定要中斷連接之用戶端的識別碼。 若要查看用戶端的識別碼，請輸入 **wdsutil/get-multicasttransmission/show： clients**。|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Force|完全停止安裝，且不使用 fallback 方法。 請注意，Wdsmcast.exe 不支援任何 fallback 機制。 如果您未使用此選項，預設行為如下所示：</br>-如果您使用 Windows 部署服務用戶端，用戶端會使用單播繼續進行安裝。</br>-如果您不是使用 Windows 部署服務用戶端，安裝將會失敗。</br>重要事項：您應該小心使用此選項，因為安裝將會失敗，而且電腦可能會處於無法使用的狀態。|

## <a name="examples"></a>範例

若要中斷用戶端連線，請輸入：
```
wdsutil /Disconnect-Client /ClientId:1
```
若要中斷用戶端的連線，並強制安裝失敗，請輸入：
```
wdsutil /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)