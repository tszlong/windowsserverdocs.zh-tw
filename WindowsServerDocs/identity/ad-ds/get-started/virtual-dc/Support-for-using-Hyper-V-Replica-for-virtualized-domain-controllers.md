---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: "使用 HYPER-V 複本模擬的網域控制站的支援"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>使用 HYPER-V 複本模擬的網域控制站的支援

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題解釋使用 HYPER-V 複本複寫一樣 (VM) 執行為網域控制站 (DC) 的支援。 HYPER-V 複本是 HYPER-V 開頭提供建複寫機制，在 VM 層級的 Windows Server 2012 」 的新功能。  
  
HYPER-V 複本非同步會複寫選取的 Vm 複本 HYPER-V 主機主要 HYPER-V 主機的區域網路或 WAN 連結。 完成初始複寫之後，系統管理員所定義的間隔複寫後續變更。  
  
容錯移轉可能未計畫或計劃。 計畫容錯移轉車載機起始主要 VM 中，系統管理員，並將取消複製的任何變更透過複製到複本 VM 為防止任何資料遺失。 回應未預期的錯誤的主要 VM 中的複本 VM 起始意外的容錯移轉。 可能會遺失資料因為有機會不會傳送上可能不已複寫尚未主要 VM 變更。  
  
如需 HYPER-V 複本，請查看[HYPER-V 複本概觀](https://technet.microsoft.com/library/jj134172.aspx)和[部署 HYPER-V 複本](https://technet.microsoft.com/library/jj134207.aspx)。  
  
> [!NOTE]  
> 只有在 Windows Server HYPER-V，不是執行 Windows 8 上的 HYPER-V 版本可執行 HYPER-V 複本。  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Windows Server 2012 所需的網域控制站  
Windows Server 2012 HYPER-V 也引進了 VM GenerationID (VMGenID)。 VMGenID 提供發生重大變更，以客體 OS 通訊 hypervisor 的方式。 例如，hypervisor 可以通訊到模擬 DC 的快照還原發生 （HYPER-V 快照還原技術，不備份還原）。 Windows Server 2012 中的 AD DS 目前正在設法 VMGenID VM 技術的並使用它來偵測執行 hypervisor 作業，例如快照還原，讓它變得更好保護您。  
  
> [!NOTE]  
> 若要加速點，只有在 Windows Server 2012 網域控制站 AD DS 提供導致 VMGenID; 從這些安全機制Dc 執行的 Windows Server 所有先前的版本會受到例如 USN 復原模擬的 DC 還原使用尚未支援的機制，例如快照還原時，可能發生的問題。 如需有關這些安全性，而且在觸發時，請查看[擬化檔案網域控制站架構](https://technet.microsoft.com/library/jj574118.aspx)。  
  
時容錯移轉 HYPER-V 複本 （計劃或計畫），Windows Server 2012 擬化檔案俠偵測 VMGenID 重設觸發上述安全性功能。 Active Directory 作業再繼續以一般。 複本 VM 執行主要 VM 的位置。  
  
> [!NOTE]  
> 現在有兩個相同俠身分執行個體，還有可能執行主要執行個體與複製執行個體。 HYPER-V 複本以確保主要有控制機制，複本 Vm 同時執行時它可能會在之後的 VM 複寫失敗的之間的連結的事件執行一次。 發生這個耗，模擬執行 Windows Server 2012 」 的網域控制站有防護功能，可協助保護 AD DS，請執行舊版 Windows Server 的網域控制站擬化檔案，而不要。  
  
使用 HYPER-V 複本，確保您依照最佳做法[上 HYPER-V 執行 virtual 網域控制站](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx)。 此討論，例如儲存 virtual SCSI 磁碟上的 Active Directory 檔案的建議提供較保證資料持續性。  
  
## <a name="supported-and-unsupported-scenarios"></a>支援和支援案例  
執行 Windows Server 2012 Vm 支援意外的錯誤後移轉和測試容錯移轉。 即使的計劃容錯移轉，Windows Server 2012 建議模擬俠為了的系統管理員的身分不慎開始主要 VM 和複寫的 VM 同時降低風險。  
  
Vm 執行舊版 Windows Server 的計劃容錯移轉支援，但因為 USN 復原可能不支援意外的錯誤後移轉的。 如需 USN 復原，請查看[USN 和 USN 復原](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。  
  
> [!NOTE]  
> 有網域或森林; 不功能層級需求有只作業系統執行使用 HYPER-V 複本複寫 Vm 為網域控制站的需求。 Vm 可以包含的其他實體或 virtual Dc 執行舊版 Windows Server 和可能或可能不也會複寫使用 HYPER-V 複本森林中部署。  
  
這個支援聲明為基礎測試所執行的單一網域-森林，但也支援多網域森林設定。 針對這些測試，DC1 和 DC2 模擬的網域控制站的 Active Directory 複寫合作夥伴相同的網站、 Windows Server 2012 上執行於 HYPER-V 的伺服器上。 執行 DC2 VM 來賓已支援 HYPER-V 複本。 在另一部遠方地理位置資料中心裝載複本伺服器。 若要協助解釋如下的測試案例處理程序，VM 複本伺服器上執行稱為 DC2-收到及 （但實際上它就會保留原始 VM 相同的名稱）。  
  
### <a name="windows-server-2012"></a>Windows Server 2012  
下表解釋模擬執行 Windows Server 2012 和測試案例網域控制站的支援。  
  
|||  
|-|-|  
|規劃錯誤後的移轉|計畫錯誤後的移轉|  
|支援|支援|  
|測試案例︰<br /><br />Lax-dc1 和 DC2 執行 Windows Server 2012。<br /><br />-DC2 已關機並容錯移轉 DC2-收到及上執行。錯誤後的移轉可以計畫或計劃。<br /><br />-DC2-收到及開始之後，它會檢查是否在其資料庫 VMGenID 的值為值相同伺服器 HYPER-V 複本儲存一樣驅動程式。<br /><br />-如此一來，DC2-收到及觸發模擬保護措施;亦即它重設其呼叫識別碼、 其 RID 集區中，會捨棄並它將會假設作業主角之前，將設定初始同步需求。 如需初始同步需求，查看。<br /><br />-DC2 收到及資料庫中儲存的新 VMGenID 值，然後確認任何後續更新的新呼叫識別碼部分。<br /><br />-做為因的呼叫識別碼重設 DC1 將會減少的所有 AD 變更導入了 DC2-收到及，即使這復原的時間，這表示任何廣告更新之後會安全地涵蓋容錯移轉 DC2-收到及執行|測試案例是一樣計劃故障，使用這些例外：<br /><br />-任何廣告更新上收到的 DC2，但尚未複寫的廣告合作夥伴複製到之前容錯移轉事件將會遺失。<br /><br />從 DC1 回 DC2-收到及複寫 AD 以 DC1 複寫復原點的時間後 DC2 上收到的廣告更新。|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 和較舊版本  
下表解釋模擬執行 Windows Server 2008 R2 和舊版的網域控制站的支援。  
  
|||  
|-|-|  
|規劃錯誤後的移轉|計畫錯誤後的移轉|  
|支援，但不是建議您因為不支援 VMGenID Dc 執行這些版本的 Windows Server 或使用相關的模擬防護功能。 這會將它們 USN 復原的風險。 如需詳細資訊，請查看[USN 和 USN 復原](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10))。|不支援**請注意：**會 USN 復原已無法使用風險，例如單一 DC 森林 （不建議設定） 中支援意外的錯誤後移轉。|  
|測試案例︰<br /><br />Lax-dc1 和 DC2 執行 Windows Server 2008 R2。<br /><br />-DC2 已關機並計劃容錯移轉 DC2-收到及上執行。關機在完成之前 DC2-收到及到複製 DC2 上的所有資料。<br /><br />-DC2-收到及開始之後，它會繼續複寫 DC1 使用相同的呼叫識別碼 DC2。|不適用|  
  


