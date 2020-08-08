---
title: 附加 vdisk
description: Attach vdisk 命令的參考文章，其附加 (有時稱為裝載或介面) 虛擬硬碟 (VHD) ，以便在主機電腦上顯示為本機硬碟。
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30ef29b0a8bbfed550cfa8c09a94c7b64c09b699
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993039"
---
# <a name="attach-vdisk"></a>附加 vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接 (有時稱為裝載或介面) 虛擬硬碟 (VHD) ，使其在主機電腦上顯示為本機硬碟。 如果 VHD 在您連結時已經有磁碟分割和檔案系統磁片區，則會將磁碟機號指派給 VHD 內的磁片區。

> [!IMPORTANT]
> 您必須選擇並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。

## <a name="syntax"></a>語法

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| readonly | 將 VHD 附加為唯讀。 任何寫入作業都會傳回錯誤。 |
| `sd=<SDDL string>` | 設定 VHD 上的使用者篩選。 篩選字串必須是安全描述項定義語言 (SDDL) 格式。 根據預設，使用者篩選允許實體磁片上的存取。 SDDL 字串可能很複雜，但在其最簡單的形式中，保護存取權的安全描述項稱為任意存取控制清單， (DACL) 。 它會使用以下格式： `D:<dacl_flags><string_ace1><string_ace2>` .。。`<string_acen>`<p>常見的 DACL 旗標如下：<ul><li>**A**。 允許存取</li><li>**D.** 拒絕存取</li></ul>一般許可權如下：<ul><li>**GA**。 所有存取</li><li>**GR**。 讀取存取權</li><li> **GW**。 寫入權限</li></ul>一般使用者帳戶如下：<ul><li>**BA**。 內建系統管理員</li><li>**AU**。 驗證的使用者</li><li>**CO**。 Creator Owner</li><li>**WD**。 所有人</li></ul>範例：<ul><li>**D:P： (;;GR;;;AU**。 提供所有已驗證使用者的讀取權限。</li><li>**D:P： (;;GA;;;WD**。 提供所有人完整存取權。</li></ul> |
| usefilesd | 指定應該在 VHD 上使用 .vhd 檔案的安全描述項。 如果未指定**Usefilesd**參數，VHD 將不會有明確的安全描述項，除非以**Sd**參數指定。 |
| noerr | 僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要將選取的 VHD 附加為唯讀，請輸入：

```
attach vdisk readonly
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [select vdisk](select-vdisk.md)

- [compact vdisk](compact-vdisk.md)

- [detail vdisk](detail-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [expand vdisk](expand-vdisk.md)

- [merge vdisk](merge-vdisk.md)

- [list](./list.md)