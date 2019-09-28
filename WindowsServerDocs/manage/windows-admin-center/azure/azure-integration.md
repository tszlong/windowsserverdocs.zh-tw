---
title: 正在設定 Azure 整合
description: 設定 Azure 整合 Windows 管理中心（Project 檀香山）。 將您的 Windows 管理中心閘道連接到 Azure。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357365"
---
# <a name="configuring-azure-integration"></a>正在設定 Azure 整合

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 系統管理中心支援數個與 Azure 服務整合的選用功能。 [瞭解 Windows 管理中心提供的 Azure 整合選項。](../plan/azure-integration-options.md)

若要允許 Windows 管理中心閘道與 Azure 通訊，以利用 Azure Active Directory 驗證來進行閘道存取，或代表您建立 Azure 資源（例如，使用 Azure 網站保護 Windows 管理中心內受管理的 Vm）復原），您必須先向 Azure 註冊您的 Windows 系統管理中心閘道。 您只需要針對您的 Windows 管理中心閘道執行此動作-當您將閘道更新為較新版本時，會保留此設定。

## <a name="register-your-gateway-with-azure"></a>向 Azure 註冊您的閘道

第一次嘗試在 Windows 管理中心使用 Azure 整合功能時，系統會提示您向 Azure 註冊閘道。 您也可以前往 Windows 管理中心 [設定] 中的 [ **Azure** ] 索引標籤來註冊閘道。

引導式的產品中步驟會在您的目錄中建立 Azure AD 應用程式，讓 Windows 系統管理中心能夠與 Azure 通訊。 若要查看自動建立的 Azure AD 應用程式，請移至 Windows 管理中心 [設定] 的 [ **Azure** ] 索引標籤。 Azure 超連結**中的 view**可讓您在 Azure 入口網站中查看 Azure AD 應用程式。 

建立的 Azure AD 應用程式會用於 Windows 系統管理中心內的所有 Azure 整合點，包括[對閘道的 Azure AD 驗證](../configure/user-access-control.md#azure-active-directory)。