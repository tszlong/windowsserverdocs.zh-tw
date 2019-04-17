---
title: 使用 Azure 備份備份您的 Windows 伺服器，從 Windows Admin Center
description: 使用 Windows Admin Center (Project Honolulu) 來備份 Windows 伺服器與 Azure 備份
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296909"
---
# 使用 Azure 備份備份您的 Windows 伺服器，從 Windows Admin Center

>適用於： Windows Admin Center 預覽版，Windows Admin Center

[深入了解 Azure 與 Windows Admin Center 整合。](../plan/azure-integration-options.md)

Windows Admin Center 簡化至 Azure 備份您的 Windows 伺服器和保護您免於意外或惡意的刪除、 損毀或甚至是勒索軟體的程序。 若要將安裝工作自動化，您可以將 Windows Admin Center 閘道連線至 Azure。

使用下列資訊設定為您的 Windows Server 的備份，並建立備份原則以備份您的伺服器磁碟區和 Windows 系統狀態從 Windows Admin Center。

## 什麼是 Azure 備份，以及它如何搭配使用 Windows Admin Center？ 

**Azure 備份**是 Azure 型的服務，您可以使用備份 （或保護） 及還原您的 Microsoft cloud 的資料。 Azure 備份取代為您的現有的內部或離備份解決方案是可靠、 安全且成本具競爭力的雲端式解決方案。
[深入了解 Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)。

Azure 備份提供多個元件的下載，並在適當的電腦上部署伺服器，或在雲端中。 元件或代理程式，您將部署取決於您想要保護。 所有的 Azure 備份元件 （不論您是否正在保護的資料內部部署或 Azure 中） 可用來在 Azure 中復原服務保存庫備份資料。

整合的 Windows Admin Center 中的 Azure 備份十分適用於備份磁碟區並將 Windows 系統狀態與內部 Windows 實體或虛擬伺服器。 這會讓的備份檔案伺服器、 網域控制站和 IIS 網頁伺服器的完整機制。

Windows Admin Center 會公開 Azure 備份整合，透過原生**備份**工具。 **備份**工具提供的安裝程式、 管理及監視體驗，以快速開始備份您的伺服器，執行常見的備份和還原作業，並監視您的 Windows 伺服器的整體的備份健康情況。

## 先決條件和規劃

- 搭配至少一個使用中訂閱 Azure 帳戶
- 目標為 Windows 伺服器您想要備份必須有網際網路存取 Azure
- [將 Windows Admin Center 閘道連線至 Azure](azure-integration.md)

若要開始備份您的 Windows Server 的工作流程，開啟伺服器連線、 按一下**備份**的工具，按照如下所述的步驟。

## 設定 Azure 備份
當您按一下所在 Azure 備份尚未啟用伺服器連線的**備份**工具時，您會看到**至 Azure 備份歡迎**畫面。 按一下 [**設定 Azure 備份**的按鈕。 這會開啟 Azure 備份安裝精靈。 遵循以下列出備份您的伺服器精靈中的步驟。

如果已設定 Azure 備份，按一下**備份**工具將會開啟**備份儀表板**。 請參閱 （[管理和監視](#management-and-monitoring)） 章節來探索作業，以及可以在儀表板中執行的工作。

### 步驟 1： 登入 Microsoft azure
登入您的 Azure 帳戶。 

> [!NOTE]
> 如果您已連線至 Azure 將 Windows Admin Center 閘道，您應該會自動登入到 Azure。 您可以按一下**登出時**進一步登入不同的使用者身分。

### 步驟 2： 設定 Azure 備份
選取適當的 Azure 備份設定，如下所述

 - **訂閱識別碼：** 您想要使用 azure 備份您的 Windows Server 的 Azure 訂閱。 像 Azure 資源群組的所有 Azure 資產，將選取的訂閱中建立復原服務保存庫 \。
 - **保存庫：** 復原服務保存庫，將會儲存您的伺服器備份的位置。 您可以從現有的保存庫選取或 Windows Admin Center 將會建立新的保存庫。  
 - **資源群組：** Azure 資源群組是一組資源的容器。 復原服務保存庫建立或包含在指定的資源群組中。 您可以從現有的資源群組選取或 Windows Admin Center 將會建立一個新。
 - **位置：** 將會建立復原服務保存庫 \ Azure 區域。 建議您選取的 Windows Server 最接近的 Azure 區域。

### 步驟 3： 選取備份的項目與排程

- 選取您要從您的伺服器備份。 Windows Admin Center 可讓您選擇從同時讓您選取的資料的預估的大小的備份**磁碟區**與**Windows 系統狀態**的組合。

> [!NOTE]
> 第一個備份是完整備份的所有選取的資料。 不過，後續的備份是增量在本質上，且資料傳輸只所做的變更，因為先前的備份。

- 選取多個預先設定**備份排程**從為您系統狀態和/或磁碟區。

### 步驟 4： 輸入加密複雜密碼

- 輸入您選擇 （最小 16 個字元） 的**加密複雜密碼**。  **Azure 備份**保護備份資料與使用者設定和使用者管理加密複雜密碼。 加密複雜密碼，才能從 Azure 備份復原資料。

> [!NOTE]
> 複雜密碼必須儲存在離線位置，例如另一部伺服器或[Azure 金鑰保存庫](https://docs.microsoft.com/azure/key-vault/quick-create-portal)中。 Microsoft 不會儲存複雜密碼，無法擷取或重設複雜密碼，如果遺失或遺失。

- 檢視所有設定，並按一下 [**套用]**

Windows Admin Center 接著執行下列作業

1. 如果它不存在已經，建立 Azure 資源群組
2. 建立指定 Azure 復原服務保存庫
3. 安裝並註冊於保存庫 Microsoft Azure 復原服務代理程式
4. 建立的備份和保留的排程，根據選取的選項，並將它們與 Windows Server 的關聯。

## 管理及監視工具

一旦您已成功完成安裝 Azure 備份，您會看到**備份儀表板**，當您開啟現有的伺服器連線的備份工具。 您可以從**備份儀表板**中執行下列工作

- **存取 Azure 中的保存庫：** 您可以按一下來將您引導至 Azure 中保存庫以執行一[組豐富的管理作業](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)**備份儀表板**[**概觀**] 索引標籤中的 [**復原服務保存庫 \** ] 連結
- **執行臨機操作的備份：** 按一下 [**立即備份**採取臨機操作的備份。 
- **監視工作和設定警示通知：** 瀏覽到**工作**] 索引標籤，在儀表板來監視持續或過去的工作，以及[設定警示通知](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts)收到電子郵件適用於任何失敗工作或其他備份相關的警示。
- **檢視復原點和復原資料：** 按一下 [檢視復原點，然後按一下 [**復原資料**的步驟來復原您的資料從 Azure 上儀表板中的**復原點**] 索引標籤上。