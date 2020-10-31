---
title: wdsutil 中斷連接-用戶端
description: Wdsutil 中斷連線-用戶端命令的參考文章，會中斷用戶端與多播傳輸或命名空間的連線。
ms.topic: reference
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd837fd9a677f78b5890cd1f2092529d44a2bc68
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070330"
---
# <a name="wdsutil-disconnect-client"></a>wdsutil 中斷連接-用戶端

中斷用戶端與多播傳輸或命名空間的連線。 除非您指定 **/force** ，否則用戶端會切換回另一個傳輸方法 (如果用戶端) 支援此方法。

## <a name="syntax"></a>語法

```
wdsutil /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| ClientId`<ClientID>` | 指定要中斷連接之用戶端的識別碼。 若要查看用戶端的識別碼，請執行 `wdsutil /get-multicasttransmission /show:clients` 命令。 |
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| /Force | 完全停止安裝，且不使用 fallback 方法。 由於 Wdsmcast.exe 不支援任何回溯機制，因此預設行為如下：<ul><li>**如果您是使用 Windows 部署服務用戶端：** 用戶端會使用單播繼續進行安裝。</li><li>**如果您不是使用 Windows 部署服務用戶端：** 安裝失敗。</li></ul>**重要事項：** 強烈建議您小心使用此參數，因為如果安裝失敗，電腦可能會處於無法使用的狀態。 |

## <a name="examples"></a>範例

若要中斷用戶端連線，請輸入：

```
wdsutil /Disconnect-Client /ClientId:1
```

若要中斷用戶端的連線，並強制安裝失敗，請輸入：

```
wdsutil /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil get-multicasttransmission 命令](wdsutil-get-multicasttransmission.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
