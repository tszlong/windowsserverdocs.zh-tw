---
title: bitsadmin setproxysettings
description: Bitsadmin setproxysettings 命令的參考文章，其會設定指定之作業的 proxy 設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a59bbb560b8c89134e81c02f99aaecebdb65ca89
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927585"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

設定指定之作業的 proxy 設定。

## <a name="syntax"></a>語法

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| usage | 設定 proxy 使用方式，包括：<ul><li>**LNK-WHAT-ARE-PRECONFIG-SOLUTIONS.** 使用擁有者的 Internet Explorer 預設值。</li><li>**NO_PROXY。** 不要使用 proxy 伺服器。</li><li>**覆寫.** 使用明確的 proxy 清單和略過清單。 Proxy 清單和 proxy 略過資訊必須遵循。</li><li>**檢測.** 會自動偵測 proxy 設定。</li></ul> |
| list | 當*Usage*參數設定為 OVERRIDE 時使用。 必須包含要使用的 proxy 伺服器清單（以逗號分隔）。 |
| 略過 | 當*Usage*參數設定為 OVERRIDE 時使用。 必須包含以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可能是 `<local>` 指在相同 LAN 上的所有伺服器。 Null 的值可用於空的 proxy 略過清單。 |

## <a name="examples"></a>範例

若要使用名為*myDownloadJob*之作業的各種使用方式選項來設定 proxy 設定：

```
bitsadmin /setproxysettings myDownloadJob PRECONFIG
```

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
```
```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80
```

```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
