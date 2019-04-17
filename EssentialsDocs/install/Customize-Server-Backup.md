---
title: "自訂伺服器備份"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>自訂伺服器備份

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

## <a name="turn-off-server-backup-by-default"></a>關閉預設伺服器備份  
 您可以選擇預設關閉伺服器備份。 您需要將值設定**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** 1 以讓此選項。  
  
 此設定時，不會透過儀表板或 Launchpad 公開伺服器備份使用者介面。 這可讓您使用協力廠商應用程式的伺服器備份。  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>若要新增 ServerBackup\ProviderDisabled 嗎？登錄檔案與設定的值為 1  
  
1.  在伺服器上，按一下 [ **[開始]**，按一下 [**執行**，輸入**regedit**在**開放**] 文字方塊中，然後按一下 [ **[確定]**。  
  
2.  在瀏覽窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**，然後展開**ServerBackup**。  
  
3.  以滑鼠右鍵按一下**ServerBackup**，按一下 [**新**，然後按一下 [ **DWARD 值**。  
  
4.  名稱，輸入**ProviderDisabled**。  
  
5.  以滑鼠右鍵按一下名稱，請選取 [**修改**，輸入**1**的數值資料，然後再按一下**[確定]**。  
  
## <a name="turn-on-server-backup"></a>打開伺服器備份  
 如果這建立已關閉，您可以關閉伺服器備份上**ProviderDisabled**（如之前所述本文件）登錄金鑰。  
  
 您需要 delete 按鍵**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled**為了讓預設伺服器備份變更 [開始] 畫面的服務類型 Windows Server 伺服器備份服務，伺服器重新開機。  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>若要 delete ServerBackup\ProviderDisabled 嗎？登錄鍵  
  
1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
2.  在搜尋方塊中，輸入**regedit**，然後按**Regedit**應用程式。  
  
3.  在瀏覽窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**，然後展開**ServerBackup**。  
  
4.  以滑鼠右鍵按一下**ProviderDisabled**，然後按**Delete**。  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>變更 [開始] 畫面的 Windows Server 伺服器備份服務類型  
  
1.  在伺服器上，將滑鼠移到畫面的右上角，然後按一下**搜尋**。  
  
2.  在搜尋方塊中，輸入**services.msc**，然後按**服務**應用程式。  
  
3.  在 [服務] 窗格中，以滑鼠右鍵按一下**Windows Server 伺服器備份服務**，按一下 [**屬性**。  
  
4.  在**一般**索引標籤，選取**自動**的**開機輸入**。  
  
5.  按一下**[確定]**來關閉對話方塊。  
  
#### <a name="restart-the-server"></a>重新開機伺服器  
  
1.  在伺服器上，將滑鼠移到畫面的右下角按**設定**，按一下 [**電源**，然後按一下 [重新開機。