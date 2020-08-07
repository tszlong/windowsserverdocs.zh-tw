---
title: 新增系統管理員信任證明的主機資訊
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: abc01dbb691843d199169bd654afaa5cf06bbb87
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971435"
---
# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>使用系統管理員信任的證明來授權 Hyper-v 主機

> 適用於：Windows Server (半年度管道)、Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 2019 開始，系統管理員信任的證明 (AD 模式) 已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。


若要以 AD 模式授權受防護主機：

1. 在網狀架構網域中，將 Hyper-v 主機新增至安全性群組。
2. 在 HGS 網域中，向 HGS 註冊安全性群組的 SID。

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>將 Hyper-v 主機新增至安全性群組，並將主機重新開機

1. 在網狀架構網域中建立**全域**安全性群組，並新增將執行受防護 Vm 的 hyper-v 主機。
   重新開機主機以更新其群組成員資格。

2. 使用 New-adgroup 來取得安全性群組 (SID) 的安全識別碼，並將其提供給 HGS 系統管理員。

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![具有輸出的 New-adgroup 命令](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 註冊安全性群組的 SID

1. 從網狀架構系統管理員取得受防護主機的安全性群組 SID，然後執行下列命令，向 HGS 註冊安全性群組。
   視需要重新執行命令以進行其他群組。
   為群組提供易記的名稱。
   不需要符合 Active Directory 安全性群組名稱。

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要確認已新增群組，請執行[HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。


