---
title: Windows Admin Center 的使用者存取選項
description: Windows Admin Center 的使用者存取選項和識別提供者 (Project Honolulu)
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 184eca56dc14e91220a7fb7eb196c48706562ff7
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766711"
---
# <a name="user-access-options-with-windows-admin-center"></a>Windows Admin Center 的使用者存取選項

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在部署於 Windows Server 上時，Windows Admin Center 可讓您在集中的位置管理伺服器環境。 藉由控制 Windows Admin Center 的存取權，您可以改善管理環境的安全性。

## <a name="gateway-access-roles"></a>閘道存取角色

Windows Admin Center 會定義兩個可存取閘道服務的角色：閘道使用者和閘道系統管理員。

> [!NOTE]
> 可存取閘道並不表示就能存取閘道看得見的目標伺服器。 若要管理目標伺服器，使用者必須使用在目標伺服器上具有系統管理權限的認證來連線。

**閘道使用者**可以連線到 Windows Admin Center 閘道服務，以透過該閘道來管理伺服器，但無法變更存取權限，也無法變更用來驗證閘道的驗證機制。

**閘道系統管理員**可以設定取得存取權的人員，以及使用者向閘道進行驗證的方式。

>[!NOTE]
> 如果 Windows Admin Center 內沒有定義任何存取群組，則這些角色會反映 Windows 帳戶對閘道伺服器的存取權。

[在 Windows Admin Center 中設定閘道使用者和系統管理員的存取權。](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>識別提供者選項

閘道系統管理員可以選擇下列其中一項：

 - [Active Directory/本機機器群組](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [以 Azure Active Directory 作為 Windows Admin Center 的識別提供者](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>智慧卡驗證

使用 Active Directory 或本機機器群組作為識別提供者時，您可以要求存取 Windows Admin Center 的使用者必須是其他智慧卡型安全性群組的成員，藉以強制執行智慧卡驗證。 [在 Windows Admin Center 中設定智慧卡驗證。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和多重要素驗證

藉由針對閘道來要求進行 Azure AD 驗證，您可以利用 Azure AD 所提供的其他安全性功能，例如條件式存取和多重要素驗證。 [深入了解如何使用 Azure Active Directory 來設定條件式存取。](/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>角色型存取控制

根據預設，使用者在想要使用 Windows Admin Center 管理的機器上，必須有完整的本機系統管理員權限。
這可讓使用者從遠端連線到機器，並確保其具有足夠權限可檢視和修改系統設定。
不過，某些使用者可能不需要獲得機器的無限制存取權就能執行其作業。
您可以在 Windows Admin Center 中使用**角色型存取控制**，來為這類使用者提供機器的有限存取權，而不是讓其成為完整的本機系統管理員。

Windows Admin Center 中的角色型存取控制運作方式，是使用 PowerShell 的 [Just Enough Administration](/powershell/scripting/learn/remoting/jea/overview) 端點來設定每部受管理的伺服器。
此端點會定義各種角色，包括每個角色所能夠管理的系統部分，以及要將哪些使用者指派給該角色。
當使用者連線到受限制的端點時，系統會建立暫時的本機系統管理員帳戶來代表其管理系統。
這可確保即使是沒有自有委派模型的工具，仍可使用 Windows Admin Center 來管理。
當使用者停止透過 Windows Admin Center 來管理機器時，系統就會自動移除暫時帳戶。

當使用者連線到設定了角色型存取控制的機器時，Windows Admin Center 會先檢查其是否為本機系統管理員。
如果是，其便會收到沒有任何限制的完整 Windows Admin Center 體驗。
否則，Windows Admin Center 會檢查使用者是否屬於任何預先定義的角色。
如果使用者屬於 Windows Admin Center 角色，但不是完整的系統管理員，就表示其具有「有限的存取權」  。
最後，如果使用者既不是系統管理員，也不是角色的成員，則系統會拒絕其管理機器的存取權。

角色型存取控制適用於伺服器管理員和容錯移轉叢集解決方案。

### <a name="available-roles"></a>可用的角色

Windows Admin Center 支援下列終端使用者角色：

角色名稱 | 預定用途
----------|-------------
Administrators | 可讓使用者使用 Windows Admin Center 中的大部分功能，而不需要向其授與遠端桌面或 PowerShell 的存取權。 此角色適用於您想要在機器上限制管理進入點的「跳躍伺服器」案例。
讀取者 | 可讓使用者檢視伺服器上的資訊和設定，但不能進行變更。
Hyper-V 系統管理員 | 可讓使用者變更 Hyper-V 虛擬機器和交換器，但會將其他功能限制為唯讀存取權。

當使用者以有限的存取權連線時，下列內建延伸模組的功能會有所縮減：

- 檔案 (不能上傳或下載檔案)
- PowerShell (無法使用)
- 遠端桌面 (無法使用)
- 儲存體複本 (無法使用)

目前您無法為組織建立自訂角色，但可以選擇要向哪些使用者授與每個角色的存取權。

### <a name="preparing-for-role-based-access-control"></a>針對角色型存取控制做準備

若要利用暫時的本機帳戶，您必須在 Windows Admin Center 中將每個目標機器設定為支援角色型存取控制。
此設定程序牽涉到使用 Desired State Configuration 在機器上安裝 PowerShell 指令碼和 Just Enough Administration 端點。

如果您只有幾部電腦，則可以在 Windows Admin Center 中使用角色型存取控制頁面，輕鬆地對每部電腦個別套用設定。
當您在個別電腦上設定角色型存取控制時，系統會建立本機安全性群組來控制每個角色的存取權。
您可以將存取權授與給使用者或其他安全性群組，方法是將其新增為角色安全性群組的成員。

若要在多部機器上進行全企業的部署，您可以從閘道下載設定指令碼，然後使用 Desired State Configuration 提取伺服器、Azure 自動化或慣用管理工具將該指令碼散發到您的所有電腦。