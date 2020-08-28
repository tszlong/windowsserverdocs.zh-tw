---
title: extend
description: 擴充命令的參考文章，此命令會將具有焦點的磁片區或磁碟分割和其檔案系統延伸至可用 (未配置的磁片上) 空間。
ms.topic: reference
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bef77ab0972390dcae85f46458989410b88cc64a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036666"
---
# <a name="extend"></a>extend

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將具有焦點和其檔案系統的磁片區或磁碟分割延伸至磁片上的可用 (未配置) 空間。

## <a name="syntax"></a>語法

```
extend [size=<n>] [disk=<n>] [noerr]
extend filesystem [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定要新增至目前磁片區或磁碟分割的空間量（以 mb 為單位） (MB) 。 如果未指定大小，則會使用磁片上所有可用的連續可用空間。 |
| 磁片 =`<n>` | 指定擴充磁片區或磁碟分割的磁片。 如果未指定任何磁片，則磁片區或磁碟分割會在目前的磁片上擴充。 |
| filesystem | 以焦點擴充磁片區的檔案系統。 僅適用于檔案系統未使用磁片區擴充的磁片。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

#### <a name="remarks"></a>備註

- 在基本磁碟上，可用空間必須與具有焦點的磁片區或磁碟分割位於相同磁片上。 它也必須緊接在具有焦點的磁片區或磁碟分割 (也就是說，它必須在下一個磁區位移) 開始。

- 在具有簡單或跨區磁片區的動態磁碟上，磁片區可延伸到任何動態磁碟上的任何可用空間。 您可以使用此命令將簡單動態磁碟區轉換成跨距動態磁碟區。 鏡像、RAID-5 和等量磁片區無法擴充。

- 如果磁碟分割先前是使用 NTFS 檔案系統格式化，檔案系統會自動擴充以填滿較大的磁碟分割，且不會遺失任何資料。

- 如果磁碟分割先前是使用 NTFS 以外的檔案系統進行格式化，則命令會失敗，而且不會變更磁碟分割。

- 如果資料分割之前未使用檔案系統格式化，磁碟分割仍會擴充。

- 分割區必須有相關聯的磁片區，才可加以擴充。

## <a name="examples"></a>範例

若要以 500 mb 的焦點擴充磁片區或磁碟分割，請在磁片3上輸入：

```
extend size=500 disk=3
```

若要在擴充磁片區之後擴充其檔案系統，請輸入：

```
extend filesystem
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
