---
description: 深入瞭解：選擇是否要在自己專用的樹系或現有的防禦樹系中安裝 HGS
title: 選擇是否要在自己的新樹系或現有的防禦樹系中安裝 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: cceea24bb4f843df06854e893742c79d25034021
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049896"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>選擇是否要在自己專用的樹系或現有的防禦樹系中安裝 HGS

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016


HGS 的 Active Directory 樹系很敏感，因為它的系統管理員可以存取控制受防護 Vm 的金鑰。
預設安裝會設定 HGS 專用的新樹系並設定其他相依性。
建議使用這個選項，因為在建立環境時，環境是獨立的，而且已知是安全的。

在現有樹系中安裝 HGS 的唯一技術需求是將它新增至根域;不支援非根域。 但是使用現有的樹系時，也有操作需求和安全性相關的最佳做法。
您可以刻意建立適當的樹系，以提供一個敏感性功能，例如 [Privileged Access Management 用於 AD DS](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 的樹系，或 [ (ESAE) 樹系的增強式安全性系統管理環境](../../identity/securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)。
這類樹系通常會展示下列特性：

- 它們的管理員 (與網狀架構管理員不同) 
- 他們的登入次數很少
- 它們在本質上並不是一般用途

一般用途樹系，例如生產樹系，不適合 HGS 使用。
網狀架構樹系也不適合，因為 HGS 需要與網狀架構系統管理員隔離。

## <a name="next-step"></a>後續步驟

選擇最適合您環境的安裝選項：

- [在自己專用的樹系中安裝 HGS](guarded-fabric-install-hgs-default.md)
- [在現有的防禦樹系中安裝 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)
