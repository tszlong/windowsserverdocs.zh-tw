---
description: 深入瞭解：使用系統管理員信任的證明授權 Hyper-v 主機
title: 新增系統管理員信任證明的主機資訊
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 0a4a9f7ebf3f88f6e19c4a78cb8dabd66c2004b9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047536"
---
# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>使用系統管理員信任的證明授權 Hyper-v 主機

> 適用於：Windows Server (半年度管道)、Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 2019 開始，已淘汰 (AD 模式) 的管理員信任證明。 針對不可能進行 TPM 證明的環境，請設定 [主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似于 AD 模式的保證，而且更容易設定。


若要以 AD 模式授權受防護主機：

1. 在網狀架構網域中，將 Hyper-v 主機新增至安全性群組。
2. 在 HGS 網域中，向 HGS 註冊安全性群組的 SID。

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>將 Hyper-v 主機新增至安全性群組，並重新啟動主機

1. 在網狀架構網域中建立 **全域** 安全性群組，並新增將執行受防護 Vm 的 hyper-v 主機。
   重新開機主機以更新其群組成員資格。

2. 使用 Get-ADGroup 取得安全性群組 (SID) 的安全識別碼，並將它提供給 HGS 系統管理員。

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![具有 output 的 Get-AdGroup 命令](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>使用 HGS 註冊安全性群組的 SID

1. 從網狀架構系統管理員取得受保護主機的安全性群組 SID，然後執行下列命令以向 HGS 註冊安全性群組。
   視需要重新執行命令以進行其他群組。
   提供群組的易記名稱。
   它不需要符合 Active Directory 安全性群組名稱。

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要確認已新增群組，請執行 [HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。


