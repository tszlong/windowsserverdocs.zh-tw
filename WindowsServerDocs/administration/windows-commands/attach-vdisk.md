---
title: 將連結 vdisk
description: 適用於 Windows 命令主題**連結 vdisk** -附加 （有時稱為的掛接或介面） 的虛擬硬碟 (VHD)，使其出現在主機電腦，為本機硬碟上。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e715b36f8d9d8b416311567c2455920243dfc7a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885609"
---
# <a name="attach-vdisk"></a>將連結 vdisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

附加 （有時稱為的掛接或介面） 的虛擬硬碟 (VHD)，使其出現在主機電腦，為本機硬碟上。 如果 VHD 已經有磁碟分割區和檔案系統磁碟區，當您在附加時，VHD 內的磁碟區指派磁碟機代號。
> [!NOTE]
> 此命令僅適用於 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|唯讀|附加為唯讀的 VHD。 任何寫入作業會傳回錯誤。|
|sd=<SDDL string>|在 VHD 上設定使用者篩選器。 篩選條件字串必須以 Security Descriptor Definition Language (SDDL) 格式。 根據預設使用者篩選器會允許存取，例如實體磁碟上。<br /><br />SDDL 字串可能很複雜，但其最簡單的形式，保護存取的安全性描述元稱為判別存取控制清單 (DACL)。 它的格式為：D:<dacl_flags><string_ace1><string_ace2>... <string_acen><br /><br />常見的 DACL 旗標如下：<br /><br />-   **A**允許存取<br />-   **D**拒絕存取<br /><br />一般權限如下：<br /><br />-   **GA**所有存取<br />-   **GR**讀取權限<br />-   **GW**寫入權限<br /><br />一般使用者帳戶是：<br /><br />-   **BA**內建的系統管理員<br />-   **AU**已驗證的使用者<br />-   **CO** Creator owner<br />-   **WD** -任何人<br /><br />範例：<br /><br />**D:P:(A;;GR;;AU**讀取權限提供給所有已驗證的使用者<br /><br />**D:P:(A;;GA;;WD**提供每個使用者的完整存取|
|usefilesd|指定在 VHD 上，應該使用.vhd 檔案上的安全性描述元。 如果**Usefilesd**未指定參數、 VHD 不會有明確的安全性描述元，除非指定**Sd**參數。|
|noerr|用於僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|
## <a name="remarks"></a>備註
-   必須選取 VHD，並將其卸離此作業才會成功。 使用**選取 vdisk**命令來選取 VHD，並將焦點移到它。
## <a name="BKMK_Examples"></a>範例
若要附加選取的 VHD，以唯讀模式，請輸入：
```
attach vdisk readonly
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [中斷連結 vdisk](detach-vdisk.md)
-   [展開 vdisk](expand-vdisk.md)
-   [合併 vdisk](merge-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
