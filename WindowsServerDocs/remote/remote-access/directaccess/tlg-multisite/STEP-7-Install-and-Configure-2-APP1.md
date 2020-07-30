---
title: 步驟7：安裝和設定 2-APP1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.prod: windows-server
ms.topic: article
ms.technology: networking-da
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ff2b9eacfa2ae888497371e3f6dffedf955d82d3
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409849"
---
# <a name="step-7-install-and-configure-2-app1"></a>步驟7：安裝和設定 2-APP1

>適用於：Windows Server (半年度管道)、Windows Server 2016

2-APP1 提供 web 與檔案共用服務。 2-APP1 設定包含下列各項：

- 在 2-APP1 上安裝作業系統

- 設定 TCP/IP 內容

- 聯結 2-APP1 至 CORP2 網域

- 在 2-APP1 上安裝網頁伺服器（IIS）角色

- 在 2-APP1 上建立共用資料夾

## <a name="install-the-operating-system-on-2-app1"></a><a name="bkmk_InstallOS"></a>在 2-APP1 上安裝作業系統
首先，安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

#### <a name="to-install-the-operating-system-on-2-app1"></a>若要在 2-APP1 上安裝作業系統

1.  開始安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完整安裝）。

2.  依指示完成安裝，為本機 Administrator 帳戶指定強式密碼。 使用本機 Administrator 帳戶登入。

3.  連接 2-APP1 至具有網際網路存取權的網路，並執行 Windows Update 以安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然後中斷網際網路連線。

4.  連接 2-APP1 至2公司網路子網。

## <a name="configure-tcpip-properties"></a><a name="bkmk_TCP"></a>設定 TCP/IP 屬性
在 2-APP1 上設定 TCP/IP 屬性。

#### <a name="to-configure-tcpip-properties"></a>設定 TCP/IP 內容

1.  在伺服器管理員主控台中，按一下 [**本機伺服器**]，然後在 [**屬性**] 區域中的 [**有線乙太**網路連線] 旁，按一下連結。

2.  在 [**網路**連線] 視窗中，以滑鼠**按右鍵 [****有線 Ethernet 連接**]，然後按一下 [內容]。

3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]****，然後按一下 [內容]****。

4.  按一下 [使用下列的 IP 位址]****。 在 [ **IP 位址**] 中，輸入**10.2.0.3**。 在 [子網路遮罩]**** 中，輸入 **255.255.255.0**。 在 [**預設閘道**] 中，輸入**10.2.0.254**。

5.  按一下 [使用下列的 DNS 伺服器位址]****。 在 [**慣用 DNS 伺服器**] 中，輸入**10.2.0.1**。

6.  按一下 [ **Advanced**]，然後按一下 [ **DNS** ] 索引標籤。在 [**這個連線的 DNS 尾碼**] 中，輸入**corp2.corp.contoso.com**，然後按兩次 **[確定]** 。

7.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]**，然後按一下 **[屬性]**。

8.  按一下 **[使用下列 IPv6 位址**]。 在 [ **IPv6 位址**] 中，輸入**2001： db8：2：： 3**。 在 [**子網前置長度**] 中，輸入**64**。 在 [**預設閘道**] 中，輸入**2001： db8：2：： fe**。 按一下 **[使用下列的 dns 伺服器位址**]，在 [**慣用 dns 伺服器**] 中，輸入**2001： db8：2：： 1**。

9. 按一下 [進階]****，然後按一下 [DNS]**** 索引標籤。

10. 在 [**這個連線的 DNS 尾碼**] 中，輸入**corp2.corp.contoso.com**，然後按兩次 **[確定]** 。

11. 在 [**有線乙太**網路連線屬性] 對話方塊上，按一下 [**關閉**]。

12. 關閉 [網路連線]**** 視窗。

## <a name="join-2-app1-to-the-corp2-domain"></a><a name="bkmk_JoinDomain"></a>聯結 2-APP1 至 CORP2 網域
聯結 2-APP1 至 corp2.corp.contoso.com 網域。

