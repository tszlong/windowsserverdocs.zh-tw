---
title: 使用 ARM 和 Azure Marketplace 順暢地部署 RDS
description: 了解如何使用 ARM 範本和 Azure Marketplace 在 Azure 中建立小型 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: 218e61e5ebe110502ebe139b27607bfeff104fde
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712619"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>使用 ARM 和 Azure Marketplace 順暢地部署 RDS

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

遠端桌面服務 (RDS) 是一個可供選擇的平台，能夠符合成本效益地託管 Windows 桌面和應用程式。 您可以使用 [Azure Marketplace 供應項目](#basic-rds-through-the-azure-marketplace)或[快速入門範本](#customized-rds-using-quickstart-templates)在 Azure IaaS 部署中快速建立 RDS。 Azure Marketplace 可為您建立測試網域，使其成為測試概念證明的簡便機制。 另一方面，快速入門範本則可讓您使用現有的網域，以其作為建置生產環境的絕佳工具。 設定好之後，您可以使用適用於 Windows、Mac、iOS 和 Android 的 Microsoft 遠端桌面應用程式，從各種平台和裝置連線至已發佈的桌面和應用程式。

## <a name="basic-rds-through-the-azure-marketplace"></a>透過 Azure Marketplace 取得的基本 RDS

透過 Azure Marketplace 建立您的部署，是最快速的啟動和執行方式。 一切都完成之後，您的環境看起來會像是[基本 RDS 架構](desktop-hosting-logical-architecture.md#basic-deployment)。 供應項目會建立您所需的所有 RDS 元件 - 您只需要提供一些資訊即可。 

您在部署 Marketplace 供應項目時必須提供下列資訊：
- 系統管理員的使用者名稱和密碼。 這是將會管理部署的新使用者。
- DNS 名稱和 AD 網域名稱。 這些是已建立的新資源。 請確定這些名稱具有意義。
- VM 大小。 您可以選擇要用於 RDSH 端點的 VM 大小。 您也可以在初始部署之後手動變更大小，以便在工作負載和成本方面將 VM 最佳化。

請使用下列步驟，從 Azure Marketplace 建立您的小型 RDS 部署： 

1. 啟動 Azure Marketplace RDS 部署：
   1. 登入 [Azure 入口網站](https://portal.azure.com)。
   2. 按一下 [新增]  以新增您的部署。
   3. 在搜尋欄位中輸入 "RDS"，然後按 Enter 鍵。
   4. 按一下 [遠端桌面服務 (RDS) - 基本 - 開發/測試]  ，然後按一下 [建立]  。
   5. 依照下列步驟在入口網站中建立及部署 RDS。 您將新增主要設定詳細資料，如上方所列的資訊。 
2. 連線至您的部署。 部署完成時，請在輸出區段中查看須完成的最終步驟，並連線至您的部署。
   1. 在測試裝置下載並執行[此 PowerShell 指令碼](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)，以安裝連線至 RDS 部署所需的任何憑證。 
   
      在測試階段才需要執行此步驟。 在生產環境中將 RDS 部署至 Azure 時，請務必遵循最佳做法，例如購買公開信任的 SSL 憑證，以及在您的 Web 伺服器上使用這些憑證。

   2. 在出現提示時，登入您的 Azure 帳戶。 選取為這個新部署建立的 Azure 訂用帳戶、資源群組和公用 IP 位址。
   3. 指令碼完成時，會在您的預設瀏覽器中啟動 RD 網頁。 您可以比較此頁面的 URL 與您在部署期間提供的 DNS 位址，以詳細檢查 RD 網頁。 
   
      使用您在部署期間建立的系統管理員認證登入，以查看系統為您發佈的預設桌面。 您也可以將 RD 網站傳送給使用者，以測試其桌面和應用程式。

      > [!TIP]
      > 忘記網域名稱或系統管理員使用者了嗎？ 您可以回到入口網站中的新資源群組，並按一下 [部署]  ，然後檢視您已輸入的參數。

現在您已有 RDS 部署，接下來可以[新增及管理使用者](rds-user-management.md)。

## <a name="customized-rds-using-quickstart-templates"></a>使用快速入門範本的自訂 RDS

您可以使用 Azure Resource Manager 範本在 Azure 中部署 RDS。 如果您需要基本 RDS 部署，但有現有的元件 (例如 AD) 可供使用，此作法會特別有用。 不同於 Marketplace 供應項目，您可以進行進一步的自訂，例如在虛擬網路上使用現有的 AD、將自訂作業系統映像用於 RDSH VM，以及將 RDS 元件設置於高可用性層級上。 為每個元件附加高可用性之後，您的環境看起來會像是[高可用性的 RDS 架構](desktop-hosting-logical-architecture.md#highly-available-deployment)。

請使用下列步驟，使用 Azure RDS 範本建立您的小型 RDS 部署： 

1. 選擇您的 Azure 快速入門範本：
   1. 移至 [RDS Azure 快速入門範本](https://aka.ms/rdautomation)網站。
   2. 根據您嘗試要執行的作業選擇適合的範本。 請確定您符合該範本的任何必要條件。 (例如，如果您想要將自訂映像用於您的 VM，請確定您已將該映像上傳至 Azure 儲存體帳戶。)
   3. 按一下 [部署至 Azure]  。
   4. 您必須在 Azure 入口網站中提供一些詳細資料 (例如系統管理員使用者名稱、AD 網域名稱)。 這些資料會根據您選擇的範本而不同。
   5. 按一下 [購買]  。
2. 連線至您的部署。 
   1. 在測試裝置下載並執行[此 PowerShell 指令碼](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)，以安裝連線至 RDS 部署所需的任何憑證。 
   
      在測試階段才需要執行此步驟。 在生產環境中將 RDS 部署至 Azure 時，請務必遵循最佳做法，例如購買公開信任的 SSL 憑證，以及在您的 Web 伺服器上使用這些憑證。

   2. 在出現提示時，登入您的 Azure 帳戶。 選取為這個新部署建立的 Azure 訂用帳戶、資源群組和公用 IP 位址。
   3. 指令碼完成時，會在您的預設瀏覽器中啟動 RD 網頁。 您可以比較此頁面的 URL 與您在部署期間提供的 DNS 位址，以詳細檢查 RD 網頁。 
   
      使用您在部署期間建立的系統管理員認證登入，以查看系統為您發佈的預設桌面。 您也可以將 RD 網站傳送給使用者，以測試其桌面和應用程式。

      > [!TIP]
      > 忘記網域名稱或系統管理員使用者了嗎？ 您可以回到入口網站中的新資源群組，並按一下 [部署]  ，然後檢視您已輸入的參數。

現在您已有 RDS 部署，接下來可以[新增及管理使用者](rds-user-management.md)。
