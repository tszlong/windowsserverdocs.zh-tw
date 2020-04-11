---
title: bitsadmin setproxysettings
description: 適用于**bitsadmin setproxysettings**的 Windows 命令主題，其會設定指定之作業的 proxy 設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea92383d9bd09372d21d3c1da84db060b0a9958
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122759"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

設定指定之作業的 proxy 設定。

## <a name="syntax"></a>語法

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| usage | 設定 proxy 使用方式，包括：<ul><li>**LNK-WHAT-ARE-PRECONFIG-SOLUTIONS.** 使用擁有者的 Internet Explorer 預設值。</li><li>**NO_PROXY。** 不要使用 proxy 伺服器。</li><li>**覆寫.** 使用明確的 proxy 清單和略過清單。 Proxy 清單和 proxy 略過資訊必須遵循。</li><li>**檢測.** 會自動偵測 proxy 設定。</li></ul> |
| list | 當*Usage*參數設定為 OVERRIDE 時使用。 必須包含要使用的 proxy 伺服器清單（以逗號分隔）。 |
| 不必 | 當*Usage*參數設定為 OVERRIDE 時使用。 必須包含以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以 `<local>` 來參考相同 LAN 上的所有伺服器。 Null 的值可用於空的 proxy 略過清單。 |

## <a name="examples"></a>範例

下列範例會設定名為*myDownloadJob*之作業的 proxy 設定。

```
C:\>bitsadmin /setproxysettings myDownloadJob PRECONFIG
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