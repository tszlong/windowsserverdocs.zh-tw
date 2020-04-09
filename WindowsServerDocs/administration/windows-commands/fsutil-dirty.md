---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: Fsutil dirty
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: cf3685bae9ed76ede4da6df244139437d92250c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844331"
---
# <a name="fsutil-dirty"></a>Fsutil dirty
>適用于： Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

查詢或設定磁片區的中途位。 設定磁片區的中途位時， **autochk**會在下一次電腦重新開機時，自動檢查磁片區是否有錯誤。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil dirty {query | set} <VolumePath>
```

### <a name="parameters"></a>參數

|   參數   |                                                 描述                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     query     |                                  查詢指定的磁片區的中途位。                                   |
|      設定      |                                    設定指定磁片區的中途位。                                    |
| \<VolumePath > | 以下列格式指定磁片磁碟機名稱，後面接著冒號或 GUID：**磁片區 {** <em>GUID</em> **}** 。 |

## <a name="remarks"></a>備註

-   磁片區的中途位表示檔案系統可能處於不一致的狀態。 您可以設定變更的位，原因如下：

    -   磁片區已上線，且有未完成的變更。

    -   已對磁片區進行變更，而且電腦已在將變更認可到磁片之前關閉。

    -   在磁片區上偵測到損毀。

-   如果重新開機電腦時設定了中途的位， **chkdsk**就會執行以驗證檔案系統完整性，並嘗試修正磁片區的任何問題。

## <a name="examples"></a><a name="BKMK_examples"></a>典型
若要查詢磁片磁碟機 C 上的中途位，請輸入：

```
fsutil dirty query c:
```

-   如果磁片區已變更，則會顯示下列輸出：

    `Volume C: is dirty`

-   如果磁片區未變更，則會顯示下列輸出：

    `Volume C: is not dirty`

若要在磁片磁碟機 C 上設定中途位，請輸入：

```
fsutil dirty set C:
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


