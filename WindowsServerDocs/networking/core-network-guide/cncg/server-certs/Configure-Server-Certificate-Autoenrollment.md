---
title: 設定伺服器的憑證自動註冊
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ddf8a905fdb68bbc474b10f526b32f3d8b83af46
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-auto-enrollment"></a>設定自動註冊憑證

>適用於：Windows Server（以每年次管道）、Windows Server 2016

> [!NOTE]
> 執行此程序之前，您必須使用 [憑證範本 Microsoft Management Console 嵌入式管理單元上執行的 AD CS CA 設定伺服器的憑證範本。
同時成員資格**企業系統管理員**並根網域的**網域系統管理員」**群組是才能完成此程序最小值。

## <a name="configure-server-certificate-auto-enrollment"></a>設定伺服器的憑證自動註冊

1. 在電腦上安裝 AD DS 時，請打開 Windows PowerShell&reg;，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 開啟。
2. 在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。
3. 在**可用嵌入式管理單元**、向下捲動並按兩下 [**群組原則編輯器] 管理**。 **選取的群組原則物件**對話方塊。

     > [!IMPORTANT]
     > 確定您選取 [**群組原則編輯器] 管理**並不**群組原則管理**。 如果您選取 [**群組原則管理**，使用這些指示您的設定將會失敗並伺服器的憑證會自動註冊至 NPS 伺服器。

4. 在**群組原則物件**，按一下 [**瀏覽]**。 **群組原則物件的瀏覽]**對話方塊。
5. 在**網域、Ou 和連結的群組原則物件，**按一下**預設網域原則**，然後按一下 [ **[確定]**。
6. 按一下**完成**，然後按**[確定]**。
7. 按兩下**預設網域原則**。 在主控台中，展開下列路徑：**電腦設定**，**原則**，**Windows 設定**、**的安全性設定**，然後**公用原則**。
8. 按一下**公用原則**。 在詳細資料窗格中，按兩下 [**憑證服務 Client 自動註冊**。 **屬性**對話方塊。 設定，下列項目，然後按一下**[確定]**:

     1. 在**設定模型**、**啟用**。
     2. 選取 [**更新過期的憑證，更新擱置中的憑證，並移除撤銷憑證**核取方塊。
     3. 選取 [**更新使用的憑證範本憑證**核取方塊。

9. 按一下**[確定]**。

## <a name="configure-user-certificate-auto-enrollment"></a>設定使用者憑證自動註冊

1. 在電腦上安裝 AD DS 時，請打開 Windows PowerShell&reg;，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 開啟。
2. 在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。
3. 在**可用嵌入式管理單元**、向下捲動並按兩下 [**群組原則編輯器] 管理**。 **選取的群組原則物件**對話方塊。

     > [!IMPORTANT]
     > 確定您選取 [**群組原則編輯器] 管理**並不**群組原則管理**。 如果您選取 [**群組原則管理**，使用這些指示您的設定將會失敗並伺服器的憑證會自動註冊至 NPS 伺服器。

4. 在**群組原則物件**，按一下 [**瀏覽]**。 **群組原則物件的瀏覽]**對話方塊。
5. 在**網域、Ou 和連結的群組原則物件，**按一下**預設網域原則**，然後按一下 [ **[確定]**。
6. 按一下**完成**，然後按**[確定]**。
7. 按兩下**預設網域原則**。 在主控台中，展開下列路徑：**使用者設定**，**原則**，**Windows 設定**、**的安全性設定**，然後**公用原則**。
8. 按一下**公用原則**。 在詳細資料窗格中，按兩下 [**憑證服務 Client 自動註冊**。 **屬性**對話方塊。 設定，下列項目，然後按一下**[確定]**:

     1. 在**設定模型**、**啟用**。
     2. 選取 [**更新過期的憑證，更新擱置中的憑證，並移除撤銷憑證**核取方塊。
     3. 選取 [**更新使用的憑證範本憑證**核取方塊。

9. 按一下**[確定]**。

## <a name="next-steps"></a>後續步驟

[重新整理群組原則](refresh-group-policy.md)