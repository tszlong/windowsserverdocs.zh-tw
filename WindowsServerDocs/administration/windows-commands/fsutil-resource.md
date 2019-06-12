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
ms.openlocfilehash: 38b0947171b7fd8afc44a95b2244a3fd2a0c9e73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439041"
---
# <a name="fsutil-resource"></a>Fsutil 資源
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7、 Windows 2008，Windows Vista

建立次要交易式資源管理員、 啟動或停止交易式資源管理員，或顯示交易式資源管理員的相關資訊並修改下列行為：

-   交易式資源管理員是否預設會清除其交易式的中繼資料，在下一步 的掛接

-   指定交易式資源管理員的一致性，而不用可用性

-   指定交易資源管理員，來把可用性擺在一致性

-   執行交易式資源管理員的特性

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
|         建立          |                                                                                                                                                                                                                    建立次要交易式資源管理員。                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        指定交易式資源管理員根目錄的完整路徑。                                                                                                                                                                                                         |
|          資訊           |                                                                                                                                                                                                            顯示指定交易式資源管理員的資訊。                                                                                                                                                                                                            |
|      setautoreset       | 指定交易式資源管理員預設是否會清除交易的中繼資料，在下一步 掛接。<br /><br />-設定**setautoreset**參數來 **，則為 true**指定交易資源管理員會清除預設的交易式的中繼資料，在下一步 的掛接。<br />-設定**setautoreset**參數來**false**指定交易資源管理員將會清除交易的中繼資料，在下一個掛接，根據預設。 |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       指定磁碟機名稱，後面接著冒號。                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 指定交易式資源管理員將會把可用性擺在一致性。                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 指定交易式資源管理員會偏好一致性，而非可用性。                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  變更交易式資源管理員的已執行的特性。                                                                                                                                                                                                  |
|         成長          |                                                                                                  指定交易式資源管理員記錄檔可以成長的數量。<br /><br />可以指定成長參數如下所示：<br /><br />-使用格式的容器數目：*容器***容器**<br />-   使用格式的百分比：*Percent***percent**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      指定交易式資源管理員所使用的資料物件。                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                指定容器的最大數目，指定交易式資源管理員。                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                指定容器的最小數目，指定交易式資源管理員。                                                                                                                                                                                                |
|  mode {full&#124;undo}  |                                                                                                                                                                                        指定是否記錄所有交易 (**完整**) 或只回復事件會都記錄 (**復原**)。                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  變更 GUID 的交易式資源管理員。                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              指定的交易式資源管理員記錄檔可以自動減少的百分比。                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              指定的數字的指定大小的交易式資源管理員*容器*。                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    啟動指定的交易式資源管理員。                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    停止指定的交易式資源管理員。                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>範例
若要將記錄設定為交易式資源管理員 c:\test，具有自動成長的五個容器，所指定輸入：

```
fsutil resource setlog growth 5 containers c:test
```

若要設定記錄，交易式資源管理員由 c:\test，有兩個百分比自動成長，輸入：

```
fsutil resource setlog growth 2 percent c:test
```

若要指定預設的交易式資源管理員會清除在下一步 的掛接磁碟機 C 上的交易式中繼資料，請輸入：

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[交易式 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


