---
title: 管理 Active Directory 管理中心中的不同網域
description: Windows Server 安全性
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8ccd183e3450fd6c520790d9130d27b99b06a5ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971445"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理 Active Directory 管理中心中的不同網域

>適用於：Windows Server (半年度管道)、Windows Server 2016

  當您開啟 Active Directory 系統管理時， \( 本機網域 \) 會出現在左窗格的 [Active Directory 管理中心] 流覽窗格中 \( \) 。 根據目前登入認證集的權限，您可以檢視或管理這個本機網域中的 Active Directory 物件。

 您也可以使用同一組登入認證和相同的 Active Directory 管理中心實例，來查看或管理相同樹系中任何其他網域中的 Active Directory 物件，或另一個樹系中與本機網域建立信任的網域。 單向 \- 信任和雙向 \- 信任都受到支援。

> [!NOTE]
>  如果網域 a 和網域 B 之間有單向信任，而網域 A 中的 \- 使用者可以存取網域 b 中的資源，但網域 b 中的使用者無法存取網域 a 中的資源 Active Directory 管理中心，則您可以使用目前的登入認證組和相同的 Active Directory 管理中心實例來連接到網域 B。 但是，如果您是在網域 B 是您本機網域的電腦上執行 Active Directory 管理中心，則無法在相同 Active Directory 管理中心執行個體內使用相同的登入認證集來連線到網域 A。

 完成此程序沒有最低群組成員資格的限制。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012：若要使用目前的登入認證集來管理所選 Active Directory 管理中心實例中的外部網域

1.  若要開啟 Active Directory 管理中心，請在**伺服器管理員**中按一下 [**工具**]，然後按一下 [ **Active Directory 管理中心**]。

    > [!NOTE]
    >  開啟 Active Directory 管理中心的另一種方式是按一下 [**開始**]，然後輸入**dsac.exe**。

2.  若要開啟 [**新增流覽節點**]，請按一下 [**管理**]，然後按一下 [**新增導覽節點**]，如下圖所示。

     ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/ADDS_ADACAddNavNode.gif)

3.  在 [**新增流覽節點**] 中，按一下 **[連接到其他網域]** ，如下圖所示。

     ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/ADDS_ADACConnectToDomain.gif)

4.  在 **[連線到]** 中，輸入您要管理的外部功能變數名稱（ \( 例如， **contoso.com** \) ），然後按一下 **[確定]**。

5.  當您成功連線到外部網域，可以瀏覽 [新增瀏覽節點]**** 視窗中的欄位，選擇要加入 Active Directory 管理中心瀏覽窗格的容器，然後按一下 [確定]****。

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：若要使用目前的登入認證集來管理所選 Active Directory 管理中心實例中的外部網域

1. 如果要開啟 [Active Directory 管理中心]，請依序按一下 [開始]****、[系統管理工具]****，然後按一下 [Active Directory 管理中心]****。

   > [!NOTE]
   >  開啟 Active Directory 管理中心的另一種方式是依序按一下 [**開始**] 和 [**執行**]，然後輸入**dsac.exe**。

2. 若要開啟 [**新增導覽節點**]，請在 [Active Directory 管理中心] 視窗頂端附近，按一下 [**新增導覽節點**]，如下圖所示。

    ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  另一個開啟 [**新增導覽節點**] 的方法，是以滑鼠右鍵 \- 按一下 Active Directory 管理中心流覽窗格中空白處的任何位置，然後按一下 [**新增導覽節點**]。

3. 在 [**新增流覽節點**] 中，按一下 **[連接到其他網域]** ，如下圖所示。

    ![顯示 [新增流覽節點] * * * [連接到其他網域] * * UI 的螢幕擷取畫面](media/add_nav_nodes.gif)

4. 在 **[連線到]** 中，輸入您要管理的外部功能變數名稱（ \( 例如， **contoso.com** \) ），然後按一下 **[確定]**。

5. 當您成功連線到外部網域，可以瀏覽 [新增瀏覽節點]**** 視窗中的欄位，選擇要加入 Active Directory 管理中心瀏覽窗格的容器，然後按一下 [確定]****。

   如需自訂 Active Directory 管理中心流覽窗格的詳細資訊，請參閱[自訂 Active Directory 管理中心流覽窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

   您也可以使用與目前登入認證集不同的登入認證集來開啟 Active Directory 管理中心。 如果您是使用標準使用者認證來登入執行 Active Directory 管理中心的電腦，但是想要以系統管理員的身分使用這部電腦上的 Active Directory 管理中心來管理本機網域，則以下程序中的命令十分有用。 \(如果您想要使用 Active Directory 管理中心從遠端系統管理與您的本機網域不同的外部網域，而這組認證與目前的登入認證組不同，此命令也很有用。 不過，外部網域必須與本機網域建立信任。\)

   完成此程序沒有最低群組成員資格的限制。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>使用和目前的登入認證集不同的登入認證來管理網域

1.  若要開啟 Active Directory 管理中心，在命令提示字元中輸入下列命令，然後按下 ENTER：

     `runas /user:<domain\user> dsac`

     其中 `<domain\user>` 是您想要開啟 Active Directory 管理中心的認證集，而 `dsac` 是 Active Directory 管理中心可執行檔名稱 \(Dsac.exe\) 。

     例如，輸入下列命令，然後按 ENTER：

     `runas /user:contoso\administrator dsac`

2.  當 Active Directory 管理中心開啟，可以瀏覽整個瀏覽窗格，以檢視或管理您的 Active Directory 網域。



