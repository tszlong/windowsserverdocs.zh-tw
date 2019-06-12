---
title: 管理 Active Directory 管理中心的不同網域
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd5650724272422d09e87b7eecf10f825b00fabf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447046"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>管理 Active Directory 管理中心的不同網域

>適用於：Windows Server （半年通道），Windows Server 2016

  當您開啟 Active Directory 系統管理，您目前登入此電腦的網域\(本機網域\)Active Directory 管理中心瀏覽窗格中會出現\(的左的窗格\). 根據您目前的登入認證集的權限，您可以檢視或管理這個本機網域中的 Active Directory 物件。

 您也可以使用同一組登入認證和相同的執行個體的 Active Directory 管理中心內檢視或管理任何其他網域中相同的樹系或已與本機建立的信任的另一個樹系的網域中的 Active Directory 物件網域。 這兩個的其中一個\-雙向信任和兩個\-支援雙向信任。

> [!NOTE]
>  如果沒有一個\-雙向信任網域 A 和網域 B 透過此網域 A 中的使用者可以存取網域 B 中的資源，但網域 B 的使用者無法存取網域 A 中的資源，如果您正在執行的電腦上的 Active Directory 管理中心內之間網域 A 其中是您的本機網域中，您可以連線到網域 B 與目前的登入認證及 Active Directory 管理中心內的相同執行個體設定。 但是，如果您執行 Active Directory 管理中心內，在電腦上在網域 B 您的本機網域，您無法連線到網域 A 與相同的 Active Directory 管理中心內相同的執行個體的認證集合。

 完成此程序沒有最低群組成員資格的限制。

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012:若要管理的 Active Directory 管理中心內，而使用目前的登入認證集選取的執行個體中的外部網域

1.  若要開啟 Active Directory 管理中心內，在**伺服器管理員**，按一下**工具**，然後按一下**Active Directory 管理中心**。

    > [!NOTE]
    >  若要開啟 Active Directory 管理中心內的另一個方法是按一下**開始**，然後輸入**dsac.exe**。

2.  若要開啟 **新增瀏覽節點**，按一下**管理**，然後按一下**新增瀏覽節點**如下圖所示。

     ![螢幕擷取畫面顯示 [新增瀏覽節點] UI](media/ADDS_ADACAddNavNode.gif)

3.  在 **新增瀏覽節點**，按一下**連線至其他網域**如下圖所示。

     ![螢幕擷取畫面顯示 [新增瀏覽節點] UI](media/ADDS_ADACConnectToDomain.gif)

4.  在**連接到**，輸入您想要管理的外部網域名稱\(比方說， **contoso.com**\)，然後按一下**確定**。

5.  當您成功連接到外部網域時，瀏覽中的資料行**新增瀏覽節點**] 視窗中，選取 [新增至 Active Directory 管理中心瀏覽窗格的容器和然後按一下**確定**。

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2：若要管理的 Active Directory 管理中心內，而使用目前的登入認證集選取的執行個體中的外部網域

1. 若要開啟 Active Directory 管理中心，按一下**開始**，按一下**系統管理工具**，然後按一下**Active Directory 管理中心**。

   > [!NOTE]
   >  若要開啟 Active Directory 管理中心內的另一個方法是按一下**開始**，按一下**執行**，然後輸入**dsac.exe**。

2. 若要開啟 **新增瀏覽節點**，附近的 Active Directory 管理中心 視窗頂端，按一下**新增瀏覽節點**如下圖所示。

    ![螢幕擷取畫面顯示 [新增瀏覽節點] UI](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  若要開啟的另一種方式**新增瀏覽節點**是右\-Active Directory 管理中心瀏覽窗格中的空白空間的任何位置按一下，然後按一下**新增瀏覽節點**.

3. 在 **新增瀏覽節點**，按一下**連線至其他網域**如下圖所示。

    ![螢幕擷取畫面顯示 [新增瀏覽節點] * * 連接到其他網域 * * UI](media/add_nav_nodes.gif)

4. 在**連接到**，輸入您想要管理的外部網域名稱\(比方說， **contoso.com**\)，然後按一下**確定**。

5. 當您成功連接到外部網域時，瀏覽中的資料行**新增瀏覽節點**] 視窗中，選取 [新增至 Active Directory 管理中心瀏覽窗格的容器和然後按一下**確定**。

   如需有關自訂 Active Directory 管理中心瀏覽窗格的詳細資訊，請參閱 <<c0> [ 來自訂 Active Directory 管理中心瀏覽窗格](customize-the-active-directory-administrative-center-navigation-pane.md)。

   您也可以開啟 Active Directory 管理中心內使用一組登入認證與您目前的登入認證集不同。 下列程序中的命令很有用，如果您以標準使用者認證執行 Active Directory 管理中心的電腦登入，但您想要使用這台電腦上的 Active Directory 管理中心來管理您以系統管理員身分的本機網域。 \(此命令也相當有用，如果您想要使用 Active Directory 管理中心遠端管理一組與您目前的登入認證集不同的認證與您本機網域不同的外部網域。 不過，外部網域必須與本機網域建立的信任。\)

   完成此程序沒有最低群組成員資格的限制。

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>使用和目前的登入認證集不同的登入認證來管理網域

1.  若要開啟 Active Directory 管理中心內，在命令提示字元中，輸入下列命令，命令並按 ENTER:

     `runas /user:<domain\user> dsac`

     何處`<domain\user>`是一組您想要開啟 Active Directory 管理中心內，使用的認證並`dsac`是 Active Directory 管理中心執行檔名稱\(Dsac.exe\)。

     例如，輸入下列命令，然後按下 ENTER：

     `runas /user:contoso\administrator dsac`

2.  開啟 Active Directory 管理中心時，瀏覽 [瀏覽] 窗格來檢視或管理您的 Active Directory 網域。

  

