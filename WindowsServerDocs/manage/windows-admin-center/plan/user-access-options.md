---
title: 使用 Windows Admin Center 使用者存取選項
description: 使用者存取選項和身分識別提供者，使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2019
ms.locfileid: "9151993"
---
# 使用 Windows Admin Center 使用者存取選項

>適用於：Windows Admin Center、Windows Admin Center 預覽版

部署 Windows Server 上，Windows Admin Center 提供集中式的管理用於伺服器環境的點。 您可以控制存取 Windows Admin Center，來改善您的管理橫向的安全性。

## 閘道存取角色

Windows Admin Center 閘道服務來定義的存取權的兩個角色： 閘道使用者和閘道系統管理員。

> [!NOTE]
> 閘道存取並不代表閘道的存取權的可見的目標伺服器。 若要管理目標伺服器，使用者必須連線使用目標伺服器擁有系統管理員權限的認證。

**閘道使用者**時，可以連線到 Windows Admin Center 閘道服務上，才能管理伺服器，透過該閘道，但是他們無法變更存取權限，也不用來驗證對閘道的驗證機制。

**閘道系統管理員**可以設定讓哪些人員取得存取以及如何使用者會向閘道。

>[!NOTE]
> 如果沒有 Windows Admin Center 中定義的存取權群組，角色將反映閘道伺服器的 Windows 帳戶存取權。 

[設定 Windows Admin Center 閘道使用者和系統管理員存取權。](../configure/user-access-control.md)

## 身分識別提供者選項

閘道系統管理員可以選擇下列其中一項：

 - [Active Directory/鎖定的本機電腦群組](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Windows Admin Center 的身分識別提供者為 azure Active Directory](../configure/user-access-control.md#azure-active-directory)


### 智慧卡驗證

當使用 Active Directory 或本機電腦群組做為身分識別提供者，您可以要求存取 Windows Admin Center 是額外的智慧卡為基礎的安全性群組的成員的使用者以強制執行智慧卡驗證。 [設定 Windows Admin Center 中的智慧卡驗證。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### 條件式存取和多重要素驗證

需要 Azure AD 驗證的閘道，您可以利用額外的安全性功能，例如條件式存取和 Azure AD 所提供的多重要素驗證。 [深入了解使用 Azure Active Directory 中設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## 角色型存取控制

根據預設，使用者不需要在他們想要使用 Windows Admin Center 管理電腦上的完整本機系統管理員權限。
這可讓他們能夠從遠端連線到電腦，並確保它們有充足的權限，若要檢視和修改系統設定。
不過，某些使用者可能不需要不受限制的存取權來執行他們的工作電腦。
您可以使用 Windows Admin Center 中的**角色型存取控制**來提供這類使用者有限的存取，而不是讓它們的完整本機系統管理員的電腦。

Windows Admin Center 中的角色型存取控制的運作方式是使用 PowerShell [Just Enough Administration](https://aka.ms/jeadocs)端點設定每個受管理的伺服器。
此端點定義的角色，包括系統的什麼層面允許每個角色來管理，以及哪些使用者指派角色。
當使用者連接到受限制的端點時，管理，代表他們的系統建立暫時的本機系統管理員帳戶。
這樣可確保仍然可以使用 Windows Admin Center 管理即使這不會有自己的委派模型的工具。
當使用者停止管理的電腦，透過 Windows Admin Center，並自動移除暫時的帳戶。

當使用者連接至使用角色型存取控制設定的電腦時，Windows Admin Center 會先檢查它們是否本機系統管理員。
如果它們是，他們將會收到沒有限制的完整 Windows Admin Center 體驗。
否則，Windows Admin Center 會檢查，是否使用者屬於任何預先定義的角色。
使用者即如果它們屬於 Windows Admin Center 角色，但不是完整的系統管理員具備*有限的存取*。
最後，如果使用者是以系統管理員都角色的成員，它們將會遭到拒絕存取權管理電腦。

使用伺服器管理員和容錯移轉叢集解決方案角色型存取控制。

### 可用的角色

Windows Admin Center 支援下列的使用者角色：

角色名稱 | 預定用途
----------|-------------
管理員 | 可讓使用者使用 Windows Admin Center 中的大多數功能，而不會授與遠端桌面或 PowerShell 來存取。 此角色很適合想要限制的管理進入點，在電腦上的 「 跳躍伺服器 」 案例。
閱讀程式 | 可讓使用者檢視資訊及設定的伺服器上，但無法進行變更。
HYPER-V 系統管理員 | 允許使用者進行變更與 HYPER-V 虛擬機器和交換器，但是會限制為唯讀存取權的其他功能。

當使用者連接具備有限的存取權，下列內建的延伸模組已降低功能：

- 檔案 （沒有檔案上傳或下載）
- PowerShell （無法使用）
- 遠端桌面 （無法使用）
- 儲存體複本 （無法使用）

在這個時候，您無法為您的組織建立自訂的角色，但您可以選擇哪些使用者已授與存取每個角色。

### 準備角色型存取控制

若要利用暫時的本機帳戶，每部目標電腦必須以支援 Windows Admin Center 中的角色型存取控制設定。
設定程序涉及在使用預期狀態設定的電腦上安裝 PowerShell 指令碼和 Just Enough Administration 端點。

如果您只會有幾台電腦，您可以輕鬆地套用設定個別 Windows Admin Center 中使用角色型存取控制頁面上的每個電腦。
當您設定個別電腦上的角色型存取控制時，來控制存取每個角色建立本機安全性群組。
您可以將他們新增角色安全性群組的成員為授與存取使用者或其他安全性群組。

針對多部電腦上的企業部署，您可以從閘道下載設定指令碼，並將其提供給您的電腦使用預期狀態設定提取伺服器、 Azure 自動化或您慣用的管理工具。
