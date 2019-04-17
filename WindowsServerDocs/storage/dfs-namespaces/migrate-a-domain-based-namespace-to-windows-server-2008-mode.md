---
title: "將網域型命名空間移轉到 Windows Server 2008 模式"
description: "本文說明如何將網域型命名空間移轉到 Windows Server 2008 模式"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ef356e4f2abccb375732b7ec60a374c64dc0e7f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>將網域型命名空間移轉到 Windows Server 2008 模式

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

網域型命名空間的 Windows Server 2008 模式包含支援存取型列舉和提高延展性。

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>若要將網域型命名空間移轉到 Windows Server 2008 模式

若要將網域型命名空間從 Windows 2000 Server 模式移轉到 Windows Server 2008 模式，您必須將命名空間匯出至檔案、刪除命名空間、在 Windows Server 2008 模式中重新建立它，然後匯入命名空間設定。 若要這樣做，請使用下列程序：

1.  開啟命令提示字元視窗並輸入下列命令，以將命名空間匯出至檔案，其中 \\\*domain*\\*namespace* 是適當網域的名稱，而 *path\\filename* 是匯出檔案的路徑和檔案名稱：
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  記下每個命名空間伺服器的路徑 (\\\*server* \\*share*)。 您必須手動新增命名空間伺服器至重新建立的命名空間，因為 Dfsutil 無法匯入命名空間伺服器。
3.  在 DFS 管理中，在命名空間上按一下滑鼠右鍵，然後按一下 **\[刪除\]**，或在命令提示字元中輸入下列命令， <br /> 其中 \\\\*domain*\\*namespace* 是適當的網域和命名空間名稱：
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  在 DFS 管理中，使用相同的名稱重新建立命名空間，但改為使用 Windows Server 2008 模式，或在命令提示字元中輸入下列命令，其中 <br /> \\\\*server*\\*namespace* 是命名空間根目錄的適當伺服器與共用的名稱：
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  若要從匯出檔案匯入命名空間，請在命令提示字元中輸入下列命令，其中 <br /> \\\\*domain*\\*namespace* 是適當的網域與命名空間的名稱，而 *path\\filename* 是要匯入之檔案的路徑與檔案名稱：
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > 若要將匯入大型命名空間所需的時間降至最低，請在命名空間伺服器的本機上執行 **Dfsutil** 根匯入命令。
6.  將任何其他命名空間伺服器新增至重新建立的命名空間，方法是在 DFS 管理的命名空間上按一下滑鼠右鍵，然後按一下 **\[新增命名空間伺服器\]**，或在命令提示字元中輸入下列命令，其中 <br /> \\\\*server*\\*share* 是命名空間根目錄的適當伺服器與共用的名稱：
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > 您可以在匯入命名空間之前先新增命名空間伺服器，但這樣做會導致命名空間伺服器在新增命名空間伺服器之後，以遞增方式下載命名空間的中繼資料，而非立即下載整個命名空間。

## <a name="see-also"></a>請參閱
-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)