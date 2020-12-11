---
description: 深入瞭解： Active Directory 樹系復原虛擬化
title: AD 樹系修復虛擬化
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.openlocfilehash: bf84f5db627d2383a47a9d23ce72acbc45251ea6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049546"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 樹系復原虛擬化

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本主題說明 Windows Server 2016、2012 R2 和2012中的虛擬網域控制站複製功能。

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用虛擬網域控制站複製以加速樹系復原

虛擬網域控制站 (DC) 複製可簡化並加速在網域中安裝其他虛擬化 Dc 的程式，特別是在集中的位置，例如在虛擬程式上執行多個 Dc 的資料中心。 當您從備份還原每個網域中的一個虛擬 DC 之後，每個網域中的其他 Dc 都可以使用虛擬化 DC 複製程式快速地上線。 您可以準備您復原的第一個虛擬化 DC、將其關閉，然後視需要複製該虛擬硬碟，以建立已複製的虛擬化 Dc 來建立網域。

虛擬化 DC 複製的需求如下：

- 虛擬程式必須支援 VM >generationid。 Windows Server 2016、2012和 Windows 8 中的 hyper-v 是支援 VM >generationid 的虛擬程式範例。 如果支援 VM-GenerationID，請洽詢您的虛擬程式廠商。
- 用來作為複製來源的虛擬化 DC 必須執行 Windows Server 2016 或2012，並且是 Cloneable 網域控制站群組的成員。
- PDC 模擬器必須執行 Windows Server 2016 或2012。 如果 PDC 模擬器已虛擬化，您可以加以複製。

如需有關如何執行虛擬化 DC 複製的逐步指示，請參閱 [Active Directory Domain Services (AD DS) 虛擬化 (層級 100) 的簡介 ](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 如需虛擬化 DC 複製運作方式的詳細資訊，請參閱 [虛擬網域控制站技術參考 (層級 300) ](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
