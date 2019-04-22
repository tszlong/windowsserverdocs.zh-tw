---
title: bitsadmin setproxysettings
description: 適用於 Windows 命令主題**bitsadmin setproxysettings** -設定 proxy 設定值，指定作業。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825589"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



設定指定之作業的 proxy 設定。

## <a name="syntax"></a>語法

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|使用量|下列其中一個值：</br>-預先設定，使用擁有者的 Internet Explorer 的預設值。</br>-NO_PROXY — 不使用 proxy 伺服器。</br>-覆寫，使用明確的 proxy 清單，並略過清單。 必須遵循的 proxy 和 proxy 繞道清單。</br>-自動偵測，自動偵測 proxy 設定。|
|清單|使用時*使用量*參數設定為覆寫 — 包含的 proxy 伺服器，以使用以逗號分隔清單。|
|略過|使用的時機*使用量*參數設定為覆寫 — 包含以空格分隔清單的主機名稱或 IP 位址，或兩者，為其傳輸會透過 proxy 路由，以不。 這可以是**\<本機 >** 來參考相同的區域網路上的所有伺服器。 NULL 值或""可用於空的 proxy 略過清單。|

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的 proxy 設定*myDownloadJob*。

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

以下是一些其他範例。

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)