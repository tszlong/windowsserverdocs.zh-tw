---
title: bitsadmin util 和 setieproxy
description: 適用于**bitsadmin util 和 setieproxy**的 Windows 命令主題-設定使用服務帳戶傳輸檔案時所要使用的 proxy 設定。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d485c0e9cb135febdb1bf99cec4de08d7c9321b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380219"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util 和 setieproxy

設定使用服務帳戶傳輸檔案時所要使用的 proxy 設定。

**BITSAdmin 1.5 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|帳戶|指定您想要定義其 proxy 設定的服務帳戶類型。 可能值為：</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|使用量|指定要使用的 proxy 偵測格式。 可能值為：</br>-NO_PROXY —請勿使用 PROXY 伺服器。</br>-自動偵測：自動偵測 proxy 設定。</br>-MANUAL_PROXY —使用明確的 PROXY 清單和略過清單。 在 usage 標記之後，立即指定 proxy 清單和略過清單。 例如，MANUAL_PROXY proxy1，proxy2 Null。</br>    -Proxy 清單是要使用的 proxy 伺服器清單（以逗號分隔）。</br>    -略過清單是以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以是 @no__t 0local 的 >，以參照相同 LAN 上的所有伺服器。 Null 或 "" 的值可能會用於空的 proxy 略過清單。</br>-AUTOSCRIPT —與自動偵測相同，但它也會執行腳本。 指定緊接在 usage 標記之後的腳本 URL。 例如，AUTOSCRIPT http://server/proxy.js 。</br>-RESET —與 NO_PROXY 相同，不同之處在于它會移除手動 PROXY Url （若有指定），以及使用自動偵測所探索到的 Url。|
|ConnectionName|選擇性：與 **/Conn**參數搭配使用，以指定要使用的數據機連接。 如果您未指定 **/Conn**參數，BITS 會使用 LAN 連接。 指定緊接在 **/Conn**參數後面的數據機連接名稱。|

## <a name="remarks"></a>備註

使用此參數的每個後續呼叫都會取代先前指定的使用方式，而不是先前定義之使用方式的參數。 例如，如果您在個別的呼叫上指定 NO_PROXY、自動偵測和 MANUAL_PROXY，BITS 會使用最後提供的使用量，但會保留先前定義之使用方式的參數。

> [!IMPORTANT]
> 您必須從提高許可權的命令提示字元執行此命令，才能順利完成。

## <a name="examples"></a>範例

下列範例會設定 NETWORK SERVICE 帳戶的 proxy 使用方式。

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

以下是更多範例。

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)