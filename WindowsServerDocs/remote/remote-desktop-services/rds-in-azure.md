---
title: 使用 ARM 和 Azure Marketplace 完美地部署 RDS
description: 了解如何在 Azure 中建立小型的 RDS 部署使用 ARM 範本和 Azure Marketplace。
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
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823079"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>使用 ARM 和 Azure Marketplace 完美地部署 RDS

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2

遠端桌面服務 (RDS) 是符合成本效益地裝載 Windows 桌面和應用程式選擇的平台。 您可以使用[Azure Marketplace 供應項目](#basic-rds-through-the-azure-marketplace)或是[快速入門範本](#Customized-RDS-using-Quickstart-templates)來快速建立 Azure IaaS 部署中的 RDS。 Azure marketplace 建立您讓簡單且方便的機制，進行測試及概念證明測試網域。 快速入門範本，相反地，可讓您使用現有的網域，使其絕佳的工具來建置實際執行環境。 設定好之後，您可以連接到已發佈的桌面和應用程式從各種平台和裝置，用於 Windows、 Mac、 iOS 和 Android 的 Microsoft 遠端桌面應用程式。

## <a name="basic-rds-through-the-azure-marketplace"></a>透過 Azure Marketplace 的基本 RDS

建立您的部署，透過 Azure Marketplace 是最快的方式來啟動並執行。 完成所有項目之後，您的環境會看起來像[基本 RDS 架構](desktop-hosting-logical-architecture.md#basic-deployment)。 供應項目會建立所有 RDS 元件，您需要-您只需要提供一些資訊。 

您必須提供下列資訊，當您部署的 Marketplace 供應項目：
- 系統管理員使用者名稱和密碼。 這是新的使用者，以管理部署。
- DNS 名稱和 AD 網域名稱。 這些是所建立的新資源。 請確定有意義的名稱。
- VM 大小。 您可以選擇將 RDSH 端點用於 Vm 的大小。 您也以手動方式可以在初始的部署，以協助您最佳化您的工作負載和成本的 Vm 之後變更的大小。

若要從 Azure Marketplace 建立小型 RDS 部署中使用下列步驟： 

1. 啟動 Azure Marketplace RDS 部署：
   1. 登入 [Azure 入口網站](https://portal.azure.com)。
   2. 按一下 **新增**加入您的部署。
   3. 在 [搜尋] 欄位中輸入 「 RDS"，然後按 Enter。
   4. 按一下 **遠端桌面服務 (RDS)-基本-開發/測試**，然後按一下**建立**。
   5. 請依照下列步驟在入口網站中建立及部署 rds。 您要加入索引鍵的設定詳細資料，例如上面所列的資訊。 
2. 連接到您的部署。 部署完成時，請檢查 [輸出] 區段的最後步驟完成，並連接到您的部署。
   1. 下載並執行[此 PowerShell 指令碼](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)來連線至 RDS 部署所需的任何憑證安裝在測試裝置上。 
   
      在測試階段，才需要此步驟。 當您在 Azure 中的 RDS 部署在生產環境中時，請務必遵循最佳作法，如同購買與您的 web 伺服器上使用公開的受信任的 SSL 憑證。

   2. 出現提示時，登入 Azure 帳戶。 選取 Azure 訂用帳戶、 資源群組，以及針對此新的部署建立的公用 IP 位址。
   3. 指令碼完成時，RD 網頁會啟動預設瀏覽器中。 您可以藉由比較您在部署期間提供的 DNS 位址頁面的 URL 請仔細檢查 RD 網頁。 
   
      若要查看預設桌面，為您發佈的部署期間建立的系統管理員認證登入。 您也可以傳送使用者的 RD Web 網站，測試他們的桌上型電腦和應用程式。

      > [!TIP]
      > 別忘了網域名稱或系統管理員使用者嗎？ 您可以移回新的資源群組入口網站中，按一下**部署**，然後檢視您所輸入的參數。

既然您有 RDS 部署，您可以[新增及管理使用者](rds-user-management.md)。

## <a name="customized-rds-using-quickstart-templates"></a>自訂的 RDS 使用快速入門範本

您可以使用 Azure Resource Manager 範本來部署在 Azure 中的 RDS。 如果您想要的基本 RDS 部署，但有 （例如 AD) 您想要使用的現有元件，這會特別有用。 不同於 Marketplace 供應項目中，您可以進行進一步的自訂，例如在虛擬網路上，使用現有的 AD RDSH Vm 和 RDS 元件的高可用性的圖層中使用自訂的作業系統映像。 關於高可用性加入每個元件之後, 您的環境會看起來像[高度可用的 RDS 架構](desktop-hosting-logical-architecture.md#highly-available-deployment)。

若要使用 Azure RDS 範本建立您的小型 RDS 部署中使用下列步驟： 

1. 請挑選您的 Azure 快速入門範本：
   1. 移至[RDS Azure 快速入門範本](https://aka.ms/rdautomation)站台。
   2. 選擇符合您嘗試執行的範本。 請確定您符合該特定範本的任何必要條件。 （比方說，如果您想要使用自訂映像的 Vm，請確定您已為 Azure 儲存體帳戶上傳該映像。）
   3. 按一下 **部署至 Azure**。
   4. 您必須在 Azure 入口網站中提供一些詳細資料 （例如 AD 網域名稱系統管理員使用者名稱）。 這將視您選擇的範本。
   5. 按一下 **採購**。
2. 連接到您的部署。 
   1. 下載並執行[此 PowerShell 指令碼](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)來連線至 RDS 部署所需的任何憑證安裝在測試裝置上。 
   
      在測試階段，才需要此步驟。 當您在 Azure 中的 RDS 部署在生產環境中時，請務必遵循最佳作法，如同購買與您的 web 伺服器上使用公開的受信任的 SSL 憑證。

   2. 出現提示時，登入 Azure 帳戶。 選取 Azure 訂用帳戶、 資源群組，以及針對此新的部署建立的公用 IP 位址。
   3. 指令碼完成時，RD 網頁會啟動預設瀏覽器中。 您可以藉由比較您在部署期間提供的 DNS 位址頁面的 URL 請仔細檢查 RD 網頁。 
   
      若要查看預設桌面，為您發佈的部署期間建立的系統管理員認證登入。 您也可以傳送使用者的 RD Web 網站，測試他們的桌上型電腦和應用程式。

      > [!TIP]
      > 別忘了網域名稱或系統管理員使用者嗎？ 您可以移回新的資源群組入口網站中，按一下**部署**，然後檢視您所輸入的參數。

既然您有 RDS 部署，您可以[新增及管理使用者](rds-user-management.md)。
