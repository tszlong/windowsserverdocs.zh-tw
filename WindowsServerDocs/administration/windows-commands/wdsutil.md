---
title: wdsutil
description: Wdsutil 的參考文章，這是用來管理 Windows 部署服務伺服器的命令列公用程式。
ms.topic: reference
ms.assetid: 3a1965a0-8677-40cc-9495-30ae806808d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7516eea3d6edd013b3f507671e6b219da7dfd632
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718365"
---
# <a name="wdsutil"></a>wdsutil

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

wdsutil 是用來管理 Windows 部署服務伺服器的命令列公用程式。 若要執行這些命令，請按一下 [ **開始**]，以滑鼠右鍵按一下 **命令提示**字元，然後按一下 [以 **系統管理員身分執行**]

## <a name="commands"></a>命令

|命令|描述|
|------|--------|
|[wdsutil add 命令](wdsutil-add.md)|新增物件或將預先設置電腦。|
|[wdsutil 核准-autoadddevices 命令](wdsutil-approve-autoadddevices.md)|核准等待系統管理員核准的電腦。|
|[wdsutil convert-riprepimage 命令](wdsutil-convert-riprepimage.md)|將現有的遠端安裝準備 (RIPrep) 映射轉換成 Windows 映像 ( .wim) 檔。|
|[wdsutil 複製命令](wdsutil-copy.md)|複製映射或驅動程式群組。|
|[wdsutil delete-autoadddevices 命令](wdsutil-delete-autoadddevices.md)|刪除自動新增資料庫 (中的電腦，該電腦會儲存伺服器) 上電腦的相關資訊。|
|[wdsutil disable 命令](wdsutil-disable.md)|停用 Windows 部署服務的所有服務。|
|[wdsutil 中斷連線-用戶端命令](wdsutil-disconnect-client.md)|中斷用戶端與多播傳輸或命名空間的連線。|
|[wdsutil enable 命令](wdsutil-enable.md)|啟用 Windows 部署服務的所有服務。|
|[wdsutil 匯出-image 命令](wdsutil-export-image.md)|從映射存放區將映射匯出到 .wim 檔案。|
|[wdsutil get 命令](wdsutil-get.md)|抓取指定物件的屬性和屬性。|
|[wdsutil initialize-server 命令](wdsutil-initialize-server.md)|設定 Windows 部署服務伺服器以供初次使用。|
|[wdsutil new 命令](wdsutil-new.md)|建立新的捕獲和探索映射，以及多播傳輸和命名空間。|
|[wdsutil 進度命令](wdsutil-progress.md)|當執行命令時，顯示進度狀態。|
|[wdsutil 拒絕-autoadddevices 命令](wdsutil-reject-autoadddevices.md)|拒絕正在等待系統管理員核准的電腦。|
|[wdsutil remove 命令](wdsutil-remove.md)|移除物件。|
|[wdsutil 取代-image 命令](wdsutil-replace-image.md)|以該映射的新版本取代開機或安裝映射。|
|[wdsutil set 命令](wdsutil-set.md)|在指定的物件上設定屬性和屬性。|
|[wdsutil 啟動伺服器命令](wdsutil-start-server.md)|啟動 Windows 部署服務伺服器上的所有服務，包括多播傳輸、命名空間和傳輸伺服器。|
|[wdsutil stop server 命令](wdsutil-stop-server.md)|停止 Windows 部署服務伺服器上的所有服務。|
|[wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)|還原伺服器初始化期間所做的變更。|
|[wdsutil update-serverfiles 命令](wdsutil-update-serverfiles.md)|更新 remoteInstall 共用上的伺服器檔案。|
|[wdsutil 詳細資訊命令](wdsutil-verbose.md)|顯示指定命令的詳細資訊輸出。|
