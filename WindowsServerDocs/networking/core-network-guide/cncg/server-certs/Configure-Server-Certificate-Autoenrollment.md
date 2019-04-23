---
title: 設定伺服器憑證自動註冊
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea7b7efe01525f4ecfb35200463c3f221d92d62d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888559"
---
# <a name="configure-certificate-auto-enrollment"></a>設定憑證自動註冊

>適用於：Windows Server （半年通道），Windows Server 2016

> [!NOTE]
> 執行此程序之前，您必須執行 AD CS CA 上使用 憑證範本 Microsoft Management Console 嵌入式管理單元來設定伺服器憑證範本。
中的成員資格**Enterprise Admins**與根網域**Domain Admins**群組是要完成此程序，至少需要。

## <a name="configure-server-certificate-auto-enrollment"></a>設定伺服器憑證自動註冊

1. 在電腦上安裝 AD DS 時，開啟 Windows PowerShell&reg;，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 便會隨即開啟。
2. 按一下 **[檔案]** 功能表上的 **[新增/移除嵌入式管理單元]**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。
3. 在 **可用的嵌入式管理單元**，向下捲動並按兩下**群組原則管理編輯器**。 **選取 群組原則物件**對話方塊隨即開啟。

     > [!IMPORTANT]
     > 請確定您選取**群組原則管理編輯器**而非**群組原則管理**。 如果您選取**群組原則管理**您使用這些指示的設定將會失敗，伺服器憑證將不會自動註冊到您 NPSs。

4. 在 **群組原則物件**，按一下**瀏覽**。 **瀏覽群組原則物件**對話方塊隨即開啟。
5. 在 **網域、 Ou 和連結的群組原則物件**按一下 **預設網域原則**，然後按一下  **確定**。
6. 按一下 [完成] ，然後按一下 [確定] 。
7. 按兩下**預設網域原則**。 在主控台中，展開下列路徑：**電腦設定**，**原則**， **Windows 設定**，**安全性設定**，然後**公開金鑰原則**.
8. 按一下 **公開金鑰原則**。 在詳細資料窗格中，按兩下 **[憑證服務用戶端 - 自動註冊]**。 **屬性**對話方塊隨即開啟。 設定下列項目，然後再按一下**確定**:

     1. 在 [設定模型] 中，選取 [啟用]。
     2. 選取 **更新過期的憑證，更新擱置中的憑證並移除撤銷的憑證**核取方塊。
     3. 選取 [**更新使用憑證範本的憑證**] 核取方塊。

9. 按一下 [確定] 。

## <a name="configure-user-certificate-auto-enrollment"></a>設定使用者憑證自動註冊

1. 在電腦上安裝 AD DS 時，開啟 Windows PowerShell&reg;，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 便會隨即開啟。
2. 按一下 **[檔案]** 功能表上的 **[新增/移除嵌入式管理單元]**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。
3. 在 **可用的嵌入式管理單元**，向下捲動並按兩下**群組原則管理編輯器**。 **選取 群組原則物件**對話方塊隨即開啟。

     > [!IMPORTANT]
     > 請確定您選取**群組原則管理編輯器**而非**群組原則管理**。 如果您選取**群組原則管理**您使用這些指示的設定將會失敗，伺服器憑證將不會自動註冊到您 NPSs。

4. 在 **群組原則物件**，按一下**瀏覽**。 **瀏覽群組原則物件**對話方塊隨即開啟。
5. 在 **網域、 Ou 和連結的群組原則物件**按一下 **預設網域原則**，然後按一下  **確定**。
6. 按一下 [完成] ，然後按一下 [確定] 。
7. 按兩下**預設網域原則**。 在主控台中，展開下列路徑：**使用者設定**，**原則**， **Windows 設定**，**安全性設定**。
8. 按一下 **公開金鑰原則**。 在詳細資料窗格中，按兩下 **[憑證服務用戶端 - 自動註冊]**。 **屬性**對話方塊隨即開啟。 設定下列項目，然後再按一下**確定**:

     1. 在 [設定模型] 中，選取 [啟用]。
     2. 選取 **更新過期的憑證，更新擱置中的憑證並移除撤銷的憑證**核取方塊。
     3. 選取 [**更新使用憑證範本的憑證**] 核取方塊。

9. 按一下 [確定] 。

## <a name="next-steps"></a>後續步驟

[重新整理群組原則](refresh-group-policy.md)
