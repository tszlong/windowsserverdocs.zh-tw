---
title: 附加 vdisk
description: 連結**vdisk**的 Windows 命令主題，它會附加（有時稱為掛接或介面）虛擬硬碟（VHD），使其在主機電腦上顯示為本機硬碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3a903ed231e34ac902ce10b5342f27e772ac89f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851261"
---
# <a name="attach-vdisk"></a>附加 vdisk

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接（有時稱為裝載或介面）虛擬硬碟（VHD），使其在主機電腦上顯示為本機硬碟。 如果 VHD 在您連結時已經有磁碟分割和檔案系統磁片區，則會將磁碟機號指派給 VHD 內的磁片區。

> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| readonly | 將 VHD 附加為唯讀。 任何寫入作業都會傳回錯誤。 |
| `sd=<SDDL string>` | 設定 VHD 上的使用者篩選。 篩選字串必須是安全描述項定義語言（SDDL）格式。 根據預設，使用者篩選允許實體磁片上的存取。 SDDL 字串可能很複雜，但在其最簡單的形式中，保護存取權的安全描述項稱為任意存取控制清單（DACL）。 其格式為： `D:<dacl_flags><string_ace1><string_ace2>`... `<string_acen>`<p>常見的 DACL 旗標如下：<p>-  **。** 允許存取<p>- **D**。拒絕存取<p>一般許可權如下：<p>- **GA**。 所有存取<p>- **GR**。 讀取存取權<p>- **GW**。 寫入權限<p>一般使用者帳戶如下：<p>- **BA**。 內建系統管理員<p>- **AU**。 Authenticated Users<p>- **CO**。 Creator Owner<p>- **WD**。 每個人<p>範例：<p>**D:P：（A;;）GR;;;AU**提供所有已驗證使用者的讀取權限。<p>**D:P：（A;;）GA;;;WD**提供所有人完整的存取權。 |
| usefilesd | 指定應該在 VHD 上使用 .vhd 檔案的安全描述項。 如果未指定**Usefilesd**參數，VHD 將不會有明確的安全描述項，除非以**Sd**參數指定。 |
| noerr | 僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註

- 必須選取並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

若要將選取的 VHD 附加為唯讀，請輸入：

```
attach vdisk readonly
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [compact vdisk](compact-vdisk.md)

- [詳細資料 vdisk](detail-vdisk.md)

- [卸離 vdisk](detach-vdisk.md)

- [展開 vdisk](expand-vdisk.md)

- [合併 vdisk](merge-vdisk.md)

- [選取 vdisk](select-vdisk.md)

- [名單](list_1.md)
