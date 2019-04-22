---
title: 相容的硬體與 Windows Server 虛擬化保護的程式碼完整性
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820369"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>相容的硬體與 Windows Server 虛擬化保護的程式碼完整性

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

Windows Server 2016 引進了新的虛擬化架構的程式碼保護，以協助保護實體和虛擬機器從修改系統程式碼的攻擊。 若要達到此最高保護層級，Microsoft 會與電腦硬體製造商 （原始設備製造商或 Oem），以防止惡意的寫入串聯系統執行程式碼中。 這項保護可以套用至任何系統，也要做為建置組塊的其中一個可用以執行受防護的虛擬機器 (Vm) 的 HYPER-V 主機健全狀況。 

如同任何硬體式的保護，有些系統可能不相容，因為問題，例如不正確標記的記憶體分頁的可執行檔或真的想要修改程式碼在執行階段，可能會導致非預期的失敗，包括資料遺失或藍色的嘗試（也稱為 「 停止 」 錯誤） 的畫面時發生錯誤。 

相容，並完全支援新的安全性功能，Oem 必須實作 UEFI 2.6，已在 2016 年 1 月發行中所定義的記憶體位址表格。 採用標準的新 UEFI 需要的時間;同時，若要避免發生問題的客戶，我們想要提供資訊系統和已完成測試設定與這項功能的組態，以及我們知道與不相容的系統。 

## <a name="non-compatible-systems"></a>非相容的系統

下列組態已知不相容的虛擬化型保護與程式碼完整性也不能做為主機受防護的 vm:

- Dell PowerEdge 伺服器執行 PERC H330 RAID 控制器的詳細資訊，請參閱下列文章，從 Dell 支援[H330 – 啟用 「 主機守護者 HYPER-V 支援 」 或 「 Device Guard"Win 2016 作業系統上會導致 OS 開機失敗](http://www.dell.com/Support/Article/us/en/19/QNA44045)。  


## <a name="compatible-systems"></a>相容的系統

這些是我們和合作夥伴已經測試過我們的環境中的系統。 請先確定您已驗證的系統如預期般在您的環境中的運作方式： 

- 虛擬機器 – 您可以讓程式碼完整性，開頭為 Windows Server 2016 的 HYPER-V 主機執行的虛擬機器上的虛擬化型保護。



