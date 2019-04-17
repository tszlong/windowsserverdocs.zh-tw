---
title: "管理不同的網域 Active Directory 管理中心"
ms.prod: windows-server-threshold
description: "Windows Server 安全性"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理不同的網域 Active Directory 管理中心

>適用於：Windows Server（以每年次管道）、Windows Server 2016

  當您開放 Active Directory 系統，您目前登入在此電腦的網域 \ (本機 domain\) 會顯示在 Active Directory 管理中心瀏覽窗格中 \(the left pane\)。 根據您目前登入認證組的權限，您可以檢視或管理此本機網域中的 Active Directory 物件。

 您也可以使用的登入認證和相同的執行個體的 Active Directory 管理中心以檢視或管理的相同的樹系的任何其他網域或網域其他建立本機信任的樹系的 Active Directory 物件網域。 支援的方式 one\ 信任和 two\ 向信任。

> [!NOTE]
>  如果有網域 A 之間網域 B 透過 one\ 向信任的使用者網域 A 可存取 B 網域中的資源，但使用者網域 B 無法存取 A 網域中的資源，如果您正在執行 Active Directory 管理中心在電腦上的網域 A 您的本機網域中，您可以連接到網域 B 與您目前的登入認證，並在相同的執行個體的 Active Directory 管理中心設定。 但如果您正在執行 Active Directory 管理中心在電腦上網域 B 您當地的網域的位置，您無法連接到網域 A 以相同的設定中的 Active Directory 管理中心相同的執行個體的認證。

 還有不基本群組成員，才能完成此程序。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012：管理外部網域中選取的執行個體的 Active Directory 管理中心使用的登入認證目前的設定

1.  若要打開 Active Directory 管理中心，在**伺服器管理員**，按一下 [**工具**，然後按一下 [ **Active Directory 管理中心**。

    > [!NOTE]
    >  打開 Active Directory 管理中心另一種方式是按**[開始]**，然後輸入**dsac.exe**。

2.  打開到**新增瀏覽節點**，按一下 [**管理**，然後按一下 [**新增瀏覽節點**如下所示。

     ![螢幕擷取畫面顯示 * * 新增瀏覽節點 * * UI](media/ADDS_ADACAddNavNode.gif)

3.  在 [**新增瀏覽節點**，按一下 [**連接到其他網域**如下所示。

     ![螢幕擷取畫面顯示 * * 新增瀏覽節點 * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  在 [**連接到**，輸入您想要管理外部網域名稱 \ (，例如**contoso.com**\)，然後按一下 [**確定**。

5.  當您連接到外部網域成功時，瀏覽欄**新增瀏覽節點**視窗中，選取要新增到您 Active Directory 管理中心瀏覽窗格中，然後按一下 [容器**[確定]**。

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：管理外部網域中選取的執行個體的 Active Directory 管理中心使用的登入認證目前的設定

1.  若要打開 Active Directory 管理中心，按一下 [ **[開始]**，按一下 [**系統管理工具]**，然後按一下 [ **Active Directory 管理中心**。

    > [!NOTE]
    >  打開 Active Directory 管理中心另一種方式是按**[開始]**，按一下 [**執行**，然後輸入**dsac.exe**。

2.  打開**新增瀏覽節點**，靠近頂端的 [Active Directory 管理中心視窗中，按**新增瀏覽節點**中如下所示。

     ![螢幕擷取畫面顯示 * * 新增瀏覽節點 * * UI](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  打開另一種方法**新增瀏覽節點**是 right\ 按一下 Active Directory 管理中心瀏覽窗格中的空白區域中的任何位置，然後按一下 [**新增瀏覽節點**。

3.  在 [**新增瀏覽節點**，按一下 [**連接到其他網域**如下所示。

     ![螢幕擷取畫面顯示 * * 新增瀏覽節點 * * * * 連接到其他網域 * * UI](media/add_nav_nodes.gif)

4.  在 [**連接到**，輸入您想要管理外部網域名稱 \ (，例如**contoso.com**\)，然後按一下 [**確定**。

5.  當您連接到外部網域成功時，瀏覽欄**新增瀏覽節點**視窗中，選取要新增到您 Active Directory 管理中心瀏覽窗格中，然後按一下 [容器**[確定]**。

 如需有關自訂 Active Directory 管理中心瀏覽窗格中，請[自訂 Active Directory 系統管理員中心瀏覽窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

 您也可以使用的登入認證登入認證您目前的設定不同的一組開放 Active Directory 管理中心。 下列程序中的命令可如果您登入執行 Active Directory 管理中心使用一般的使用者的認證的電腦，但您想要使用這台電腦上的 Active Directory 管理中心管理您本機系統管理員的身分網域。 \（這個命令也可以是您想要使用遠端管理外部網域與當地的一組憑證登入認證您目前的設定不同的網域不同的 Active Directory 管理中心才有用。 不過，外部網域必須建立的信任與當地的網域。\)

 還有不基本群組成員，才能完成此程序。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>若要管理使用不同的目前的登入認證集登入認證網域中

1.  若要打開 Active Directory 管理中心，在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

     `runas /user:<domain\user> dsac`

     其中`<domain\user>`是您想要開放 Active Directory 管理中心的認證組和`dsac`是 Active Directory 管理中心可執行檔名稱 \(Dsac.exe\)。

     例如，輸入下列命令，並按一下 ENTER:

     `runas /user:contoso\administrator dsac`

2.  Active Directory 管理中心開放時，瀏覽以檢視或管理您的 Active Directory domain 瀏覽窗格。

  

