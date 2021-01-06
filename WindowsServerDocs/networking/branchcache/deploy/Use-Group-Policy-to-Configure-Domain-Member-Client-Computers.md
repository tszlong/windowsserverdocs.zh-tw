---
title: 使用群組原則設定網域成員用戶端電腦
description: 瞭解如何使用群組原則設定網域成員用戶端電腦。
manager: dougkim
ms.topic: how-to
ms.assetid: 911c1538-f79d-42e9-ba38-f4618f87b008
ms.author: lizross
author: eross-msft
ms.date: 06/02/2018
ms.openlocfilehash: cc212a899c1fe0cce24ab8a1a6aea9aa232c6e82
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948894"
---
# <a name="use-group-policy-to-configure-domain-member-client-computers"></a>使用群組原則設定網域成員用戶端電腦

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本節中，您會為組織中的所有電腦建立群組原則物件、使用分散式快取模式或託管快取模式來設定網域成員用戶端電腦，以及設定具有 Advanced Security 的 Windows 防火牆以允許 BranchCache 流量。

本節包含下列程式。

1.  [建立群組原則物件並設定 BranchCache 模式](#bkmk_gp)

2.  [設定具有 Advanced Security 輸入流量規則的 Windows 防火牆](#bkmk_inbound)

3.  [設定具有 Advanced Security 輸出流量規則的 Windows 防火牆](#bkmk_outbound)

> [!TIP]
> 在下列程式中，系統會指示您在預設網域原則中建立群組原則物件，不過，您可以在組織單位 (OU) 或適用于您部署的其他容器中建立物件。

您必須是 **Domain Admins** 的成員或同等許可權，才能執行這些程式。

## <a name="to-create-a-group-policy-object-and-configure-branchcache-modes"></a><a name="bkmk_gp"></a>建立群組原則物件並設定 BranchCache 模式

1.  在安裝 Active Directory Domain Services 伺服器角色的電腦上伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。 此時會開啟群組原則管理主控台。

2.  在群組原則管理主控台中，展開下列路徑： [ **樹系：** *example.com*]、[ **網域**]、[ *example.com*]、[ **群組原則物件**]，其中 *example.com* 是您要設定的 BranchCache 用戶端電腦帳戶所在網域的名稱。

3.  以滑鼠右鍵按一下 [群組原則物件] ，然後按一下 [新增] 。 [ **新增 GPO** ] 對話方塊隨即開啟。 在 [ **名稱**] 中，輸入新群組原則物件 (GPO) 的名稱。 例如，如果您想要命名物件 BranchCache 用戶端電腦，請輸入 **Branchcache 用戶端電腦**。 按一下 [確定]。

4.  在群組原則管理主控台中，確定已選取 [ **群組原則物件** ]，然後在 [詳細資料] 窗格中，以滑鼠右鍵按一下您剛才建立的 GPO。 例如，如果您將 GPO BranchCache 用戶端電腦命名為，請以滑鼠右鍵按一下 [ **Branchcache 用戶端電腦**]。 按一下 **[編輯]** 。 群組原則管理編輯器主控台隨即開啟。

5.  在群組原則管理編輯器主控台中，展開下列路徑： [ **電腦** 設定]、[ **原則**]、[ **系統管理範本：原則定義] (ADMX 檔案，) 取自本機電腦**、 **網路**、 **BranchCache**。

6.  按一下 [ **branchcache**]，然後在詳細資料窗格中，按兩下 [ **開啟 branchcache**]。 [原則設定] 對話方塊隨即開啟。

7.  在 [ **開啟 BranchCache** ] 對話方塊中，按一下 [ **已啟用**]，然後按一下 **[確定]**。

8.  若要啟用 BranchCache 分散式快取模式，請在詳細資料窗格中，按兩下 [ **設定 BranchCache 分散式** 快取模式]。 [原則設定] 對話方塊隨即開啟。

9. 在 [ **設定 BranchCache 分散式** 快取模式] 對話方塊中，按一下 [ **已啟用**]，然後按一下 **[確定]**。

10. 如果您有一或多個分公司以託管快取模式部署 BranchCache，且您已在這些辦公室部署託管快取伺服器，請按兩下 [ **依服務連接點啟用自動託管** 快取探索]。 [原則設定] 對話方塊隨即開啟。

11. 在 [ **依服務連接點啟用自動託管** 快取探索] 對話方塊中，按一下 [ **已啟用**]，然後按一下 **[確定]**。

    > [!NOTE]
    > 當您同時啟用 **Set BranchCache 分散式** 快取模式和啟用自動裝載快取 **探索（依服務連接點** 原則設定）時，用戶端電腦會在 BranchCache 分散式快取模式下運作，除非它們找到分公司的託管快取伺服器，此時它們會在託管快取模式下運作。

12. 使用下列程式，在用戶端電腦上使用群組原則設定防火牆設定。

## <a name="to-configure-windows-firewall-with-advanced-security-inbound-traffic-rules"></a><a name="bkmk_inbound"></a>設定具有 Advanced Security 輸入流量規則的 Windows 防火牆

1.  在群組原則管理主控台中，展開下列路徑： [ **樹系：** *example.com*]、[ **網域**]、[ *example.com*]、[ **群組原則物件**]，其中 *example.com* 是您要設定的 BranchCache 用戶端電腦帳戶所在網域的名稱。

2.  在群組原則管理主控台中，確定已選取 [ **群組原則物件** ]，然後在 [詳細資料] 窗格中，以滑鼠右鍵按一下您先前建立的 BranchCache 用戶端電腦 GPO。 例如，如果您將 GPO BranchCache 用戶端電腦命名為，請以滑鼠右鍵按一下 [ **Branchcache 用戶端電腦**]。 按一下 **[編輯]** 。 群組原則管理編輯器主控台隨即開啟。

3.  在群組原則管理編輯器主控台中，展開下列路徑： [ **電腦** 設定]、[ **原則**]、[ **windows 設定**]、[ **安全性設定**]、[ **具有 Advanced Security 的 windows 防火牆**]、[ **具有 advanced security 的 windows 防火牆**]、[ **輸入規則**]。

4.  以滑鼠右鍵按一下 [**輸入規則**]，然後按一下 [**新增規則**]。 [新增輸入規則] 嚮導隨即開啟。

5.  在 [ **規則類型**] 中，按一下 [ **預先定義**]，展開選項清單，然後按一下 [ **BranchCache-內容抓取 (使用 HTTP)**。 按一下 [下一步] 。

6.  在 **預先定義的規則** 中，按 **[下一步]**。

7.  在 [ **動作**] 中，確定已選取 **[允許連接** ]，然後按一下 **[完成]**。

    > [!IMPORTANT]
    > 您必須選取 [允許 BranchCache 用戶端的連線能夠接收此埠上 **的** 流量]。

8.  若要建立 WS-Discovery 防火牆例外，請再次以滑鼠右鍵按一下 [ **輸入規則**]，然後按一下 [ **新增規則**]。 [新增輸入規則] 嚮導隨即開啟。

9. 在 [ **規則類型**] 中，按一下 [ **預先定義**]，展開選項清單，然後按一下 [ **BranchCache-對等探索 (使用 WSD)**。 按一下 [下一步] 。

10. 在 **預先定義的規則** 中，按 **[下一步]**。

11. 在 [ **動作**] 中，確定已選取 **[允許連接** ]，然後按一下 **[完成]**。

    > [!IMPORTANT]
    > 您必須選取 [允許 BranchCache 用戶端的連線能夠接收此埠上 **的** 流量]。

## <a name="to-configure-windows-firewall-with-advanced-security-outbound-traffic-rules"></a><a name="bkmk_outbound"></a>設定具有 Advanced Security 輸出流量規則的 Windows 防火牆

1.  在群組原則管理編輯器主控台中，以滑鼠右鍵按一下 [ **輸出規則**]，然後按一下 [ **新增規則**]。 [新增輸出規則] 嚮導隨即開啟。

2.  在 [ **規則類型**] 中，按一下 [ **預先定義**]，展開選項清單，然後按一下 [ **BranchCache-內容抓取 (使用 HTTP)**。 按一下 [下一步] 。

3.  在 **預先定義的規則** 中，按 **[下一步]**。

4.  在 [ **動作**] 中，確定已選取 **[允許連接** ]，然後按一下 **[完成]**。

    > [!IMPORTANT]
    > 您必須選取 [允許 BranchCache 用戶端 **的** 連線能夠在此埠上傳送流量]。

5.  若要建立 WS-Discovery 防火牆例外，請再次以滑鼠右鍵按一下 [ **輸出規則**]，然後按一下 [ **新增規則**]。 [新增輸出規則] 嚮導隨即開啟。

6.  在 [ **規則類型**] 中，按一下 [ **預先定義**]，展開選項清單，然後按一下 [ **BranchCache-對等探索 (使用 WSD)**。 按一下 [下一步] 。

7.  在 **預先定義的規則** 中，按 **[下一步]**。

8.  在 [ **動作**] 中，確定已選取 **[允許連接** ]，然後按一下 **[完成]**。

    > [!IMPORTANT]
    > 您必須選取 [允許 BranchCache 用戶端 **的** 連線能夠在此埠上傳送流量]。



