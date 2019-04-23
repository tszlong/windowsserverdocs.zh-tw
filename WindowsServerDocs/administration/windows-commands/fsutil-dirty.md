---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: fsutil 已變更
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c308b0497a5a39a25384b22441b733143df8727b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852129"
---
# <a name="fsutil-dirty"></a>fsutil 已變更
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查詢，或設定磁碟區的已變更位元。 磁碟區的已變更時設定位元**autochk**會自動檢查是否有錯誤的磁碟區在下次重新啟動電腦。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|查詢|查詢指定的磁碟區已變更位元。|
|設定|設定指定的磁碟區已變更位元。|
|\<VolumePath>|指定磁碟機名稱後面加上冒號或 GUID 格式如下：**磁碟區 {***GUID***}**。|

## <a name="remarks"></a>備註

-   磁碟區的已變更位元表示檔案系統可能處於不一致的狀態。 您可以設定已變更位元，因為：

    -   磁碟區在線上，而且它有未處理的變更。

    -   磁碟區已變更，並在電腦關機之前所做的變更已認可至磁碟。

    -   偵測到損毀磁碟區上。

-   如果在電腦重新啟動時，設定已變更位元**chkdsk**執行以驗證檔案系統的完整性，並嘗試修正任何問題，與磁碟區。

## <a name="BKMK_examples"></a>範例
若要查詢已變更的位元 C 磁碟機上，請輸入：

```
fsutil dirty query c:
```

-   如果磁碟區已變更，則會顯示下列輸出：

    `Volume C: is dirty`

-   如果磁碟區未變更，則會顯示下列輸出：

    `Volume C: is not dirty`

若要設定已變更位元 C 磁碟機上，輸入：

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


