---
title: 遠端桌面服務 - 確保資料儲存安全性
description: 在 RDS 中使用使用者設定檔磁碟 (UPD) 安全地儲存資料的規劃資訊。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 934aab380f9e58f4fe9567921623279a1893af4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860291"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>遠端桌面服務 - 使用 UPD 確保資料儲存安全性

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

在內部部署環境或 Azure 中安全地儲存商務資源、使用者個人化資料和設定。 RD 工作階段主機會使用 AD 驗證，並且讓使用者能夠安全地在個人化的環境中使用他們所需的資源。 

確保使用者無論從何種端點存取遠端資源都能有一致的體驗，是管理 RDS 部署的重要層面。 使用者設定檔磁碟 (UPD) 可讓使用者資料、自訂和應用程式設定均依循單一集合內的使用者。 UPD 是就個別使用者和集合而定的 VHD 檔案，儲存於中央共用位置，且會在使用者登入時掛接到其工作階段 - 在該工作階段的持續期間，會將 UPD 視為本機磁碟機。 

從使用者的觀點來看，UPD 可提供熟悉的體驗 - 他們將其文件儲存在 [文件] 資料夾中 (在看似本機磁碟機的位置上)、如往常般變更其應用程式設定，以及對其 Windows 環境進行任何自訂。 這些資料 (包括登錄區) 全都儲存在 UPD 上，並保存於中央網路共用位置。 使用者必須主動連線至桌面或 RemoteApp，才可使用 UPD。 UPD 只能在集合內漫遊，因為使用者的整個 `C:\Users\<username\>` 目錄 (包括 AppData\Local) 都會儲存在 UPD 上。

您可以使用 [PowerShell Cmdlet](https://technet.microsoft.com/library/jj215443.aspx) 來指定中央共用位置的路徑、每個 UPD 的大小，以及儲存至 UPD 的使用者設定檔所應包含或排除的資料夾。 或者，您可以移至 [遠端桌面服務]   > [集合]   > [桌面集合]   > [桌面集合內容]   > [使用者設定檔磁碟]  ，以透過伺服器管理員來啟用 UPD。 請注意，您會為整個集合的所有使用者啟用或停用 UPD，而不是針對該集合中的特定使用者執行。 UPD 必須儲存在集合中的伺服器具有完整控制權限的中央檔案共用位置。 

您可以透過[儲存空間直接存取](rds-storage-spaces-direct-deployment.md)將 UPD 儲存在 Azure 中，使您的 UPD 達到高可用性。 