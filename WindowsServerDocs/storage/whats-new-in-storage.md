---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: "Windows Server 中存放裝置的新功能"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Windows Server 2016 中儲存空間的新功能

>適用於︰Windows Server 2016

本主題說明 Windows Server 2016 上「儲存空間」的新功能和已變更的功能。

## <a name="s2d"></a>儲存空間直接存取  
「儲存空間直接存取」能讓您使用具本機磁碟的伺服器，建置高可用且可調整的儲存空間。 它簡化了軟體定義儲存體系統的部署和管理，也解除了先前使用共用磁碟的叢集儲存空間限制，而能夠使用新的磁碟裝置類型 (例如 SATA SSD 和 NVMe 磁碟裝置)。  

**這項變更有什麼附加價值？**  
「儲存空間直接存取」能讓服務提供者和企業使用具本機存放區的業界標準伺服器，建立高可用且可調整的軟體定義儲存空間。 使用具本機存放區的伺服器可降低複雜性、提升延展性，並能夠使用之前不支援的存放裝置 (例如 SATA 固態硬碟) 以降低快閃儲存體成本，或是使用 NVMe 固態硬碟以取得較佳的效能。  

「儲存空間直接存取」免除了共用 SAS 結構的需求，以針對部署及設定做出簡化。 它改為以網路做為存放結構，利用 SMB3 和 SMB 直接傳輸 (RDMA) 來取得高速、低延遲，並能有效運用 CPU 的儲存空間。 如需進行擴充，只需要新增更多伺服器即可增加儲存空間和 I/O 效能  
如需詳細資訊，請參閱 [Windows Server 2016 中的儲存空間直接存取](storage-spaces/storage-spaces-direct-overview.md)。  

**運作上有何差異？**  
這是 Windows Server 2016 中的新功能。  

## <a name="storage-replica"></a>儲存體複本  
儲存體複本可在伺服器或叢集之間啟用與存放裝置無關、區塊層級及同步的複寫來進行災害復原，以及在站台之間延伸容錯移轉叢集。 同步複寫能以當機時保持一致的磁碟區啟用對實體站台中資料的鏡像，來確保檔案系統層級零資料遺失。 非同步複寫允許都會範圍外的站台擴充功能，但有資料遺失的可能性。  

**這項變更有什麼附加價值？**  
儲存體複寫可讓您執行下列操作︰  

* 針對任務關鍵性工作負載的計畫與非計畫中斷，提供單一廠商災害復原解決方案。
* 使用經證明具可靠性、可調整性和效能的 SMB3 傳輸。
* 將 Windows 容錯移轉叢集延伸至都會距離。
* 針對儲存體和叢集端對端地使用 Microsoft 軟體，例如 Hyper-V、儲存體複本、儲存空間、叢集、向外延展檔案伺服器、SMB3、重複資料刪除，以及 ReFS/NTFS。
* 以下列方式協助降低成本和複雜性： 
    * 與硬體無關，不具有像 DAS 或 SAN 的特定儲存體組態需求。
    * 允許平價儲存體和網路技術。
    * 透過容錯移轉叢集管理員，針對個別的節點和叢集提供易於使用的圖形化管理。
    * 透過 Windows PowerShell，提供完整、大規模的指令碼選項。 
* 協助降低停機時間，並提升 Windows 本身的可靠性和生產力。  
* 提供支援能力、效能標準及診斷功能。  

如需詳細資訊，請參閱 [Windows Server 2016 中的儲存體複本](storage-replica/storage-replica-overview.md)。  

**運作上有何差異？**  
這是 Windows Server 2016 中的新功能。  

## <a name="storage-qos"></a>存放裝置服務品質  
您現在可以在 Windows Server 2016 中，使用存放裝置服務品質 (QoS) 集中監視端對端儲存體效能，以及使用 Hyper-V 和 CSV 叢集建立管理原則。  

**這項變更有什麼附加價值？**  
您現在可在 CSV 叢集上建立存放裝置 QoS 原則，並將它們指派給 Hyper-V 虛擬機器上的一或多個虛擬磁碟。 存放裝置效能會隨工作負載和存放裝置負載的變動自動進行調整以符合原則。  

* 每個原則都可以指定要套用到資料流集合 (例如虛擬硬碟、單一或群組的虛擬機器、服務，或租用戶) 的保留 (最小值) 和/或上限 (最大值)。  
* 透過 Windows PowerShell 或 WMI，您可以執行下列工作︰  
    * 在 CSV 叢集上建立原則。
    * 列舉 CSV 叢集上可用的原則。
    * 將原則指派至 Hyper-V 虛擬機器的虛擬硬碟。 
    * 監視原則內每個流程和狀態的效能。  
* 如果有多個虛擬硬碟共用同一個原則，則效能會均勻分散以符合原則之最小值和最大值設定內的要求。 因此，原則可用來管理一個虛擬硬碟、一部虛擬機器、組成服務的多部虛擬機器，或是由租用戶擁有的所有虛擬機器。  

