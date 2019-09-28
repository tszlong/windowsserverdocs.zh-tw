---
title: Windows 系統管理中心的使用者存取選項
description: Windows 管理中心的使用者存取選項和識別提供者（Project 檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356924"
---
# <a name="user-access-options-with-windows-admin-center"></a>Windows 系統管理中心的使用者存取選項

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在 Windows Server 上部署時，Windows 管理中心會針對您的伺服器環境提供集中管理點。 藉由控制 Windows 管理中心的存取權，您可以改善管理環境的安全性。

## <a name="gateway-access-roles"></a>閘道存取角色

Windows 管理中心會定義兩個角色來存取閘道服務：閘道使用者和閘道系統管理員。

> [!NOTE]
> 對閘道的存取並不表示存取閘道所見的目標伺服器。 若要管理目標伺服器，使用者必須使用在目標伺服器上具有系統管理許可權的認證進行連接。

**閘道使用者**可以連線到 Windows 管理中心閘道服務，以便透過該閘道管理伺服器，但無法變更存取權限，也無法變更用來驗證閘道的驗證機制。

**閘道系統管理員**可以設定誰取得存取權，以及使用者如何向閘道進行驗證。

>[!NOTE]
> 如果 Windows 系統管理中心內沒有定義任何存取群組，這些角色會反映 Windows 帳戶對閘道伺服器的存取權。 

[在 Windows 管理中心設定閘道使用者和系統管理員存取權。](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>識別提供者選項

閘道系統管理員可以選擇下列其中一項：

 - [Active Directory/本機電腦群組](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory 做為 Windows 系統管理中心的識別提供者](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>智慧卡驗證

使用 Active Directory 或本機電腦群組做為身分識別提供者時，您可以要求存取 Windows 管理中心的使用者是其他以智慧卡為基礎的安全性群組的成員，藉以強制執行智慧卡驗證。 [在 Windows 系統管理中心內設定智慧卡驗證。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和多重要素驗證

藉由要求閘道 Azure AD 驗證，您可以利用 Azure AD 所提供的其他安全性功能，例如條件式存取和多重要素驗證。 [深入瞭解如何使用 Azure Active Directory 來設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>角色型存取控制

根據預設，使用者在想要使用 Windows 系統管理中心管理的電腦上，需要有完整的本機系統管理員許可權。
這可讓他們從遠端連線到電腦，並確保它們具有足夠的許可權來查看和修改系統設定。
不過，某些使用者可能不需要對電腦進行不受限制的存取來執行其工作。
您可以使用 Windows 系統管理中心的**角色型存取控制**，為這類使用者提供電腦的有限存取權，而不是讓他們成為完整的本機系統管理員。

Windows 系統管理中心的角色型存取控制的運作方式，是使用 PowerShell 來設定每部受管理伺服器，[就夠了足夠的管理](https://aka.ms/jeadocs)端點。
此端點會定義角色，包括系統每個角色可以管理的部分，以及要將哪些使用者指派給該角色。
當使用者連線到受限制的端點時，會建立暫時的本機系統管理員帳戶來代表其管理系統。
這可確保即使是沒有自己委派模型的工具，仍然可以使用 Windows 系統管理中心來管理。
當使用者透過 Windows 系統管理中心停止管理電腦時，就會自動移除暫存帳戶。

當使用者連線到設定了角色型存取控制的電腦時，Windows 管理中心會先檢查它們是否為本機系統管理員。
如果是，他們會收到完整的 Windows 管理中心體驗，而且沒有任何限制。
否則，Windows 管理中心會檢查使用者是否屬於任何預先定義的角色。
如果使用者屬於 Windows 系統管理中心角色，但不是完整的系統管理員，則會被視為具有*有限的存取權*。
最後，如果使用者既不是系統管理員，也不是角色的成員，則會被拒絕存取來管理電腦。

以角色為基礎的存取控制適用于伺服器管理員和容錯移轉叢集解決方案。

### <a name="available-roles"></a>可用的角色

Windows 系統管理中心支援下列終端使用者角色：

角色名稱 | 預定用途
----------|-------------
Administrators | 可讓使用者在 Windows 系統管理中心使用大部分的功能，而不需授與他們存取遠端桌面或 PowerShell 的許可權。 此角色適用于您想要限制電腦上管理進入點的「跳躍伺服器」案例。
讀取者 | 可讓使用者在伺服器上查看資訊和設定，但不能進行變更。
Hyper-V 系統管理員 | 可讓使用者變更 Hyper-v 虛擬機器和交換器，但會將其他功能限制為唯讀存取。

當使用者以有限的存取權連接時，下列內建延伸模組的功能會降低：

- 檔案（不上傳或下載檔案）
- PowerShell （無法使用）
- 遠端桌面（無法使用）
- 儲存體複本（無法使用）

目前，您無法為您的組織建立自訂角色，但可以選擇哪些使用者會被授與每個角色的存取權。

### <a name="preparing-for-role-based-access-control"></a>準備以角色為基礎的存取控制

若要利用暫時的本機帳戶，必須設定每部目的電腦，以支援 Windows 系統管理中心的角色型存取控制。
設定程式牽涉到使用 Desired State Configuration 在電腦上安裝 PowerShell 腳本和足夠的管理端點。

如果您只有幾部電腦，您可以使用 Windows 系統管理中心的角色型存取控制頁面，輕鬆地將設定個別套用到每部電腦。
當您在個別電腦上設定以角色為基礎的存取控制時，會建立本機安全性群組來控制每個角色的存取權。
您可以將存取權授與使用者或其他安全性群組，方法是將它們新增為角色安全性群組的成員。

若要在多部電腦上進行全企業部署，您可以從閘道下載設定腳本，然後使用 Desired State Configuration 提取伺服器、Azure 自動化或慣用的管理工具將它散發到您的電腦。
