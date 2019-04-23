---
title: bitsadmin util 和 setieproxy
description: 適用於 Windows 命令主題**bitsadmin util 和 setieproxy** -設定使用的服務帳戶的檔案傳輸時所要使用的 proxy 設定。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d4f6ab2e52284895d2e7918364c24bbb69f2b1c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853509"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util 和 setieproxy

設定傳輸使用的服務帳戶的檔案時所要使用的 proxy 設定。

**1.5 和更早版本的 BITSAdmin**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|帳戶|指定服務帳戶的 proxy 設定，您想要定義的類型。 可能值為：</br>-LOCALSYSTEM</br>-   NETWORKSERVICE</br>-LOCALSERVICE|
|使用量|指定要使用的 proxy 偵測的表單。 可能值為：</br>-NO_PROXY — 不使用 proxy 伺服器。</br>-自動偵測，自動偵測 proxy 設定。</br>-MANUAL_PROXY — 使用明確的 proxy 清單，並略過清單。 指定的 proxy 清單，並略過使用標記的正後方的清單。 例如，MANUAL_PROXY proxy1 proxy2 NULL。</br>    -Proxy 清單是以逗號分隔清單使用的 proxy 伺服器。</br>    -略過清單是以空格分隔清單的主機名稱或 IP 位址，或兩者，為其傳輸要不透過 proxy 路由傳送。 這可以是\<本機 > 來參考相同的區域網路上的所有伺服器。 NULL 值或""可用於空的 proxy 略過清單。</br>-AUTOSCRIPT — 一樣，自動偵測，但它也會執行指令碼。 指定使用方式標記的正後方的指令碼 URL。 比方說，AUTOSCRIPT http://server/proxy.js。</br>重設-NO_PROXY，但是它與相同移除手動 proxy Url （如果有指定），並使用自動偵測探索到的 Url。|
|ConnectionName|選擇性 — 搭配 **/conn**參數來指定要使用的數據機連接。 如果您未指定 **/conn**參數，BITS 會使用區域網路連線。 指定的數據機連線名稱，緊接 **/conn**參數。|

## <a name="remarks"></a>備註

使用這個參數的每個後續呼叫會取代先前指定的使用方式，但不是先前定義的使用方式的參數。 比方說，如果您指定 NO_PROXY、 自動偵測，以及 MANUAL_PROXY 在個別的呼叫時，BITS 會使用最後一個提供的使用，但保留參數從先前定義的使用方式。

> [!IMPORTANT]
> 您必須執行此命令從提升權限的命令提示字元，才能順利完成。

## <a name="BKMK_examples"></a>範例

下列範例會設定網路服務帳戶的 proxy 使用方式。

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

以下是詳細的範例。

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)