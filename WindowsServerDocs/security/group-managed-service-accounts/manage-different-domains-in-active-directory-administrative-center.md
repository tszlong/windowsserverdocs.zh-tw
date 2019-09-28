---
title: 管理 Active Directory 管理中心中的不同網域
ms.prod: windows-server
description: Windows Server 安全性
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 71edf6bb38cc665fe5c780ce986d0c0b8807d6ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386934"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理 Active Directory 管理中心中的不同網域

>適用於：Windows Server (半年度管道)、Windows Server 2016

  當您開啟 Active Directory 系統管理時，您目前在這部電腦上登入的網域 \(the local domain @ no__t-1 會出現在 Active Directory 管理中心流覽窗格中，\(the 左窗格 @ no__t-3。 根據您目前登入認證集的許可權，您可以查看或管理此本機網域中的 Active Directory 物件。

 您也可以使用同一組登入認證和相同的 Active Directory 管理中心實例，來查看或管理相同樹系中任何其他網域中的 Active Directory 物件，或另一個樹系中已建立與本機的信任的網域domain. 同時支援一個 @ no__t-0way 信任和兩個 @ no__t-1way 信任。

> [!NOTE]
>  如果網域 A 和網域 B 之間有一個 @ no__t-0way 信任，而網域 A 中的使用者可以存取網域 B 中的資源，但網域 B 中的使用者無法存取網域 A 中的資源，則如果您在電腦上執行 Active Directory 管理中心，網域 A 是您的本機網域，您可以使用目前的登入認證組和 Active Directory 管理中心的相同實例來連接到網域 B。 但是，如果您是在網域 B 是本機網域的電腦上執行 Active Directory 管理中心，就無法在相同的 Active Directory 管理中心實例中，使用同一組認證連接到網域 A。

 完成此程序沒有最低群組成員資格的限制。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012：若要使用目前的登入認證集來管理所選 Active Directory 管理中心實例中的外部網域

1.  若要開啟 Active Directory 管理中心，請在**伺服器管理員**中按一下 [**工具**]，然後按一下 [ **Active Directory 管理中心**]。

    > [!NOTE]
    >  開啟 Active Directory 管理中心的另一種方式是按一下 [**開始**]，然後輸入**dsac.exe**。

2.  若要開啟 [**新增流覽節點**]，請按一下 [**管理**]，然後按一下 [**新增導覽節點**]，如下圖所示。

     ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/ADDS_ADACAddNavNode.gif)

3.  在 [**新增流覽節點**] 中，按一下 **[連接到其他網域]** ，如下圖所示。

     ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/ADDS_ADACConnectToDomain.gif)

4.  在 **[連線到]** 中，輸入您要管理的外部功能變數名稱 \(for 範例中， **contoso.com**\)，然後按一下 **[確定]** 。

5.  當您成功連線到外部網域時，流覽 [**新增流覽節點**] 視窗中的資料行，選取要加入至 Active Directory 管理中心流覽窗格的容器，然後按一下 **[確定]。** .

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：若要使用目前的登入認證集來管理所選 Active Directory 管理中心實例中的外部網域

1. 若要開啟 Active Directory 管理中心，請依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [ **Active Directory 管理中心**]。

   > [!NOTE]
   >  開啟 Active Directory 管理中心的另一種方式是按一下 [**開始**]，按一下 [**執行**]，然後輸入**dsac.exe**。

2. 若要開啟 [**新增導覽節點**]，請在 [Active Directory 管理中心] 視窗頂端附近，按一下 [**新增導覽節點**]，如下圖所示。

    ![顯示 [新增流覽節點] * * UI 的螢幕擷取畫面](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  另一種開啟 [**新增導覽節點**] 的方式是在 Active Directory 管理中心流覽窗格中，于空白空間的任何位置1click，然後按一下 [**新增導覽節點**]。

3. 在 [**新增流覽節點**] 中，按一下 **[連接到其他網域]** ，如下圖所示。

    ![顯示 [新增流覽節點] * * * [連接到其他網域] * * UI 的螢幕擷取畫面](media/add_nav_nodes.gif)

4. 在 **[連線到]** 中，輸入您要管理的外部功能變數名稱 \(for 範例中， **contoso.com**\)，然後按一下 **[確定]** 。

5. 當您成功連線到外部網域時，流覽 [**新增流覽節點**] 視窗中的資料行，選取要加入至 Active Directory 管理中心流覽窗格的容器，然後按一下 **[確定]。** .

   如需自訂 Active Directory 管理中心流覽窗格的詳細資訊，請參閱[自訂 Active Directory 管理中心流覽窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

   您也可以使用一組不同于目前登入認證的登入認證來開啟 Active Directory 管理中心。 如果您使用一般使用者認證登入執行 Active Directory 管理中心的電腦，但想要在這部電腦上使用 Active Directory 管理中心來管理您的，則下列程式中的命令會很有用。以系統管理員身分使用的本機網域。 如果您想要使用 Active Directory 管理中心從遠端系統管理與您的本機網域不同的外部網域，而這組認證與您目前的登入認證組不同，@no__t 0This 命令也會很有用。 不過，外部網域必須與本機網域建立信任。 \)

   完成此程序沒有最低群組成員資格的限制。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>使用和目前的登入認證集不同的登入認證來管理網域

1.  若要開啟 Active Directory 管理中心，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵：

     `runas /user:<domain\user> dsac`

     其中 `<domain\user>` 是您想要開啟 Active Directory 管理中心的一組認證，而 `dsac` 是 Active Directory 管理中心可執行檔名稱 \(Dsac @ no__t-3。

     例如，輸入下列命令，然後按下 ENTER：

     `runas /user:contoso\administrator dsac`

2.  當 Active Directory 管理中心開啟時，流覽導覽窗格以查看或管理您的 Active Directory 網域。

  

