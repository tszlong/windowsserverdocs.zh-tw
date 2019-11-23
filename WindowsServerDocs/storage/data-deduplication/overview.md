---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: 重複資料刪除概觀
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 1050c63d77db66c8e280ea1bea9503390c5d0bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386296"
---
# <a name="data-deduplication-overview"></a>重複資料刪除概觀

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、 

## <a name="what-is-dedup"></a>什麼是重復資料刪除？

重復資料刪除（通常稱為「重復資料刪除」）是一項功能，可協助降低重復資料對儲存體成本的影響。 啟用時，重複資料刪除會藉由尋找磁碟區上的重複部分來檢查磁碟區上的資料，以最佳化磁碟區上的可用空間。 磁碟區資料集的重複部分只會儲存一次並 (選擇性) 進行壓縮，進一步節省空間。 重複資料刪除可將備援最佳化，而不必犧牲資料精確度或完整性。 如需重複資料刪除運作方式的詳細資訊，請參閱[重複資料刪除如何運作？](understand.md#how-does-dedup-work)。 一節，其在[了解重複資料刪除](understand.md)頁面上。

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334)包含重復資料刪除的修正的匯總套件，包括重要的可靠性修正程式，我們強烈建議您在搭配 windows server 2016 和 windows server 2019 使用重復資料刪除時安裝它。

## <a name="why-is-dedup-useful"></a>為何重復資料刪除很有用？

重複資料刪除可協助存放裝置系統管理員降低與重複資料相關聯的成本。 大型資料集通常會有「許多」 **<u></u>** 重複資料，使得儲存資料的成本增加。 例如：

- 使用者的檔案共用可能有許多相同或類似的檔案複本。
- 虛擬機器與虛擬機器之間的虛擬化客體可能幾乎完全相同。
- 每天的備份快照之間可能只有細微的差異。

重複資料刪除可獲得的空間節省效果，取決於資料集或磁碟區上的工作負載。 將資料重複性高的資料集最佳化比率可能會高達 95%，或讓存放裝置使用率降低 20 倍。 下表強調說明各種內容類型一般會有的重複資料刪除節省量：

| 案例       | 內容                                        | 一般的節省空間 |
|----------------|------------------------------------------------|-----------------------|
| 使用者文件 | Office 文件、相片、音樂、影片等  | 30-50%                |
| 部署共用 | 軟體二進位檔、cab 檔案、符號等 | 70-80%                |
| 虛擬程式庫 | ISO、虛擬硬碟檔案等  | 80-95%                |
| 一般檔案共用 | 上述所有項目                           | 50-60%                |

## <a id="when-can-dedup-be-used"></a>何時可以使用重復資料刪除？  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>一般用途的檔案伺服器</b><br />
一般用途的檔案伺服器是用途一般的檔案伺服器，可能會包含下列任一種共用類型︰ <ul>
                    <li>小組共用</li>
                    <li>使用者主資料夾</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">工作資料夾</a></li>
                    <li>軟體開發共用</li>
                </ul>
一般用途的檔案伺服器是適合進行重複資料刪除的候選項目，原因是多位使用者可能就會有相同檔案的許多複本或版本。 軟體開發共用會從重複資料刪除獲益，原因是在不同組建之間，基本上有許多二進位檔都會維持不變。 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">
                <b>虛擬桌面基礎結構（VDI）部署</b><br />
VDI 伺服器 (例如<a href="https://technet.microsoft.com/library/cc725560.aspx">遠端桌面服務</a>) 可為組織提供輕量選項，將桌面佈建給使用者。 組織依賴這類技術的原因有許多︰ <ul>
                    <li><b>應用程式部署</b>︰可以在您的企業快速部署應用程式。 當您的應用程式會頻繁更新、不常使用或難以管理時，這會特別有用。</li>
                    <li><b>應用程式彙總</b>︰從一組集中管理的虛擬機器安裝和執行應用程式時，不需要更新用戶端電腦上的應用程式。 這個選項也會降低存取應用程式所需的網路頻寬量。</li>
                    <li><b>遠端存取</b>︰使用者可以從像是家用電腦、Kiosk、低階硬體以及 Windows 以外的作業系統等裝置，來存取企業應用程式。</li>
                    <li><b>分公司存取</b>：VDI 部署可為需要存取集中式資料存放區的分公司員工，提供較佳的應用程式效能。 有時資料密集應用程式沒有最適合緩速連線使用的用戶端/伺服器通訊協定。</li>
                </ul>
VDI 部署是最佳的重複資料刪除候選項目，原因是為使用者驅動遠端桌面的虛擬硬碟基本上完全相同。 此外，重複資料刪除有助於處理所謂的「VDI 開機壅塞情況」，也就是許多使用者一早同時登入其桌面時，造成存放裝置效能驟降的情況。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>備份目標，例如虛擬備份應用程式</b><br />
像是 <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a> 的備份應用程式是重複資料刪除功能的絕佳候選項目，原因是備份快照之間會大量重複。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>其他工作負載</b><br />
                <a href="install-enable.md#enable-dedup-candidate-workloads" data-raw-source="[Other workloads may also be excellent candidates for Data Deduplication](install-enable.md#enable-dedup-candidate-workloads)">其他工作負載也可能是重複資料刪除功能的絕佳候選項目</a>。
            </td>
        </tr>
    </tbody>
</table>
