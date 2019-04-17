---
title: 設定 Azure 整合
description: 設定 Azure 整合 Windows Admin Center (Project Honolulu)。 連線至 Azure 將 Windows Admin Center 閘道。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296918"
---
# 設定 Azure 整合

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 支援與 Azure 服務整合的數個選擇性功能。 [深入了解使用 Windows Admin Center 可用 Azure 整合選項。](../plan/azure-integration-options.md)

若要允許 Windows Admin Center 閘道來運用 Azure Active Directory 驗證，針對閘道的存取權，或建立 Azure 資源 （例如，若要保護使用 Azure 站台在 Windows Admin Center 中管理 Vm 代表您的 Azure 與通訊修復），您必須先向 Azure 註冊您的 Windows Admin Center 閘道。 您只需要執行此動作，一旦您的 Windows Admin Center 閘道-設定會保留，當您更新您的閘道至較新版本。

## 向 Azure 註冊您的閘道

您第一次您嘗試使用 Azure 整合功能在 Windows Admin Center，系統將提示您註冊至 Azure 閘道。 您也可以註冊閘道移至 Windows Admin Center 設定中的 [ **Azure** ] 索引標籤。

引導式的產品中的步驟將會在您的目錄，可讓 Windows Admin Center 通訊與 Azure 建立 Azure AD 應用程式。 若要檢視會自動建立的 Azure AD 應用程式，請移至 Windows Admin Center 設定 [ **Azure** ] 索引標籤。 **在 Azure 中的檢視**超連結可讓您檢視 Azure 入口網站中的 Azure AD 應用程式。 

建立的 Azure AD 應用程式適用於 Azure 整合 Windows Admin Center，包括[Azure AD 驗證閘道](../configure/user-access-control.md#azure-active-directory)中的所有點。