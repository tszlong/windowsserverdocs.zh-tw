---
description: 深入瞭解：使用 Windows Server 虛擬化型的程式碼完整性保護的相容硬體
title: 以 Windows Server 虛擬化為基礎的程式碼完整性保護的相容硬體
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 3437aadb68af98ac97e072b97a979c4f7a885869
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047456"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>以 Windows Server 虛擬化為基礎的程式碼完整性保護的相容硬體

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

Windows Server 2016 引進了新的虛擬化型程式碼保護，可協助保護實體和虛擬機器免于修改系統程式碼的攻擊。
為了達到此高保護層級，Microsoft 會與電腦硬體製造商一同 (原始設備製造商或 Oem) ，以防止惡意寫入系統執行程式碼。
這項保護可以套用至任何系統，並作為用來為受防護虛擬機器 (Vm) 執行 Hyper-v 主機健全狀況的其中一個組建區塊。

如同任何以硬體為基礎的保護，某些系統可能不符合規範，因為將記憶體頁面標示為可執行檔，或實際嘗試在執行時間修改程式碼，這可能會導致非預期的失敗，包括資料遺失或藍色螢幕錯誤 (也稱為停止錯誤) 。

為了相容並完全支援新的安全性功能，Oem 需要執行在 2.6 2016 年1月發行的 UEFI 中所定義的記憶體位址表。
採用新的 UEFI 標準需要時間;同時，為了避免客戶遇到問題，我們想要提供有關系統和設定的資訊，這些是我們已測試過這個功能集的系統和設定，以及我們已知不相容的系統。

## <a name="non-compatible-systems"></a>不相容系統

已知下列設定與以虛擬化為基礎的程式碼完整性保護是不相容的，且無法作為受防護 Vm 的主機使用：

- 執行 PERC H330 RAID 控制器的 dell PowerEdge 伺服器如需詳細資訊，請參閱下列來自 Dell 支援 H330 的文章： [在 Win 2016 作業系統上啟用「主機守護者 Hyper-v 支援」或「Device Guard」會導致 os 開機失敗](http://www.dell.com/Support/Article/us/en/19/QNA44045)。


## <a name="compatible-systems"></a>相容系統

這些是我們和我們的合作夥伴在環境中測試的系統。
請確定您確認系統在您的環境中如預期般運作：

- 虛擬機器–您可以在從 Windows Server 2016 開始的 Hyper-v 主機上執行的虛擬機器上，啟用以虛擬化為基礎的程式碼完整性保護。



