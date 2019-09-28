---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectid
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 509e58b85842826b71cb1bfed72ae4c7e5337e25
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376829"
---
# <a name="fsutil-objectid"></a>Fsutil objectid
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

管理物件識別元（Oid），這是分散式連結追蹤（DLT）用戶端服務和檔案複寫服務（FRS）用來追蹤其他物件（例如檔案、目錄和連結）的內建物件。 大部分的程式都不會看到物件識別碼，而且絕對不能修改。

> [!CAUTION]
> 請勿刪除、設定或以其他方式修改物件識別碼。 刪除或設定物件識別碼可能會導致從檔案的某些部分遺失資料，最多包括和包含整個資料量。 此外，您可能會在分散式連結追蹤（DLT）用戶端服務和檔案複寫服務（FRS）中造成不良的行為。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|create|如果指定的檔案不存在，則建立物件識別碼。 如果檔案已經有物件識別碼，這個子命令就相當於**查詢**子命令。|
|delete|刪除物件識別碼。|
|query|查詢物件識別碼。|
|設定|設定物件識別碼。|
|\<ObjectID >|設定特定檔案的16位元組十六進位識別碼，保證在磁片區中是唯一的。 [分散式連結追蹤（DLT）] 用戶端服務和 [檔案複寫服務（FRS）] 會使用物件識別碼來識別檔案。|
|\<BirthVolumeID >|指出第一次取得物件識別碼時，檔案所在的磁片區。 此值是 DLT 用戶端服務所使用的16位元組十六進位識別碼。|
|\<BirthObjectID >|指出檔案的原始物件識別碼（移動檔案時， *ObjectID*可能會變更）。 此值是 DLT 用戶端服務所使用的16位元組十六進位識別碼。|
|\<DomainID >|16位元組的十六進位網域識別碼。 目前未使用這個值，而且必須設定為所有零。|
|\<檔案名 >|指定檔案的完整路徑，包括檔案名和副檔名，例如 C:\documents\filename.txt。|

## <a name="remarks"></a>備註

-   任何具有物件識別碼的檔案，也會有出生的磁片區識別碼、出生物件識別碼和網域識別碼。 當您移動檔案時，物件識別碼可能會變更，但出生卷和出生物件識別碼會維持不變。 這種行為可讓 Windows 作業系統一律尋找檔案，不論檔案移動位置為何。

## <a name="BKMK_examples"></a>典型
若要建立物件識別碼，請輸入：

`fsutil objectid create c:\temp\sample.txt`

若要刪除物件識別碼，請輸入：

`fsutil objectid delete c:\temp\sample.txt`

若要查詢物件識別碼，請輸入：

`fsutil objectid query c:\temp\sample.txt`

若要設定物件識別碼，請輸入：

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


