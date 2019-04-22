---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectid
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813739"
---
# <a name="fsutil-objectid"></a>Fsutil objectid
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理物件識別項 (Oid)，也就是用來追蹤其他物件，例如檔案、 目錄和連結的分散式連結追蹤 (DLT) 用戶端服務和檔案複寫服務 (FRS) 的內部物件。 物件識別項看不到大部分的程式，您不應該修改。

> [!CAUTION]
> 請勿刪除、 設定，或修改其物件識別碼。 刪除或設定物件識別碼可以產生的檔案，包括整個磁碟區資料的部分資料遺失。 此外，您可能會造成不良的行為 「 分散式連結追蹤 (DLT) 用戶端服務 」 和 「 檔案複寫服務 (FRS)。

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
|建立|如果指定的檔案還沒有其中一個，請建立物件識別項。 如果檔案已經有物件識別碼，這個子命令相當於**查詢**子命令。|
|[刪除]|刪除的物件識別碼。|
|查詢|查詢的物件識別碼。|
|設定|設定物件識別碼。|
|\<ObjectID>|設定特定檔案的 16 位元組的十六進位識別碼，保證是唯一的磁碟區。 分散式連結追蹤 (DLT) 用戶端服務和檔案複寫服務 (FRS) 使用物件識別碼來識別檔案。|
|\<BirthVolumeID>|表示磁碟區上的檔案是位於時首次取得物件識別碼。 這個值是 16 位元組的十六進位識別碼，以供 DLT 用戶端服務。|
|\<BirthObjectID>|指出檔案的原始物件識別項 ( *ObjectID*在移動檔案時可能會變更)。 這個值是 16 位元組的十六進位識別碼，以供 DLT 用戶端服務。|
|\<DomainID>|16 位元組十六進位的網域識別碼。 此值目前未使用，且必須設為全部為零。|
|\<FileName>|指定包含檔案名稱和副檔名，例如 C:\documents\filename.txt 檔案的完整路徑。|

## <a name="remarks"></a>備註

-   物件識別碼的任何檔案也會有出生磁碟區識別碼、 生日物件識別碼和網域識別項。 當您移動檔案時，可能會變更的物件識別項，但出生的磁碟區和生日物件識別碼維持不變。 此行為可啟用 Windows 作業系統，總是找出檔案，不論其中它已移動。

## <a name="BKMK_examples"></a>範例
若要建立的物件識別碼，請輸入：

`fsutil objectid create c:\temp\sample.txt`

若要刪除的物件識別碼，請輸入：

`fsutil objectid delete c:\temp\sample.txt`

若要查詢的物件識別碼，請輸入：

`fsutil objectid query c:\temp\sample.txt`

若要設定的物件識別碼，請輸入：

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


