---
title: 建立受防護主機的安全性群組，並向 HGS 註冊群組
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: fb84720b94746a3c5757037ceb5c9bc8c965ff7f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447159"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>建立受防護主機的安全性群組，並向 HGS 註冊群組

>適用於：Windows Server （半年通道），Windows Server 2016

>[!IMPORTANT]
>AD 模式開頭為 Windows Server 2019 已被取代。 TPM 證明不可能的環境下，設定[裝載金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 


本主題說明準備 HYPER-V 主機，才能使用系統管理信任證明 （AD 模式） 的受防護的主機的中繼步驟。 執行這些步驟，來完成中的步驟[設定會變成受防護的主機的主機網狀架構 DNS](guarded-fabric-configuring-fabric-dns-ad.md)。


## <a name="create-a-security-group-and-add-hosts"></a>建立安全性群組，並將主機新增

1. 建立新**GLOBAL**安全性 fabric 網域中群組和新增 HYPER-V 主機執行受防護的 Vm。 重新啟動主機，以更新其群組成員資格。

2. 您可以使用 Get ADGroup 來取得安全性群組的安全性識別碼 (SID)，並提供給 HGS 系統管理員。 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![取得 AdGroup 命令輸出](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 註冊安全性群組 SID  

1. HGS 伺服器上，執行下列命令，以向 HGS 註冊的安全性群組。 
   如果所需的其他群組，請重新執行命令。 
   提供群組的易記名稱。 
   它不需要符合 Active Directory 安全性群組名稱。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要確認已加入該群組，請執行[Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [確認證明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>另請參閱

- [部署主機守護者服務的受防護主機和受防護的 Vm](guarded-fabric-deploying-hgs-overview.md)
