---
title: nslookup root
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11179ff3cd22acd9df67261e7ab752aa159201a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838641"
---
# <a name="nslookup-root"></a>nslookup root

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將伺服器的預設伺服器變更為網域名稱系統（DNS）功能變數名稱空間的根目錄。
## <a name="syntax"></a>語法
```
root 
```
### <a name="parameters"></a>參數

|    參數    |                      描述                      |
|-----------------|-------------------------------------------------------|
| {help &#124; ？} | 顯示**nslookup**子命令的簡短摘要。 |

## <a name="remarks"></a>備註
- 目前會使用 ns.nic.ddn.mil 名稱伺服器。 此命令是 lserver ns.nic.ddn.mil 的同義字。 您可以使用 [**設定根**] 命令來變更根功能變數名稱伺服器的名稱。
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 集根](nslookup-set-root.md)
