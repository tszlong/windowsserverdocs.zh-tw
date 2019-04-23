---
title: 新增系統管理信任證明的主機資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849459"
---
>適用於：Windows Server （半年通道），Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>授權使用系統管理信任證明的 HYPER-V 主機

>[!IMPORTANT]
>已被取代開頭為 Windows Server 2019 的系統管理信任證明 （AD 模式）。 TPM 證明不可能的環境下，設定[裝載金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 


若要在 AD 模式中的受防護的主機： 

1. 在網狀架構的網域中，HYPER-V 將主機新增至安全性群組。
2. 在 HGS 網域中，請向 HGS 註冊安全性群組的 SID。 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>新增 HYPER-V 主機的安全性群組，並重新啟動主機

1. 建立**GLOBAL**安全性 fabric 網域中群組和新增 HYPER-V 主機執行受防護的 Vm。 
   重新啟動主機，以更新其群組成員資格。

2. 您可以使用 Get ADGroup 來取得安全性群組的安全性識別碼 (SID)，並提供給 HGS 系統管理員。 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![取得 AdGroup 命令輸出](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 註冊安全性群組 SID  

1. 取得受防護主機網狀架構系統管理員安全性群組的 SID，並執行下列命令，以向 HGS 註冊的安全性群組。 
   如果所需的其他群組，請重新執行命令。 
   提供群組的易記名稱。 
   它不需要符合 Active Directory 安全性群組名稱。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要確認已加入該群組，請執行[Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 


