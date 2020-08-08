---
title: 相容硬體與 Windows Server 虛擬化型的程式碼完整性保護
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 616718b2b2a5ce3c30655d3fef37da5ee2ea25e5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971355"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>相容硬體與 Windows Server 虛擬化型的程式碼完整性保護

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

Windows Server 2016 引進了新的虛擬化型程式碼保護，以協助保護實體和虛擬機器，免于遭受修改系統程式碼的攻擊。
為了達到此高保護層級，Microsoft 會與電腦硬體製造 (原始設備製造商，或 Oem) 以防止惡意寫入系統執行程式碼。
這項保護可以套用至任何系統，並作為執行受防護虛擬機器之 Hyper-v 主機健全狀況的其中一個構成要素， (Vm) 。

如同任何以硬體為基礎的保護，部分系統可能會因為問題（例如，將記憶體分頁標示為可執行檔，或實際嘗試在執行時間修改程式碼）而不符合規範，這可能會導致非預期的失敗，包括資料遺失或藍色螢幕錯誤 (也稱為停止錯誤) 。

為了與新的安全性功能相容並完全支援，Oem 必須執行在 Jan 2.6 中所定義的記憶體位址資料表，此作業是在 Jan 2016 中發行。
採用新的 UEFI 標準需要一些時間;同時，為了避免客戶遇到問題，我們想要提供已測試過這項功能集的系統和設定相關資訊，以及我們知道不相容的系統。

## <a name="non-compatible-systems"></a>不相容的系統

已知下列設定與以虛擬化為基礎的程式碼完整性保護不相容，且無法做為受防護 Vm 的主機使用：

- 執行 PERC H330 RAID 控制器的 dell PowerEdge 伺服器如需詳細資訊，請參閱《 Dell 支援 H330 》中的下列文章：在[Win 2016 OS 上啟用「主機守護者 Hyper-v 支援」或「Device Guard」會導致 OS 開機失敗](http://www.dell.com/Support/Article/us/en/19/QNA44045)。


## <a name="compatible-systems"></a>相容的系統

這些是我們和我們的合作夥伴在我們的環境中進行測試的系統。
請確定您在您的環境中確認系統如預期般運作：

- 虛擬機器–您可以在從 Windows Server 2016 開始的 Hyper-v 主機上執行的虛擬機器上，啟用程式碼完整性的虛擬化保護。



