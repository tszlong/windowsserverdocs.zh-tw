---
title: 叢集親和性
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: 這篇文章說明容錯移轉叢集親和性和 antiAffinity 層級
ms.openlocfilehash: 67929e6d3399633ebfec0b908463131973aecaf7
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453034"
---
# <a name="cluster-affinity"></a>叢集親和性

> 適用於：Windows Server 2019，Windows Server 2016

容錯移轉叢集可以保存多個角色可以節點之間移動，並執行。  有些的時候，當特定角色 （也就是虛擬機器、 資源群組等等） 不應該執行相同的節點。  這可能是因為資源耗用量、 記憶體使用量等。比方說，有兩部虛擬機器的記憶體和 CPU 密集和兩部虛擬機器相同的節點上執行，如果一或兩個虛擬機器可能會有影響的效能問題。  這篇文章將說明叢集 antiaffinity 層級，以及您如何使用它們。

## <a name="what-is-affinity-and-antiaffinity"></a>親和性和 AntiAffinity 為何？

親和性會建立 （i、 e、 虛擬機器、 資源群組等等） 要保持在一起的兩個或多個角色之間的關聯性的規則設定。  AntiAffinity 相同，但會用來嘗試和保持彼此分開指定的角色。  容錯移轉叢集使用 AntiAffinity 及其角色。  更具體來說， [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames)參數定義的角色，因此它們不在相同節點上執行。  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

檢視群組的內容時，沒有 AntiAffinityClassNames 的參數，而且它是預設值為空白。  下列範例中，Group1 和 Group2 應以相同的節點上執行。  若要檢視屬性，此 PowerShell 命令，結果會是：

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

因為 AntiAffinityClassNames 未定義為預設值，這些角色可以執行在一起或分開。  目標是將它們分隔。  AntiAffinityClassNames 的值可以是您想要他們就不必相同。  假設 Group1 和 Group2 是在虛擬機器中執行的網域控制站，而且它們會最能滿足不同節點上執行。  因為這些都是網域控制站，我會使用 DC 的類別名稱。  若要設定的值，PowerShell 命令，而且結果會是：

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

既然已設定，容錯移轉叢集會嘗試將它們分開。  

AntiAffinityClassName 參數是 「 軟式 」 區塊。  這表示，它就會嘗試讓它們保持分開的但如果不行，它仍然會允許他們在相同節點上執行。  例如，群組執行的雙節點容錯移轉叢集。  如果一個節點需要停機進行維護，它表示這兩個群組會在相同節點上會啟動並執行。  在此情況下，它就可以擁有此資料庫。  它可能不是最理想的但這兩個 virtial 機器可讓您將仍在可接受的效能範圍內執行。

## <a name="i-need-more"></a>我需要更多

如前所述，AntiAffinityClassNames 是軟區塊。  但如果需要硬區塊？  無法執行虛擬機器上相同的節點;否則，效能影響將會發生，導致某些服務可能中斷。

針對這些情況下，沒有 ClusterEnforcedAntiAffinity 的其他叢集屬性。  此 antiaffinity 的層級會防止在所有成本相同 AntiAffinityClassNames 值在相同節點上執行。

若要檢視的屬性和值，會是 PowerShell 命令 （和結果）：

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

值為"0"，表示已停用，而不會強制執行。  值為"1"可讓它並且硬區塊。  若要啟用此硬碟的區塊，命令 （和結果） 是：

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

當這兩種設定時，群組將無法上線在一起。  如果它們位於相同的節點時，這是您會看到在容錯移轉叢集管理員。

![叢集親和性](media/Cluster-Affinity/Cluster-Affinity-1.png)

在 PowerShell 清單的群組中，您會看到這個：

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>其他註解

- 請確定您使用正確 AntiAffinity 的設定值，視需求而定。
- 請記住，在兩個節點的案例和 ClusterEnforcedAntiAffinity，如果一個節點已關閉，這兩個群組將不會執行。  

- 在群組中使用慣用的擁有者可以結合 AntiAffinity，三個或多個節點叢集中。
- AntiAffinityClassNames 和 ClusterEnforcedAntiAffinity 設定才會在資源回收之後。 I.E. 您可以設定它們，但如果這兩個群組都在線上的相同節點上設定時，它們都會繼續保持線上狀態。



