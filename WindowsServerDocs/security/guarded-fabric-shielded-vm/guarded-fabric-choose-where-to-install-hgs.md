---
title: 選擇是否要在自己的新樹系或現有的防禦樹系中安裝 HGS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 52376a4e193b8021dc58214003e9b7579b096a79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856881"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>選擇是否要在其專屬的樹系或現有的防禦樹系中安裝 HGS

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016


HGS 的 Active Directory 樹系是敏感性的，因為它的系統管理員可以存取控制受防護 Vm 的金鑰。 預設安裝會設定一個專門用於 HGS 的新樹系，並設定其他相依性。 建議使用此選項，因為環境是獨立的，而且在建立時是已知的安全。 

在現有樹系中安裝 HGS 的唯一技術需求是將它新增至根域;不支援非根域。 但也有使用現有樹系的操作需求和安全性相關的最佳作法。 適當的樹系專門用來提供一個敏感性功能，例如 AD DS 的特殊許可權[存取管理](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)所使用的樹系，或[增強式安全性系統管理環境（ESAE）樹](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM)系。 這類樹系通常會展現下列特性：

- 他們有少數的系統管理員（與網狀架構管理員不同）
- 它們的登入次數很少
- 它們本質上並不是一般用途 

一般用途的樹系（例如生產樹系）不適合供 HGS 使用。 網狀架構樹系也不適合，因為 HGS 必須與網狀架構系統管理員隔離。

## <a name="next-step"></a>後續步驟

選擇最適合您環境的安裝選項：

- [在專屬的專用樹系中安裝 HGS](guarded-fabric-install-hgs-default.md)
- [在現有的防禦樹系中安裝 HGS](guarded-fabric-install-hgs-in-a-bastion-forest.md)


