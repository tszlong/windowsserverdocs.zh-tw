---
title: bitsadmin setproxysettings
description: Bitsadmin setproxysettings 命令的參考文章，此命令會設定指定工作的 proxy 設定。
ms.topic: reference
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 199cb3f4b4259a52a8960cac23b9e408e71ded23
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630688"
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
| 作業 | 作業的顯示名稱或 GUID。 |
| usage | 設定 proxy 使用方式，包括：<ul><li>**PRECONFIG.** 使用擁有者的 Internet Explorer 預設值。</li><li>**NO_PROXY。** 請勿使用 proxy 伺服器。</li><li>**覆蓋。** 使用明確的 proxy 清單和略過清單。 Proxy 清單和 proxy 略過資訊必須遵循。</li><li>**自動.** 自動偵測 proxy 設定。</li></ul> |
| list | 當 *Usage* 參數設定為 OVERRIDE 時使用。 必須包含要使用的 proxy 伺服器清單（以逗號分隔）。 |
| 略過 | 當 *Usage* 參數設定為 OVERRIDE 時使用。 必須包含以空格分隔的主機名稱或 IP 位址清單，或兩者都不會透過 proxy 路由傳送。 這可以 `<local>` 參考相同 LAN 上的所有伺服器。 Null 的值可用於空的 proxy 略過清單。 |

## <a name="examples"></a>範例

若要使用名為 *myDownloadJob*之作業的各種使用方式選項來設定 proxy 設定：

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
