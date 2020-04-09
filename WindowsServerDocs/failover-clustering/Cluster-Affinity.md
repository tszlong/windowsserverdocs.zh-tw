---
title: 叢集親和性
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/07/2019
description: 本文說明容錯移轉叢集親和性和 antiAffinity 層級
ms.openlocfilehash: c9910cac602802b753391fad1009fb7f1fa3d2f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828281"
---
# <a name="cluster-affinity"></a>叢集親和性

> 適用于： Windows Server 2019、Windows Server 2016

容錯移轉叢集可以保存許多可在節點之間移動並執行的角色。  有些時候，某些角色（例如虛擬機器、資源群組等）不應該在相同的節點上執行。  這可能是因為資源耗用量、記憶體使用量等。 例如，有兩部虛擬機器耗用記憶體和 CPU，而且如果兩部虛擬機器在相同的節點上執行，則其中一部或兩部虛擬機器可能會影響效能問題。  本文將說明叢集 antiaffinity 層級，以及您可以如何使用它們。

## <a name="what-is-affinity-and-antiaffinity"></a>什麼是親和性和 AntiAffinity？

[相似性] 是您設定的規則，可在兩個或多個角色（i、e、虛擬機器、資源群組等）之間建立關聯性，以將它們保持在一起。  AntiAffinity 是相同的，但用於嘗試並讓指定的角色彼此分開。  容錯移轉叢集會使用 AntiAffinity 作為其角色。  更明確地說，在角色上定義的[AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames)參數，因此不會在相同的節點上執行。  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

查看群組的屬性時，會有參數 AntiAffinityClassNames，而且預設值為空白。  在下列範例中，Group1 和 Group2 應該與在相同節點上執行的分開。  若要查看屬性，PowerShell 命令和結果會是：

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

由於 AntiAffinityClassNames 未定義為預設值，因此這些角色可以一起或分開執行。  其目標是要讓它們分開。  AntiAffinityClassNames 的值可以是任何您想要的值，只需要相同。  假設 [Group1] 和 [Group2] 是在虛擬機器中執行的網域控制站，而且在不同節點上執行的是最佳服務。  因為這些都是網域控制站，所以我將使用 DC 做為類別名稱。  若要設定此值，PowerShell 命令和結果會是：

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

設定好之後，容錯移轉叢集就會嘗試將它們分開。  

AntiAffinityClassName 參數是「軟性」區塊。  也就是說，它會嘗試將它們分開，但如果無法這麼做，仍然可以讓它們在相同的節點上執行。  例如，群組會在兩個節點的容錯移轉叢集上執行。  如果一個節點需要關閉進行維護，這表示兩個群組會在相同節點上啟動並執行。  在此情況下，您可以這麼做。  這可能不是最理想的做法，但這兩部 virtial 機器仍會在可接受的效能範圍內執行。

## <a name="i-need-more"></a>我需要更多

如前所述，AntiAffinityClassNames 是一個軟區塊。  但如果需要使用硬區塊，該怎麼辦？  虛擬機器無法在他相同的節點上執行;否則，將會影響效能，並導致某些服務可能會中斷。

在這些情況下，有一個額外的 cluster 屬性 ClusterEnforcedAntiAffinity。  此 antiaffinity 層級可避免在相同節點上執行相同 AntiAffinityClassNames 值的所有成本。

若要查看屬性和值，PowerShell 命令（和結果）會是：

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

"0" 的值表示它已停用而不會強制執行。  "1" 的值會啟用它，而是硬區塊。  若要啟用此硬區塊，命令（和結果）為：

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

當這兩個都已設定時，將無法將群組同時上線。  如果它們位於相同的節點上，這就是您會在容錯移轉叢集管理員中看到的內容。

![叢集親和性](media/Cluster-Affinity/Cluster-Affinity-1.png)

在群組的 PowerShell 清單中，您會看到：

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>其他批註

- 請確定您使用的是適當的 AntiAffinity 設定（視需求而定）。
- 請記住，在雙節點案例和 ClusterEnforcedAntiAffinity 中，如果某個節點已關閉，這兩個群組將不會執行。  

- 在群組上使用慣用的擁有者，可以在三個或更多節點叢集中與 AntiAffinity 結合。
- 只有在資源回收之後，才會發生 AntiAffinityClassNames 和 ClusterEnforcedAntiAffinity 設定。 亦即. 您可以設定它們，但如果兩個群組在設定的相同節點上都處於線上狀態，則兩者都會繼續保持線上狀態。



