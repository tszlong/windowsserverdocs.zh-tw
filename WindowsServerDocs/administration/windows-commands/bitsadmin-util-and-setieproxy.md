---
title: bitsadmin util 和 setieproxy
description: 適用于**bitsadmin util 和 setieproxy**的 Windows 命令主題，其會設定使用服務帳戶傳輸檔案時所要使用的 proxy 設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ebb33ff917ddd43bbc62413755ca28478ad5a95
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122521"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util 和 setieproxy

設定使用服務帳戶傳輸檔案時所要使用的 proxy 設定。 您必須從提高許可權的命令提示字元執行此命令，才能順利完成。

> [!NOTE]
> BITS 1.5 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>參數


| 參數 | 描述 |
| --------- | ---------- |
| 帳戶 | 指定您想要定義其 proxy 設定的服務帳戶。 可能的值包括：<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| usage | 指定要使用的 proxy 偵測格式。 可能的值包括：<ul><li>**NO_PROXY。** 不要使用 proxy 伺服器。</li><li>**檢測.** 自動偵測 proxy 設定。</li><li>**MANUAL_PROXY。** 使用指定的 proxy 清單和略過清單。 您必須在 usage 標記之後立即指定清單。 例如， `MANUAL_PROXY proxy1,proxy2 NULL`。<ul><li>**Proxy 清單。** 要使用的 proxy 伺服器清單（以逗號分隔）。</li><li>**略過清單。** 以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以是 \<本機 >，以參照相同 LAN 上的所有伺服器。 Null 或的值可用於空的 proxy 略過清單。</li></ul><li>**AUTOSCRIPT.** 與自動偵測**相同，不同**之處在于它也會執行腳本。 您必須在 usage 標記之後立即指定腳本 URL。 例如， `AUTOSCRIPT http://server/proxy.js`。</li><li>**啟動.** 與**NO_PROXY**相同，不同之處在于它會移除手動 PROXY url （若有指定），以及使用自動偵測所探索到的任何 url。</li></ul> |
| connectionname | 選擇性。 與 **/conn**參數搭配使用，以指定要使用的數據機連接。 如果您未指定 **/conn**參數，BITS 會使用 LAN 連接。 |

## <a name="remarks"></a>備註

使用此參數的每個後續呼叫都會取代先前指定的使用方式，而不是先前定義之使用方式的參數。 例如，如果您在個別的呼叫上指定**NO_PROXY**、自動偵測和**MANUAL_PROXY** **，BITS**會使用最後提供的使用量，但會保留先前定義之使用方式的參數。

## <a name="examples"></a>範例

下列範例會設定 NETWORK SERVICE 帳戶的 proxy 使用方式。

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

以下是更多範例。

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)