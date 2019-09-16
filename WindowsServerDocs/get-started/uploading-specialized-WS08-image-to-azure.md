---
title: 將 Windows Server 2008/2008 R2 專用映像上傳至 Azure
description: Windows Server 2008 與 2008 R2 即將終止服務。 了解如何拿起及移動至 Azure 架設在雲端中的 Windows Server。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 9bd2b17296872d4b94de5a7468178fbb2ba39709
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868402"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>將 Windows Server 2008/2008 R2 專用映像上傳至 Azure 

![介紹 WS08 映像主題的橫幅](media/WS08-image-banner-large.png)

現在您可以透過 Azure 在雲端中執行 Windows Server 2008/2008 R2 VM。 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>準備 Windows Server 2008/2008 R2 專用映像
請先進行以下變更，您才能上傳映像：

- 如果您尚未將 Windows Server 2008 Service Pack 2 (SP2) 安裝在您的映像上，請下載並安裝 SP2。

- 設定遠端桌面 (RDP) 設定。
  1. 移至 [控制台]   >   [系統設定]。   
  2. 選取左側功能表中的 [遠端設定]  。

     ![系統設定螢幕擷取畫面，反白顯示 [遠端設定]。](media/1a_remote_settings.png)

  3. 選取 [系統內容] 中的 [遠端]  索引標籤。   

     ![系統內容中遠端索引標籤的螢幕擷取畫面。](media/2c_sysprops.png)

  4. 選取 [允許來自執行任何版本之遠端桌面的電腦進行連線 (較不安全)]。   
  5. 依序按一下 [套用]  、[確定]  。
- 設定 Windows 防火牆設定。   
   1. 在系統管理員模式的命令提示字元處，輸入 “**wf.msc**” 以進入 Windows 防火牆與進階安全性設定。   
   2. 依 [連接埠]  排序發現的項目，然後選取 [連接埠 3389]  。   
     ![WIndows 防火牆設定輸入規則螢幕擷取畫面。](media/3b_inboundrules.png)   
   3. 為設定檔啟用遠端桌面 (TCP IN)：**網域**、**私人**及**公用** (如上所示)。

- 儲存所有設定，並關閉映像。   
- 如果您使用的是 Hyper-V，請確定子系 AVHD 已合併至父系 VHD 以保存變更。

目前的已知錯誤會造成所上傳映像的系統管理員密碼在 24 小時內到期。 請依照下列步驟避免此問題： 

1. 移至 \[開始\]   >   \[執行\]
2. 輸入 **lusrmgr.msc**
3. 選取 \[本機使用者和群組\] 下方的 \[使用者\] 
4. 以滑鼠右鍵按一下 \[系統管理員\]  ，然後選取 \[內容\] 
5. 選取 \[密碼永久有效\]  ，然後選取 \[確定\]  
![系統管理員內容螢幕擷取畫面](media/6_adminprops.png)。

## <a name="uploading-the-image-vhd"></a>上傳映像 VHD
您可以使用下方的指令碼上傳 VHD。 執行這項作業之前，您必須發佈 Azure 帳戶的設定檔案。 取得您的 [Azure 檔案設定](https://azure.microsoft.com/downloads/)。

以下是指令碼：

```powershell
Get-AzurePublishSettingsFile 

Login-AzureRmAccount
 
      # Import publishsettings
      Import-AzurePublishSettingsFile -PublishSettingsFile <LocationOfPublishingFile>
      $subscriptionId = 'xxxx-xxxx-xxxx-xxxx-xxxxx'
 
      # Set NodeFlight subscription as default subscription
      Select-AzureRmSubscription -SubscriptionId $subscriptionId
      Set-AzureRmContext -SubscriptionId $subscriptionId
      $rgName = "<resourcegroupname>"
    
      $urlOfUploadedImageVhd = "<BlobUrl>/<NameForVHD>.vhd"
      Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "<FilePath>"  
```
## <a name="deploy-the-image-in-azure"></a>在 Azure 中部署映像
在此區段中，您會將映像 VHD 部署在 Azure 中。 

> [!IMPORTANT]
> 請勿在 Azure 中使用預先定義的使用者映像。

1.  建立新的[資源群組](https://docs.microsoft.com/rest/api/resources/resourcegroups/createorupdate) (英文)。 
2.  在資源群組內部建立新的[儲存體 Blob](https://docs.microsoft.com/rest/api/storageservices/put-blob)(英文)。
3.  在儲存體 Blob 內部建立[容器](https://docs.microsoft.com/rest/api/storageservices/create-container) (英文)。
4.  複製內容中 Blob 儲存體的 URL。
5.  使用於上方提供的指令碼將映像上傳至新的儲存體 Blob。
6.  為您的 VHD 建立[磁碟](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。   
     a. 移至 \[磁碟\]，按一下 \[新增\]  。  
     b. 輸入磁碟的名稱。 選取您要使用的訂用帳戶、設定區域，然後選擇帳戶類型。   
     c. 對於 \[來源類型\]，選取儲存體。 瀏覽至使用指令碼建立的 Blob VHD 位置。  
     d. 選取 Windows 作業系統類型和 \[大小\] (預設：1023)。   
     e. 按一下 \[建立\]  。   

7.  移至 \[已建立磁碟\]，按一下 \[建立 VM\]  。   
     a. 命名 VM。   
     b. 選取您在步驟 5 (在此您已上傳磁碟) 中建立的現有群組。   
     c. 為您的 VM 選擇大小與 SKU 計劃。   
     d. 在設定頁面上選取網路介面。 確定網路介面的下列規則已指定：
 
        PORT:3389 Protocol: TCP Action: Allow Priority: 1000 Name: ‘RDP-Rule'.   
     e. 按一下 \[建立\]  。




