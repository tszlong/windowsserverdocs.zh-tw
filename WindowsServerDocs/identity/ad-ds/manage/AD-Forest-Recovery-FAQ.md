---
description: 深入瞭解： AD 樹系復原-常見問題
title: AD 樹系復原 - 常見問題
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.openlocfilehash: 52ade074287eaf41814099ba31f6b894c5f2437a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045606"
---
# <a name="ad-forest-recovery---faq"></a>AD 樹系復原 - 常見問題

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2、Windows Server 2003

本檔包含有關樹系復原 (常見問題) 的常見問題：

## <a name="general-recovery"></a>一般復原

**問：我可以怎麼做才能加快復原的速度？**

雖然復原的速度不是本指南的主要目標，但您可以透過下列方式來達到較短的復原時間：

- 建立詳細的樹系復原方案、定期更新，並在合理大小的模擬測試環境中進行測試，至少一年一次
- 使用虛擬網域控制站 (DC) 複製
   - 虛擬化 DC 複製可加速在每個網域中的備份還原一個 DC 之後，取得其他 Dc 的程式。 您可以複製額外的虛擬化 Dc，而不是等候可能冗長的 AD DS 安裝完成，並在安裝後完成非關鍵性的複寫。
   - 裝載虛擬 Dc 的樹系，在復原期間可能會受益于較少的連線資料中心。 不過，在相同網域的多個虛擬化 Dc 都位於相同的虛擬機器主機上的任何環境都應該受益。
-  (Rodc 部署唯讀網域控制站) 
   - Rodc 可以在復原程式期間供應商務持續性，因為它們不需要像可寫入的 Dc 一樣從網路中斷連線。 Rodc 不會執行輸出複寫。 因此，它們不會呈現可寫入 Dc 將損毀的資料複寫回復原的環境中所造成的相同風險。

影響樹系復原程式持續時間的其他因素包括下列各項：

- 當您從備份還原 Dc 時，需要一些時間才能：
   - 找出實體備份媒體，例如磁帶。
   - 請重新安裝作業系統。
   - 從備份媒體還原資料。
      - 您可以執行完整伺服器復原（而非系統狀態還原），以縮短重新安裝作業系統和從備份還原資料所需的時間。 由於完整伺服器復原是以二進位為基礎，因此它的完成速度會比系統狀態還原快很多。
      - 但是，如果伺服器包含的資料已從您不想要還原的系統狀態資料中排除，則完整伺服器復原可能不是系統狀態還原的可行替代方案。 請考慮執行完整伺服器復原的優點，而不是明確地執行伺服器的系統狀態還原，並藉由執行您打算稍後還原的適當備份類型來做好準備。
- 當您重建 Dc 時，複寫資料以進行網路型升級需要一些時間。
   - 您可以執行下列步驟來減少還原 Dc 所需的時間：
- 減少取得備份媒體的時間：
   - 使用 Active Directory 資料庫掛接工具 ( # A0) 來識別要用於還原作業的最佳備份。 如需有關使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱 [Active Directory 資料庫掛接工具的逐步指南](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577) 。
   - 以組織的方式清楚標示備份媒體，並以組織的方式儲存媒體，以提供快速的抓取。
   - 使用磁碟區陰影複製服務與存放區域網路 (SAN) 維護不同時間點的備份。 如需詳細資訊，請參閱 [Windows Server 2003 Active Directory 使用磁碟區陰影複製服務和虛擬磁碟服務的快速](https://go.microsoft.com/fwlink/?LinkId=70781) 復原 (https://go.microsoft.com/fwlink/?LinkId=70781) 。
- 強制移除 Dc 中的 AD DS，而不是重新安裝作業系統。 如果將整個樹系失敗的原因識別為純粹在 AD DS 的範圍內，您就不需要重新安裝 Dc 上的作業系統。
   - 如需從執行 Windows Server 2008 或更新版本的 DC 強制移除 AD DS 的詳細資訊，請參閱 [強制移除 Windows server 2008 網域控制站](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627) 。 如需從執行 Windows Server 2003 的 DC 強制移除 AD DS 的詳細資訊，請參閱 Microsoft 知識庫 (中的 [文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780) https://go.microsoft.com/fwlink/?LinkId=70780) 。
- 使用更快速的磁帶裝置或磁片備份，減少還原作業所需的時間。

您也可以使用從媒體安裝 (IFM) 功能來重建每個網域中的 Dc，以協助加速 AD DS 安裝。 IFM 可減少當您在每個網域中重建 Dc 時所產生的複寫延遲。

具有更積極服務等級協定 (SLA) 的企業可能會考慮改變樹系復原程式以加快復原速度。

**問：我是否可以將樹系復原程式自動化？**

由於樹系復原程式的複雜和重要本質，目前沒有端對端自動化。 樹系復原程式是比程式自動化的技術問題還原商務持續性更高的物流和組織挑戰。 因此，管理環境的人員應該建立該環境專屬的樹系復原方案，然後將可成功自動化的部分自動化。

您可以使用命令列工具執行大部分的樹系復原步驟。 因此，大部分的步驟都可以編寫腳本。 例如，Ntdsutil.exe 是樹系復原程式中最常使用的工具之一。

雖然腳本可以加快復原速度，但您必須先徹底測試這些腳本，然後再將它們套用到實際的環境。 此外，您必須根據 Active Directory 環境中的變更（例如新增網域或 DC）或新版本的 Active Directory 來更新它們。

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
