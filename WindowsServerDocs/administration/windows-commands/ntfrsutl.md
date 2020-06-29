---
title: ntfrsutl
description: Ntfrsutl 命令的參考主題，它會傾印 NT 檔案複寫服務（NTFRS）的內部資料表、執行緒和記憶體資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f931d916888a372d66a1cc06cb7543067b9b9d3b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472764"
---
# <a name="ntfrsutl"></a>ntfrsutl

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從本機和遠端伺服器轉儲 NT 檔案複寫服務（NTFRS）的內部資料表、執行緒和記憶體資訊。 在服務控制管理員（SCM）中，NTFRS 的復原設定對於尋找並保留電腦上重要的記錄事件而言，是不可或缺的。 這項工具提供了一種方便的方法來檢查這些設定。

## <a name="syntax"></a>語法

```
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]
ntfrsutl[memory|threads|stage][<computer>]
ntfrsutl ds[<computer>]
ntfrsutl [sets][<computer>]
ntfrsutl [version][<computer>]
ntfrsutl poll[/quickly[=[<n>]]][/slowly[=[<n>]]][/now][<computer>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| idtable | 指定識別碼資料表。 |
| configtable | 指定 FRS 設定資料表。 |
| inlog | 指定輸入記錄檔。 |
| outlog | 指定輸出記錄檔。 |
| `<computer>` | 指定電腦。 |
| memory | 指定記憶體使用量。 |
| 執行緒 | 指定記憶體使用量。 |
| stage (階段) | 指定記憶體使用量。 |
| ds | 列出 NTFRS 服務的 DS 視圖。 |
| 集合 | 指定使用中的複本集。 |
| version | 指定 API 和 NTFRS 服務版本。 |
| poll | 指定目前的輪詢間隔。<ul><li>`/quickly`-快速輪詢，直到它抓取穩定的設定。</li><li>`/quickly=`-快速輪詢每個預設分鐘數。</li><li>`/quickly=<n>`-每隔*n*分鐘會快速輪詢。</li><li>`/slowly`-輪詢變慢，直到它抓取穩定的設定。</li><li>`/slowly=`-每隔預設分鐘數會慢慢輪詢。</li><li>`/slowly=<n>`-每隔*n*分鐘會慢慢輪詢。</li><li>`/now`-立即輪詢。</li></ul>|
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要判斷檔案複寫的輪詢間隔，請輸入：

```
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1
```

若要判斷目前的 NTFRS 應用程式介面（API）版本，請輸入：

```
C:\Program Files\SupportTools>ntfrsutl version
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
