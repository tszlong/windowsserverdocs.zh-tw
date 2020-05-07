---
title: Sc.exe 查詢
description: 瞭解如何使用 sc.exe 公用程式取得服務、驅動程式、服務類型或驅動程式類型的相關資訊
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86aabbbc42c965b72f317a3bfaa99acc99c46f3b
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850039"
---
# <a name="scexe-query"></a>Sc.exe 查詢

取得並顯示指定之服務、驅動程式、服務類型或驅動程式類型的相關資訊。

## <a name="syntax"></a>語法

```
sc.exe [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

### <a name="parameters"></a>參數

|       參數        |                                                                                                                          描述                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例（UNC）格式（ \\ \\例如 myserver）。 若要在本機執行 SC.EXE，請省略此參數。                        |
|     \<ServiceName>     |                                      指定**getkeyname**作業所傳回的服務名稱。 此**查詢**參數不會與其他**查詢**參數搭配使用（除了*ServerName*以外）。                                      |
|     類型 = {driver      |                                                                                                                            服務                                                                                                                            |
|       類型 = {自有       |                                                                                                                             分享                                                                                                                             |
|     state = {active     |                                                                                                                           非使用中                                                                                                                            |
| bufsize = \<BufferSize> |                     指定列舉緩衝區的大小（以位元組為單位）。 預設緩衝區大小為1024個位元組。 當查詢所產生的顯示超過1024個位元組時，您應該增加列舉緩衝區的大小。                      |
|   ri = \<ResumeIndex>   | 指定列舉開始或繼續的索引編號。 預設值為**0** （零）。 當查詢傳回的資訊超過預設緩衝區可顯示的數目時，請使用此參數搭配**bufsize =** 參數。 |
|  群組 = \<組名>   |                                                                             指定要列舉的服務群組。 根據預設，系統會列舉所有群組（* * group = * *）。                                                                              |
|           /?           |                                                                                                             在命令提示字元顯示說明。                                                                                                              |

## <a name="remarks"></a>備註

- 如果參數與其值之間沒有空格（也就是**type = 自有**，而不是**type = 自有**），作業將會失敗。
- **查詢**作業會顯示服務的下列相關資訊： SERVICE_NAME （服務的登錄子機碼名稱）、類型、狀態（以及無法使用的狀態）、WIN32_EXIT_B、SERVICE_EXIT_B、檢查點和 WAIT_HINT。
- 在某些情況下， **type =** 參數可以使用兩次。 **Type =** 參數的第一個外觀指定要查詢服務、驅動程式或兩者（**全部**）。 **Type =** 參數的第二個外觀會從**create**作業指定類型，以進一步縮小查詢範圍。
- 當**查詢**命令所產生的顯示超過列舉緩衝區的大小時，會顯示類似下列的訊息：  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  若要顯示其餘的**查詢**資訊，請重新執行**查詢**，將**bufsize =** 設定為位元組數目，並將**ri =** 設定為指定的索引。 例如，在命令提示字元中輸入下列內容，就會顯示其餘的輸出：  
  ```
  sc.exe query bufsize= 1822 ri= 79
  ```

## <a name="examples"></a>範例

若只要顯示作用中服務的資訊，請輸入下列其中一個命令：
```
sc.exe query
sc.exe query type= service
```
若要顯示作用中服務的資訊，並指定2000個位元組的緩衝區大小，請輸入：
```
sc.exe query type= all bufsize= 2000
```
若要顯示 WUAUSERV 服務的資訊，請輸入：
```
sc.exe query wuauserv
```
若要顯示所有服務（作用中和非作用中）的資訊，請輸入：
```
sc.exe query state= all
```
若要顯示所有服務（作用中和非作用中）的資訊，請從第56行開始，輸入：
```
sc.exe query state= all ri= 56
```
若要顯示互動式服務的資訊，請輸入：
```
sc.exe query type= service type= interact
```
若只要顯示驅動程式的資訊，請輸入：
```
sc.exe query type= driver
```
若要顯示 [網路驅動程式介面規格（NDIS）] 群組中驅動程式的資訊，請輸入：
```
sc.exe query type= driver group= ndis
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
