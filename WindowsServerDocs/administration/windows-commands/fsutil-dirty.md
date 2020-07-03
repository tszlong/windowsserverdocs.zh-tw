---
title: fsutil dirty
description: Fsutil dirty 命令的參考文章，它會查詢或設定磁片區的中途位。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c61ab5405fb5b469b6f4513459e4096524f4b7fe
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929269"
---
# <a name="fsutil-dirty"></a>fsutil dirty

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或設定磁片區的中途位。 設定磁片區的中途位時， **autochk**會在下一次電腦重新開機時，自動檢查磁片區是否有錯誤。

## <a name="syntax"></a>語法

```
fsutil dirty {query | set} <volumepath>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 查詢 | 查詢指定的磁片區的中途位。 |
| set | 設定指定磁片區的中途位。 |
| `<volumepath>` | 以下列格式指定磁片磁碟機名稱，後面接著冒號或 GUID： `volume{GUID}` 。 |

#### <a name="remarks"></a>備註

- 磁片區的中途位表示檔案系統可能處於不一致的狀態。 您可以設定變更的位，原因如下：

    - 磁片區已上線，且有未完成的變更。

    - 已對磁片區進行變更，而且電腦已在將變更認可到磁片之前關閉。

    - 在磁片區上偵測到損毀。

- 如果重新開機電腦時設定了中途的位， **chkdsk**就會執行以驗證檔案系統完整性，並嘗試修正磁片區的任何問題。

### <a name="examples"></a>範例

若要查詢磁片磁碟機 C 上的中途位，請輸入：

```
fsutil dirty query c:
```

    If the volume is dirty, the following output displays:

    `Volume C: is dirty`

    If the volume isn't dirty, the following output displays:

    `Volume C: is not dirty`

若要在磁片磁碟機 C 上設定中途位，請輸入：

```
fsutil dirty set C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
