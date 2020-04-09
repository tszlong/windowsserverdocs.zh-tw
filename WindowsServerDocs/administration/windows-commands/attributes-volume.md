---
title: 屬性磁片區
description: '**屬性磁片**區的 Windows 命令主題，它會顯示、設定或清除磁片區的屬性。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00991cdba57f0728cfa348dea2b0916ad758b34a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851231"
---
# <a name="attributes-volume"></a>屬性磁片區

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示、設定或清除磁片區的屬性。

## <a name="syntax"></a>語法  

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
### <a name="parameters"></a>參數  
  
| 參數 | 描述 |  
| ------- | -------- |  
| 設定 | 設定具有焦點之磁片區的指定屬性。 |  
| 清除 | 清除具有焦點之磁片區的指定屬性。 |  
| readonly | 指定磁碟區為唯讀。 |  
| hidden | 指定磁碟區為隱藏。 |  
| nodefaultdriveletter | 指定磁碟區根據預設不接受磁碟機代號。 |  
| shadowcopy | 指定磁碟區為陰影複製磁碟區。 |  
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |  
  
## <a name="remarks"></a>備註  
  
- 在基本的主開機記錄（MBR）磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數適用于磁片上的所有磁片區。  
  
- 在基本 GUID 磁碟分割表格（GPT）磁片上，以及在動態 MBR 和 GPT 磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數只適用于選取的磁片區。  
  
- 必須選取磁片區，**屬性 volume**命令才會成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。  
  
## <a name="examples"></a><a name=BKMK_examples></a>典型

若要在選取的磁片區上顯示目前的屬性，請輸入：  
  
```
attributes volume  
```  
  
若要將選取的磁片區設定為隱藏和唯讀，請輸入：  
  
```
attributes volume set hidden readonly  
```  
  
若要移除所選磁片區上的隱藏和唯讀屬性，請輸入：  
  
```
attributes volume clear hidden readonly  
```  
  
## <a name="additional-references"></a>其他參考資料  

- [命令列語法關鍵](command-line-syntax-key.md)