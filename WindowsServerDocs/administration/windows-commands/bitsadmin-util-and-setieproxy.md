---
title: bitsadmin util and setieproxy
description: Bitsadmin util 和 setieproxy 命令的參考文章，此命令會設定使用服務帳戶傳輸檔案時所使用的 proxy 設定。
ms.topic: reference
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 442e76a55a3bf469b680e8bbb97be790f867af55
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034686"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util and setieproxy

設定使用服務帳戶傳輸檔案時所要使用的 proxy 設定。 您必須從提升許可權的命令提示字元執行此命令，才能順利完成。

> [!NOTE]
> BITS 1.5 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| account | 指定您想要定義其 proxy 設定的服務帳戶。 可能的值包括：<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| usage | 指定要使用的 proxy 偵測格式。 可能的值包括：<ul><li>**NO_PROXY。** 請勿使用 proxy 伺服器。</li><li>**自動.** 自動偵測 proxy 設定。</li><li>**MANUAL_PROXY。** 使用指定的 proxy 清單和略過清單。 您必須在使用方式標籤之後立即指定清單。 例如： `MANUAL_PROXY proxy1,proxy2 NULL` 。<ul><li>**Proxy 清單。** 要使用的 proxy 伺服器清單（以逗號分隔）。</li><li>**略過清單。** 以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以 \<local> 參考相同 LAN 上的所有伺服器。 Null 或可用於空的 proxy 略過清單的值。</li></ul><li>**AUTOSCRIPT.** 與自動偵測 **相同，但**它也會執行腳本。 您必須在使用方式標籤之後立即指定腳本 URL。 例如： `AUTOSCRIPT http://server/proxy.js` 。</li><li>**重 置。** 與 **NO_PROXY**相同，不同之處在于它會移除手動 PROXY url (如果有指定) 以及使用自動偵測所探索到的任何 url。</li></ul> |
| connectionname | 選擇性。 搭配 **/conn** 參數使用，以指定要使用哪一種數據機連接。 如果您未指定 **/conn** 參數，BITS 會使用 LAN 連接。 |

### <a name="remarks"></a>備註

使用此參數的每個後續呼叫都會取代先前指定的使用方式，但不會取代先前定義之使用方式的參數。 例如，如果您在個別的呼叫上指定 **NO_PROXY** **、自動偵測和** **MANUAL_PROXY** ，BITS 會使用最後提供的使用量，但會保留先前定義之使用方式的參數。

## <a name="examples"></a>範例

若要設定 LOCALSYSTEM 帳戶的 proxy 使用方式：

```
bitsadmin /util /setieproxy localsystem AUTODETECT
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
