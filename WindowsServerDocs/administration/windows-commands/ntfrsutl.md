---
title: ntfrsutl
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372619"
---
# <a name="ntfrsutl"></a>ntfrsutl

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

傾印 NT 檔案複寫服務 \(NTFRS\)的內部資料表、執行緒和記憶體資訊。 它會針對本機和遠端伺服器執行。 在服務控制管理員中，NTFRS 的復原設定 \(SCM\) 對於尋找並保留電腦上重要的記錄事件而言，是不可或缺的。 這項工具提供了一種方便的方法來檢查這些設定。   
  
## <a name="syntax"></a>語法  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Parameters  
  
|  參數  |                                                                                                                                                                                                                                                                                                                                        描述                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          識別碼資料表                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  FRS 設定表                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        輸入記錄檔                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        輸出記錄檔                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  指定電腦。                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        記憶體使用狀況                                                                                                                                                                                                                                                                                                                                        |
|   串接   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    階段    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     Ds      |                                                                                                                                                                                                                                                                                                                         列出 NTFRS 服務的 DS 視圖。                                                                                                                                                                                                                                                                                                                          |
|    設定     |                                                                                                                                                                                                                                                                                                                             指定使用中的複本集                                                                                                                                                                                                                                                                                                                              |
|   版本   |                                                                                                                                                                                                                                                                                                                       指定 API 和 NTFRS 服務版本。                                                                                                                                                                                                                                                                                                                        |
|    檢測     | 指定目前的輪詢間隔。<br /><br />參數：<br /><br /><ul><li>**\/快速**\[ **\=** \[ <N>\]\]會快速輪詢 \(  \)<br /><br /><ul><li>**快速 \- 輪詢**，直到 rectrieved 穩定的設定為止</li><li>**快速\=** \- 在每個預設分鐘快速輪詢。</li><li>**快速\=** <N> \- 每*N*分鐘快速輪詢一次</li></ul></li><li>**\/緩慢**\[ **\=** \[ <N>\]\] \(輪詢緩慢\)<br /><br /><ul><li>**緩慢**的 \- 輪詢會變慢，直到抓取穩定的設定</li><li>**緩慢\=** \- 每個預設分鐘會緩慢輪詢</li><li>**緩慢\=** <N> \- 每隔*N*分鐘快速輪詢一次</li></ul></li><li>現在 **\/** \(立即輪詢\)</li></ul> |
|     \/？     |                                                                                                                                                                                                                                                                                                                            在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>典型  
若要判斷檔案複寫的輪詢間隔：  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
若要判斷目前的 NTFRS 應用程式介面 \(API\) 版本：  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
  
  

