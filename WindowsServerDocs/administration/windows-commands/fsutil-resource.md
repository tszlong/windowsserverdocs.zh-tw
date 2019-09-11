---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Fsutil 資源
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ea97f7f71d1b484c7ac63c7c429f291fba607bba
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867064"
---
# <a name="fsutil-resource"></a>Fsutil 資源
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7、Windows 2008、Windows Vista

建立次要交易式 Resource Manager、啟動或停止交易式 Resource Manager，或顯示交易式 Resource Manager 的相關資訊，並修改下列行為：

-   預設的交易 Resource Manager 是否會在下一次掛接時清除其交易中繼資料

-   指定的交易 Resource Manager，以優先使用可用性

-   指定的交易 Resource Manager，以優先使用一致的可用性

-   執行中交易式 Resource Manager 的特性

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

### <a name="parameters"></a>參數

|        參數        |                                                                                                                                                                                                                                        描述                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         建立          |                                                                                                                                                                                                                    建立次要交易式 Resource Manager。                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        指定交易式 Resource Manager 根目錄的完整路徑。                                                                                                                                                                                                         |
|          資訊           |                                                                                                                                                                                                            顯示指定的交易式 Resource Manager 資訊。                                                                                                                                                                                                            |
|      setautoreset       | 指定預設的交易式 Resource Manager 是否會在下一次掛接時清除交易式中繼資料。<br /><br />-將**setautoreset**參數設定為**true** ，以指定交易 Resource Manager 將會在下一次掛接時清除事務中繼資料（預設為）。<br />-將**setautoreset**參數設定為**false** ，以指定交易 Resource Manager 不會在下一次掛接時清除事務中繼資料（預設為）。 |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       指定磁片磁碟機名稱，後面接著冒號。                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 指定交易式 Resource Manager 會偏好一致性的可用性。                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 指定交易式 Resource Manager 會優先使用一致性而不是可用性。                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  變更已在執行中之交易式 Resource Manager 的特性。                                                                                                                                                                                                  |
|         趨勢          |                                                                                                  指定交易 Resource Manager 記錄可成長的數量。<br /><br />成長參數可以指定如下：<br /><br />-使用下列格式的容器數目：_容器_**容器**<br />-使用下列格式的百分比：_百分比_**百分比**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      指定交易 Resource Manager 所使用的資料物件。                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                指定指定交易 Resource Manager 的容器數目上限。                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                指定指定交易 Resource Manager 的容器數目下限。                                                                                                                                                                                                |
|  模式 {完整&#124;復原}  |                                                                                                                                                                                        指定是否記錄所有交易（ **full**），或只記錄回復事件（**復原**）。                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  變更交易式 Resource Manager 的 GUID。                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              指定交易 Resource Manager 記錄可自動減少的百分比。                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              將交易 Resource Manager 的大小指定為指定的*容器*數目。                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    啟動指定的交易式 Resource Manager。                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    停止指定的交易式 Resource Manager。                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>典型
若要設定 c:\test 指定之交易式 Resource Manager 的記錄，使其自動成長為五個容器，請輸入：

```
fsutil resource setlog growth 5 containers c:test
```

若要設定 c:\test 指定之交易式 Resource Manager 的記錄，使其自動成長為兩%，請輸入：

```
fsutil resource setlog growth 2 percent c:test
```

若要指定預設的交易 Resource Manager 將在磁片磁碟機 C 上的下一個掛接中清除交易中繼資料，請輸入：

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[交易式 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


