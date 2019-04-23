---
title: 遠端桌面服務-保護資料存放區
description: 規劃資訊安全地將資料儲存在 RDS 使用使用者設定檔磁碟 (Upd)
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 242d09e49141e9fb789a83421714366105730cc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870589"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>遠端桌面服務-使用 Upd 的安全資料儲存體

>適用於：Windows Server （半年通道），Windows Server 2016

Store 商務資源、 使用者的個人化資料和設定安全地在內部部署或 Azure 中。 RD 工作階段主機會使用 AD 驗證，並讓使用者使用他們所需的資源在個人化的環境中，安全地能力。 

確保使用者有一致的體驗，不論它們從中存取遠端資源，是很重要的層面管理 RDS 部署的端點。 使用者資料、 自訂和應用程式設定來遵循單一集合內的使用者，可讓使用者設定檔磁碟 (Upd)。 UPD 是每位使用者，每個集合的 VHD 檔案儲存在中央共用掛接到使用者工作階段時在登入時，因為 UPD 當做本機的磁碟機來處理該工作階段的持續期間。 

從使用者的觀點來看，UPD 提供 famililar 體驗-這些儲存其文件資料夾 （在功能上似乎是本機的磁碟機），變更其文件其應用程式設定如往常般，並讓 Windows 環境的任何自訂項目。 所有這些資料，包括登錄區中，儲存在 UPD 上，並保存在中央網路共用。 使用者主動連線到桌上型電腦或 RemoteApp 時，才可供使用者 Upd。 因為使用者的整個 C:\Users Upd 僅可在集合內漫遊&#92;\<使用者名稱\>目錄 （包括 AppData\Local） 會儲存在 UPD 上。

您可以使用[PowerShell cmdlet](https://technet.microsoft.com/library/jj215443.aspx)指定中央共用的每個 UPD，大小，應包含或排除的使用者設定檔儲存在 UPD 資料夾的路徑。 或者，您可以啟用 Upd 透過 伺服器管理員，方法是前往**遠端桌面服務 > 集合 > 桌面集合 > 桌面集合內容 > 使用者設定檔磁碟**。 請注意您啟用或停用 Upd，整個集合的所有使用者，而不適用於該集合中的特定使用者。 Upd 必須儲存在中央檔案共用所在的伺服器集合中具有完全控制權限。 

您也可以將它們儲存在 Azure 中使用您的 Upd 的達到高可用性[儲存空間直接存取](rds-storage-spaces-direct-deployment.md)。 