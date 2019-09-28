---
title: 附加 vdisk
description: '**Attach vdisk**的 Windows 命令主題-附加（有時稱為掛接或介面）虛擬硬碟（VHD），使其在主機電腦上顯示為本機硬碟。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382636"
---
# <a name="attach-vdisk"></a>附加 vdisk

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接（有時稱為裝載或介面）虛擬硬碟（VHD），使其在主機電腦上顯示為本機硬碟。 如果 VHD 在您連結時已經有磁碟分割和檔案系統磁片區，則會將磁碟機號指派給 VHD 內的磁片區。
> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>參數

|    參數     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     唯讀     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             將 VHD 附加為唯讀。 任何寫入作業都會傳回錯誤。                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sd = <SDDL string> | 設定 VHD 上的使用者篩選。 篩選字串必須是安全描述項定義語言（SDDL）格式。 根據預設，使用者篩選允許實體磁片上的存取。<br /><br />SDDL 字串可能很複雜，但在其最簡單的形式中，保護存取權的安全描述項稱為任意存取控制清單（DACL）。 其形式如下：D： < dacl_flags > < string_ace1 > < string_ace2 > .。。< string_acen ><br /><br />常見的 DACL 旗標如下：<br /><br />@no__t **-0 允許**存取<br />-   **D**拒絕存取<br /><br />一般許可權如下：<br /><br />-   **GA**所有存取<br />-   **GR**讀取權限<br />-   **GW**寫入權限<br /><br />一般使用者帳戶如下：<br /><br />-   **BA**內建系統管理員<br />-   **AU**驗證使用者<br />-   **CO** Creator 擁有者<br />-   **WD** -Everyone<br /><br />例如：<br /><br />**D:P：（A;;）GR;;;AU**提供所有已驗證使用者的讀取權限<br /><br />**D:P：（A;;）GA;;;WD**讓所有人都能完整地進行 |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          指定應該在 VHD 上使用 .vhd 檔案的安全描述項。 如果未指定**Usefilesd**參數，VHD 將不會有明確的安全描述項，除非以**Sd**參數指定。                                                                                                                                                                                                                                                                                                                                                                                          |
|      noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           僅用於腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>備註
- 必須選取並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。
  ## <a name="BKMK_Examples"></a>典型
  若要將選取的 VHD 附加為唯讀，請輸入：
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [compact vdisk](compact-vdisk.md)

- [詳細資料 vdisk](detail-vdisk.md)
- [卸離 vdisk](detach-vdisk.md)
- [展開 vdisk](expand-vdisk.md)
- [合併 vdisk](merge-vdisk.md)
- [選取 vdisk](select-vdisk.md)
- [list_1](list_1.md)
