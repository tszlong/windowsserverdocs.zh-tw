---
title: 使用 Windows Admin Center 的使用者存取選項
description: 使用者存取選項和與 Windows Admin Center （專案檀香山） 的身分識別提供者
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825889"
---
# <a name="user-access-options-with-windows-admin-center"></a>使用 Windows Admin Center 的使用者存取選項

>適用於：Windows Admin Center，Windows Admin Center 預覽

部署 Windows Server 上，Windows Admin Center 提供集中式的伺服器環境的管理點。 藉由控制存取 Windows Admin Center，您可以改善您的管理架構的安全性。

## <a name="gateway-access-roles"></a>閘道存取角色

Windows Admin Center 到閘道服務會定義兩個角色進行存取： 閘道使用者和閘道系統管理員。

> [!NOTE]
> 閘道，閘道存取權並不表示可見的目標伺服器的存取權。 若要管理目標伺服器，使用者必須使用目標伺服器具有系統管理權限的認證連線。

**閘道使用者**可以連線至 Windows Admin Center 閘道服務，以便管理伺服器，透過該閘道，但無法變更存取權限也用來驗證閘道的驗證機制。

**閘道管理員**可以設定使用者取得存取權以及如何使用者會向閘道。

>[!NOTE]
> 如果沒有 Windows Admin Center 中定義的存取群組，角色會反映到閘道伺服器的 Windows 帳戶存取。 

[在 Windows Admin Center 中設定閘道的使用者和系統管理員存取。](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>身分識別提供者選項

閘道系統管理員可以選擇下列其中一項：

 - [Active Directory] / [本機電腦群組](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory，為 Windows Admin Center 的身分識別提供者](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>智慧卡驗證

當使用 Active Directory 或本機電腦群組做為身分識別提供者，您可以需要存取其他智慧卡為基礎的安全性群組成員的 Windows Admin Center 的使用者強制執行智慧卡驗證。 [在 Windows Admin Center 中設定智慧卡驗證。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和 multi-factor authentication

閘道要求 Azure AD 驗證，您可以利用額外的安全性功能，例如條件式存取與 Azure AD 所提供的多重要素驗證。 [深入了解與 Azure Active Directory 中設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>角色型存取控制

根據預設，使用者會需要完整的本機系統管理員權限，他們想要使用 Windows Admin Center 進行管理的電腦上。
這可讓他們從遠端連接至電腦，並確保它們有足夠的權限來檢視和修改系統設定。
不過，某些使用者可能不需要執行其工作電腦的無限制地的存取。
您可以使用**角色型存取控制**中為這類使用者提供的有限存取權的機器，而不是將它們完整的本機系統管理員的 Windows Admin Center。

Windows Admin Center 中的角色型存取控制的運作方式是使用 PowerShell 設定每個受管理的伺服器[Just Enough Administration](https://aka.ms/jeadocs)端點。
此端點定義的角色，包括哪些方面的系統管理允許每個角色，以及哪些使用者指派給角色。
當使用者連線到受限制的端點時，供您管理代表他們執行系統建立暫時的本機系統管理員帳戶。
這可確保仍然可以使用 Windows Admin Center 管理甚至工具並沒有自己的委派模型。
當使用者停止管理機器透過 Windows Admin Center 時，會自動移除暫時帳戶。

當使用者連線到機器，使用角色型存取控制設定時，Windows Admin Center 會先檢查它們是否為本機系統管理員。
如有需要，就會收到完整的 Windows Admin Center 體驗，而無任何限制。
否則會檢查 Windows Admin Center，是否使用者所屬的任何預先定義的角色。
使用者即具有*有限存取*如果它們屬於 Windows Admin Center 角色，但不是完整的系統管理員。
最後，如果使用者不是系統管理員或角色的成員，它們會被拒絕存取來管理電腦。

以角色為基礎的存取控制是適用於伺服器管理員和容錯移轉叢集解決方案。

### <a name="available-roles"></a>可用的角色

Windows Admin Center 支援下列使用者角色：

角色名稱 | 預定用途
----------|-------------
Administrators | 可讓使用者在 Windows Admin Center 使用的大部分功能，而不授與他們存取遠端桌面或 PowerShell。 此角色是適用於您想要用來限制管理進入點，在電腦上的 「 跳躍伺服器 」 案例。
讀取者 | 可讓使用者在伺服器上，檢視資訊和設定，但不是進行變更。
Hyper-V 系統管理員 | 可讓使用者對 HYPER-V 虛擬機器和交換器，進行變更，但會限制為唯讀存取的其他功能。

當使用者具備有限存取權，下列內建的延伸模組有精簡功能：

- 檔案 （不含檔案上傳或下載）
- PowerShell （無法使用）
- 遠端桌面 （無法使用）
- （無法使用） 的儲存體複本

在此階段中，您不能為您的組織建立自訂角色，但您可以選擇哪些使用者會獲得存取權的每個角色。

### <a name="preparing-for-role-based-access-control"></a>準備進行角色型存取控制

若要利用暫時的本機帳戶，每個目標電腦必須設定為支援 Windows Admin Center 中的角色型存取控制。
設定程序牽涉到使用 Desired State Configuration 在電腦上安裝 PowerShell 指令碼和 Just Enough Administration 的端點。

如果您只有少數電腦，您可以輕鬆地套用設定個別 Windows Admin Center 中使用的角色型存取控制 頁面每一部電腦。
當您設定的個別電腦上的角色型存取控制時，會建立本機的安全性群組，來控制存取權的每個角色。
您可以將它們新增為成員的角色安全性的群組授與存取給使用者或其他安全性群組。

針對企業部署多部電腦上，可以從閘道中下載設定指令碼，並將它散發給您使用 Desired State Configuration 提取伺服器、 Azure 自動化中或您慣用的管理工具的電腦。
