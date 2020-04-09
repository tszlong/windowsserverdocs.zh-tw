---
title: bitsadmin setproxysettings
description: 適用于 bitsadmin setproxysettings 的 Windows 命令主題，其會設定指定之作業的 proxy 設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dea72d956d12070b2638f953a7a00dcb1ed7a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849201"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

設定指定之作業的 proxy 設定。

## <a name="syntax"></a>語法

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|使用方式|下列其中一個值：</br>-LNK-WHAT-ARE-PRECONFIG-SOLUTIONS —使用擁有者的 Internet Explorer 預設值。</br>-NO_PROXY-請勿使用 PROXY 伺服器。</br>-OVERRIDE —使用明確的 proxy 清單和略過清單。 Proxy 和 proxy 略過清單必須遵循。</br>-自動偵測：自動偵測 proxy 設定。|
|清單|當*Usage*參數設定為 OVERRIDE 時使用：包含要使用的 proxy 伺服器清單（以逗號分隔）。|
|不必|*使用方式參數設定*為 [覆寫] 時，會包含以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以是 **\<本機 >** ，以參照相同 LAN 上的所有伺服器。 Null 或的值可用於空的 proxy 略過清單。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會設定名為*myDownloadJob*之作業的 proxy 設定。

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

以下是一些其他範例。

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)