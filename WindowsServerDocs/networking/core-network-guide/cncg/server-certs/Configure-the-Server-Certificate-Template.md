---
title: 設定伺服器憑證範本
description: 本主題是適用于 802.1 X 有線和無線部署的指南部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 4eaeef21e90a3a0a56c6c136363e102948c31b97
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950194"
---
# <a name="configure-the-server-certificate-template"></a>設定伺服器憑證範本

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來設定 Active Directory &reg; 憑證服務 (AD CS) 使用的憑證範本，做為向您網路上的伺服器註冊之伺服器憑證的基礎。

設定此範本時，您可以透過 Active Directory 群組指定伺服器，該伺服器應該會自動從 AD CS 接收伺服器憑證。

下列套裝程式含設定範本以將憑證發行至下列所有伺服器類型的指示：

- 執行遠端存取服務的伺服器，包括 ras **和 資訊存取伺服器** 群組的成員，包括 Ras 閘道伺服器。
- 執行網路原則伺服器 (NPS) 服務的伺服器，其為 **RAS 和 資訊存取伺服器** 群組的成員。

若要完成此程式，至少需要 **Enterprise Admins** 和 Root 網域 **domain Admins** 群組的成員資格。

### <a name="to-configure-the-certificate-template"></a>設定憑證範本

1.  在 CA1 的 [伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **憑證授權單位** 單位]。  (MMC) 的憑證授權單位單位 Microsoft Management Console 隨即開啟。

2.  在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下 [ **憑證範本**]，然後按一下 [ **管理**]。

3.  [憑證範本] 主控台隨即開啟。 所有的憑證範本都會顯示在詳細資料窗格中。

4.  在詳細資料窗格中，按一下 [ **RAS 和 資訊存取伺服器** ] 範本。

5.  按一下 [ **動作** ] 功能表，然後按一下 [ **複製範本**]。 [範本 **屬性** ] 對話方塊隨即開啟。

6.  按一下 [安全性]  索引標籤。

7.  在 [ **安全性** ] 索引標籤的 [ **群組或使用者名稱**] 中，按一下 [ **RAS 和 資訊存取伺服器**]。

8.  在 [ **RAS 和 資訊存取伺服器的許可權**] 的 [ **允許**] 底下，確定已選取 [ **註冊** ]，然後選取 [ **自動註冊** ] 核取方塊。 按一下 **[確定]**，然後關閉 [憑證範本] MMC。

9.  在 [憑證授權單位單位] MMC 中，按一下 [ **憑證範本**]。 在 [ **動作** ] 功能表上，指向 [ **新增**]，然後按一下 [ **要發出的憑證範本**]。 [ **啟用憑證範本** ] 對話方塊隨即開啟。

10. 在 [ **啟用憑證範本**] 中，按一下您剛剛設定的憑證範本名稱，然後按一下 **[確定]**。 例如，如果您沒有變更預設憑證範本名稱，請按一下 [ **RAS 和 資訊存取伺服器的複本**]，然後按一下 **[確定]**。



