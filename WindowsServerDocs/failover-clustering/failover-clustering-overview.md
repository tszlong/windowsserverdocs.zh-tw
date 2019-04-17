---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: 容錯移轉叢集
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: b0fece76b871da3fa9d6a996798a5008756f486b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "9178599"
---
# Windows Server 中的容錯移轉叢集

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<hr />

容錯移轉叢集是由獨立電腦組成的群組，它們共同運作以提升叢集角色 (之前稱為叢集應用程式和服務) 的可用性及擴充性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一或多個叢集節點失敗，其他節點即會透過稱為「容錯移轉」的程序開始提供服務。 不僅如此，叢集角色還會主動監看以確認服務正常運作。 如果服務沒有運作，就會重新啟動或移到另一個節點。

容錯移轉叢集也提供叢集共用磁碟區 (CSV) 功能，此功能提供一致的分散式命名空間，叢集角色可使用這個空間從所有節點存取共用存放空間。 透過容錯移轉叢集功能，使用者幾乎不會感覺到服務中斷的情況。

容錯叢集有許多實用的應用程式，包括：
* Microsoft SQL Server 和 Hyper-V 虛擬機器之類應用程式的高可用性或持續可用的檔案共用儲存區
* 在實體伺服器或安裝在執行 Hyper-V 伺服器的虛擬機器上執行的高可用性叢集角色

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">容錯移轉叢集的新功能</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>了解</h3>
<HR />
                                        <p><a href="sofs-overview.md">用於應用程式資料的向外延展檔案伺服器</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">叢集和集區仲裁</a></p>
<HR />
                                        <p><a href="fault-domains.md">容錯網域感知</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">簡化 SMB 多重通道和多個 NIC 的叢集網路</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">VM 負載平衡</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">叢集集合</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">叢集親和性</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>規劃</h3>
<HR />
                                        <p><a href="clustering-requirements.md">容錯移轉叢集硬體需求及儲存體選項</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">使用叢集共用磁碟區 (CSV)</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">使用客體虛擬機器叢集搭配儲存空間直接存取</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>部署</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">在 Active Directory Domain Services 中預先設置叢集電腦物件</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">建立容錯移轉叢集</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">部署雙節點檔案伺服器</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">管理仲裁和見證</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">部署雲端見證</a></p>
<HR />
                                        <p><a href="file-share-witness.md">部署檔案共用見證</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">叢集作業系統輪流升級</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">升級相同硬體上的容錯移轉叢集</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">部署已中斷連結 Active Directory 的叢集</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>管理</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">叢集感知更新</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">健康情況服務</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">叢集網域移轉</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">使用 Windows 錯誤報告進行疑難排解</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>工具及設定</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">容錯移轉叢集 PowerShell Cmdlet</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">叢集感知更新 PowerShell Cmdlet</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>社群資源</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">高可用性 (叢集) 論壇</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">容錯移轉叢集和網路負載平衡小組部落格</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