**運作上有何差異？**  
這是 Windows Server 2016 中的新功能。 在舊版 Windows Server 中，無法管理最小保留值、透過單一命令來監視叢集中所有虛擬磁碟的流程，以及進行集中式原則管理。  

如需詳細資訊，請參閱[存放裝置服務品質](storage-qos/storage-qos-overview.md)

## <a name="dedup"></a>重複資料刪除  
| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| [支援大型磁碟區](data-deduplication/whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，使用者必須特別針對預期的變動設定磁碟區大小，而大於 10 TB 的磁碟區並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，重複資料刪除支援**最高 64 TB** 的磁碟區大小。 |
| [支援大型檔案](data-deduplication/whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的檔案並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，已完全支援**最高 1 TB** 大小的檔案。 |
| [支援 Nano 伺服器](data-deduplication/whats-new.md#nano-server-support) | 新增 | Windows Server 2016 的新 Nano 伺服器部署選項可以使用並且完全支援重複資料刪除。 |
| [簡化的備份支援](data-deduplication/whats-new.md#simple-backup-support) | 新增 | 在 Windows Server 2012 R2 中，虛擬備份應用程式 (例如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx)) 必須透過一系列的手動設定步驟來取得支援。 在 Windows Server 2016 中，已加入新的預設使用類型 "Backup"，以無接縫地針對虛擬備份應用程式部署重複資料刪除。 |
| [支援叢集 OS 輪流升級](data-deduplication/whats-new.md#cluster-upgrade-support) | 新增 | 重複資料刪除完整支援 Windows Server 2016 新的[叢集 OS 輪流升級](..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能。 |

## <a name="smb-hardening-improvements"></a>針對 SYSVOL 和 NETLOGON 連線的 SMB 強化功能改進  
在 Windows 10 和 Windows Server 2016 中，用戶端針對網域控制站上 Active Directory 網域服務之預設 SYSVOL 和 NETLOGON 共用的連線，現在需要 SMB 簽署及相互驗證 (例如 Kerberos)。   

**這項變更有什麼附加價值？**  
這項變更降低了攔截式攻擊的可能性。   

**運作上有何差異？**  
如果無法使用 SMB 簽署和相互驗證，Windows 10 或 Windows Server 2016 電腦將不會處理網域型的群組原則和指令碼。  

> [!NOTE]  
> 這些設定的登錄值預設將不會顯示，但仍會套用強化原則，直到由群組原則或其他登錄值覆寫為止。  

如需這些安全性改進 (也稱為 UNC 強化) 的詳細資訊，請參閱 Microsoft 知識庫文章 [3000483](http://support.microsoft.com/kb/3000483) 和 [MS15-011 與 MS15-014：強化群組原則](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy) (英文)。  

## <a name="work-folders"></a>工作資料夾
工作資料夾伺服器執行的是 Windows Server 2016 而工作資料夾用戶端為 Windows 10 時的改良變更通知。

**這項變更有什麼附加價值？**<br>
在 Windows Server 2012 R2 中，將檔案變更同步處理到工作資料夾伺服器時，用戶端不會收到變更的通知，而且最多等待 10 分鐘就會收到更新。  使用 Windows Sever 2016 時，工作資料夾伺服器會立即通知 Windows 10 用戶端，並立即同步處理檔案變更。

**運作上有何差異？**<br>
這是 Windows Server 2016 中的新功能。 這需要 Windows Server 2016 工作資料夾伺服器，而且用戶端必須是 Windows 10。

如果您使用的是舊版用戶端，或工作資料夾伺服器是 Windows Server 2012 R2，則用戶端會每隔 10 分鐘繼續輪詢變更。

## <a name="refs"></a>ReFS 
ReFS 的下一個反覆運算支援大規模儲存部署並具備多種工作負載，為您的資料提供可靠性、復原及延展性。     

**這項變更有什麼附加價值？**<br>
ReFS 引入下列改善：

* ReFS 實作新儲存層功能，協助提供更快的效能並提升儲存容量。 此項新功能可以達成：
    * 相同虛擬磁碟上的多種復原類型 (例如，在效能層使用鏡像並在容量層使用同位)
    * 對漂移工作集的回應性提升。 
    * 支援 SMR (疊瓦式磁記錄) 媒體。 
* 區塊複製的引入大幅改善了 VM 作業的效能，例如 .vhdx 檢查點合併作業。
* 新的 ReFS 掃描工具可讓流失的儲存體復原，並協助回收嚴重損毀的資料。 

**運作上有何差異？**<br>
這些為 Windows Server 2016 的新功能。 

## <a name="see-also"></a>另請參閱  
* [Windows Server 2016 的新功能](../get-started/what-s-new-in-windows-server-2016.md)  
