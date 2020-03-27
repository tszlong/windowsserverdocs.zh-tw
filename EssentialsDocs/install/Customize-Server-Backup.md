---
title: 自訂伺服器備份
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7224ba49ecbf880dad8d88a2b197cc279e20d27
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311928"
---
# <a name="customize-server-backup"></a>自訂伺服器備份

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>根據預設，會關閉 Server Backup  
 您可以選擇根據預設關閉 Server Backup。 您需要將 **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** 的值設定為 1，以便啟用此選項。  
  
 設定此機碼時，不會透過儀表板或啟動列公開 Server Backup 使用者介面。 這可讓您利用 Server Backup 的協力廠商應用程式。  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>新增 ServerBackup\ProviderDisabled？登錄機碼，並將值設為1  
  
1.  在伺服器上，請依序按一下 **[開始]** 及 **[執行]** ，並在 **[開啟]** 文字方塊中輸入 **regedit**，然後按一下 **[確定]** 。  
  
2.  在瀏覽窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server** 及 **ServerBackup**。  
  
3.  在 **ServerBackup** 上按一下滑鼠右鍵，按一下 **[新增]** ，然後按一下 **[DWARD 值]** 。  
  
4.  名稱輸入 **ProviderDisabled**。  
  
5.  以滑鼠右鍵按一下名稱，選取 [修改]，值資料輸入 **1**，然後按一下 [確定]。  
  
## <a name="turn-on-server-backup"></a>開啟 Server Backup  
 您可以將建立 **ProviderDisabled** 登錄機碼時關閉的伺服器備份開啟 (如本文件之前所述)。  
  
 您需要刪除 **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** 機碼，以便啟用預設的伺服器備份，請變更 Windows Server Server Backup Service 的服務啟動類型，然後重新啟動伺服器。  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>要刪除 ServerBackup\ProviderDisabled 嗎？登錄機碼  
  
1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]** 。  
  
2.  在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit] 應用程式。  
  
3.  在瀏覽窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server** 及 **ServerBackup**。  
  
4.  在 **[ProviderDisabled]** 上按一下滑鼠右鍵，然後按一下 **[刪除]** 。  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>變更 Windows Server Server Backup Service 的啟動類型  
  
1.  在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]** 。  
  
2.  在 [搜尋] 方塊中，輸入 **services.msc**，然後按一下 **Services** 應用程式。  
  
3.  在服務窗格中，在 **[Windows Server Server Backup Service]** 上按一下滑鼠右鍵，然後按一下 **[內容]** 。  
  
4.  在 **[一般]** 索引標籤中，選取 **[自動]** 作為 **[啟動類型]** 。  
  
5.  按一下 **[確定]** 關閉對話方塊。  
  
#### <a name="restart-the-server"></a>重新啟動伺服器  
  
1.  在伺服器上，將滑鼠移至畫面的右上角，按一下 **[設定]** ，然後按一下 **[電源]** ，然後按一下 [重新啟動]。