#### <a name="to-join-2-app1-to-the-corp2-domain"></a>將 2-APP1 加入 CORP2 網域

1.  在伺服器管理員主控台的 [**本機伺服器**] 的 [內容 **] 區域中，按一下 [** **電腦名稱稱**] 旁的連結。

2.  在 [系統內容]**** 的 [電腦名稱]**** 索引標籤上，按一下 [變更]****。

3.  在 [**電腦名稱稱**] 中，輸入**2-APP1**。 在 [**成員隸屬**] 中，按一下 [**網域**]，輸入**Corp2.corp.contoso.com**，然後按一下 **[確定]**。

4.  當系統提示您輸入使用者名稱和密碼時，請輸入**系統管理員**和其密碼，然後按一下 **[確定]**。

5.  當您看到對話方塊歡迎您加入 corp2.corp.contoso.com 網域時，請按一下 **[確定**]。

6.  當提示您必須重新啟動電腦時，按一下 [確定]****。

7.  按一下 [系統內容]**** 對話方塊中的 [關閉]****。

8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]****。

9. 在電腦重新開機之後，按一下 [**切換使用者**]，然後按一下 [**其他使用者**]，並使用系統管理員帳戶登入 CORP2 網域。

## <a name="install-the-web-server-iis-role-on-2-app1"></a><a name="bkmk_IIS"></a>在 2-APP1 上安裝網頁伺服器（IIS）角色
安裝網頁伺服器（IIS）角色，讓 web 伺服器具有 2 APP1。

#### <a name="to-install-the-web-server-iis-role"></a>若要安裝網頁伺服器（IIS）角色

1.  在伺服器管理員主控台的 [**儀表板**] 上，按一下 [**新增角色及功能**]。

2.  按三次 **[下一步]** 以移至伺服器角色選取畫面

3.  在 [**選取伺服器角色**] 頁面上，選取 [**網頁伺服器（IIS）**]，然後按 **[下一步]** 四次。

4.  在 [確認安裝選項]**** 頁面上，按一下 [安裝]****。

5.  確認安裝成功，然後按一下 [**關閉**]。

## <a name="create-a-shared-folder-on-2-app1"></a><a name="bkmk_Share"></a>在 2-APP1 上建立共用資料夾
在 2-APP1 的資料夾中建立共用資料夾和文字檔。

#### <a name="to-create-a-shared-folder"></a>建立共用資料夾

1.  在 [**開始**] 畫面上，輸入**explorer.exe**，然後按 enter。

2.  按一下 [**電腦**]，然後按兩下 **[本機磁片（C：）**]。

3.  按一下 [**新增資料夾**]，輸入**Files**，然後按 enter。 將 [**本機磁片**] 視窗保持開啟。

4.  在 [**開始**] 畫面上，輸入**notepad.exe**，以滑鼠右鍵按一下 [**記事本**]，按一下 [ **Advanced**]，然後按一下 [以**系統管理員身分執行**]。

5.  在 [未**命名的-記事本**] 視窗中，輸入**這是 2-APP1 上的共用**檔案。

6.  依序**按一下 [檔案**]、[**儲存** **]、[****電腦**]，再按兩下 [**本機磁片（C：）**]，然後按兩下 [檔案] 資料夾。

7.  在 [**檔案名**] 中，輸入**example.txt**，然後按一下 [**儲存**]。 關閉 [記事本]。

8.  在 [**本機磁片**] 視窗中，在 [檔案] 資料夾上按一下滑鼠右鍵，指向 [**共用位置**]，然後**按一下 [** **特定人員**]。

9. 在 [檔案**共用**] 對話方塊的下拉式清單中，按一下 [**所有人**]，然後按一下 [**新增**]。 在 [**每位使用者**的**許可權等級**] 中，按一下 [**讀取/寫入**]。

10. 按一下 [**共用**]，然後按一下 [**完成**]。

11. 關閉 [**本機磁片**] 視窗。



