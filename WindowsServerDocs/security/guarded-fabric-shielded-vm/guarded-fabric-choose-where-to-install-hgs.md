---
title: 選擇是否要安裝它自己的新樹系中，或在現有的防禦樹系中的 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827499"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>選擇是否要在其自己專用的樹系中，或在現有的防禦樹系中安裝 HGS

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


HGS 的 Active Directory 樹系是機密的因為其系統管理員可以控制受防護的 Vm 的金鑰存取。 預設安裝會設定新的樹系專用 HGS 並設定其他相依性。 此選項是建議使用，因為是獨立的環境，且已知會在建立時為安全。 

在現有樹系中安裝 HGS 只有技術需求就是它會加入至根網域中;不支援非根網域。 但也有作業需求和使用現有的樹系的安全性相關最佳作法。 提供一個機密的函式，例如所使用的樹系刻意建置適合的樹系[Privileged Access Management，適用於 AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)或[增強式安全性系統管理環境 (ESAE) 樹系](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). 這類的樹系通常會展現下列特性：

- 它們有幾個系統管理員 （獨立於網狀架構系統管理員）
- 它們具有較少的登入
- 它們不是一般用途在本質上 

一般用途樹系，例如生產環境樹系並不適合使用的 HGS。 網狀架構樹系也是不適合的因為 HGS 需要隔離從網狀架構系統管理員。

## <a name="next-step"></a>後續步驟

選擇最適合您環境的安裝選項：

- [其自己專用的樹系中安裝 HGS](guarded-fabric-install-hgs-default.md)
- [在 現有的防禦樹系中安裝 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)


