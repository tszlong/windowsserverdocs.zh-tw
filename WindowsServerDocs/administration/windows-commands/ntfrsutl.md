---
title: ntfrsutl
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 698237380d02fb1ceb4e738c6fb4f083dd31aef3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723459"
---
# <a name="ntfrsutl"></a>ntfrsutl

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

傾印 NT File Replication Service \(NTFRS\)的內部資料表、執行緒和記憶體資訊。 它會針對本機和遠端伺服器執行。 在服務控制管理員\(SCM\)中，NTFRS 的復原設定對於尋找並保留電腦上重要的記錄事件而言，是不可或缺的。 這項工具提供了一種方便的方法來檢查這些設定。   
  
## <a name="syntax"></a>語法  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
#### <a name="parameters"></a>參數  
  
|  參數  |                                                                                                                                                                                                                                                                                                                                        描述                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          識別碼資料表                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  FRS 設定表                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        輸入記錄檔                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        輸出記錄檔                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  指定電腦。                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        記憶體使用量                                                                                                                                                                                                                                                                                                                                        |
|   執行緒   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    stage (階段)    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         列出 NTFRS 服務的 DS 視圖。                                                                                                                                                                                                                                                                                                                          |
|    集合     |                                                                                                                                                                                                                                                                                                                             指定使用中的複本集                                                                                                                                                                                                                                                                                                                              |
|   版本   |                                                                                                                                                                                                                                                                                                                       指定 API 和 NTFRS 服務版本。                                                                                                                                                                                                                                                                                                                        |
|    poll     | 指定目前的輪詢間隔。<p>參數：<p><ul><li>**\/快速輪詢**\[ **\=** \[ <N> \] \] \(  \)<p><ul><li>**快速** \-輪詢，直到 rectrieved 穩定的設定</li><li>快速輪詢每個預設分鐘數。 ** \= ** \-</li><li>**每\=隔 N 分鐘快速地輪詢** *N* \- <N></li></ul></li><li>**\/** \[緩慢**\=** \[ <N>輪詢緩慢\] \] \(\)<p><ul><li>**緩慢** \-的輪詢會變慢，直到抓取穩定的設定</li><li>每個預設分鐘**緩慢\= ** \-輪詢緩慢</li><li>**每\=隔** *N*分鐘快速\-輪詢一次<N></li></ul></li><li>** \/立即** \(輪詢\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                            |
  
## <a name="examples"></a>範例  
若要判斷檔案複寫的輪詢間隔：  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
若要判斷目前的 NTFRS 應用程式\(介面\) API 版本：  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
  
  

