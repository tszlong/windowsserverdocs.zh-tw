---
title: 遷移 AD FS 2.0 同盟伺服器 WID 伺服器陣列
description: 提供將 AD FS 2.0 伺服器 WID 伺服器陣列遷移至 Windows Server 2012 的相關資訊
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: 8ee9d015b56dfec59e1d13a9ffa51f6297d60787
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962752"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>遷移 AD FS 2.0 WID 伺服器陣列
本檔提供將 AD FS 2.0 Windows 內部資料庫 (WID) 伺服器陣列遷移至 Windows Server 2012 的詳細資訊。

## <a name="migrate-an-ad-fs-wid-farm"></a>遷移 AD FS WID 伺服器陣列
若要將 WID 伺服器陣列遷移至 Windows Server 2012，請執行下列程式：

1.  針對 WID 伺服器陣列中 (server) 的每個節點，請檢查並執行[準備遷移 WID 伺服器](prepare-to-migrate-a-wid-farm.md)陣列中的程式。

2.  從負載平衡器移除任何非主要節點。

3.  將此伺服器上的作業系統從 Windows Server 2008 R2 或 Windows Server 2008 升級至 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11))。

> [!IMPORTANT]
>  作業系統升級會造成此伺服器上的 AD FS 設定遺失且 AD FS 2.0 伺服器角色會被移除。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須建立原始 AD FS 設定並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。

4. 在這個伺服器上建立原始 AD FS 設定。

您可以使用 [AD FS 同盟伺服器設定精靈]**** 建立原始 AD FS 設定，將同盟伺服器新增到 WID 伺服器陣列。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。

> [!NOTE]
> 當您來到 [AD FS 同盟伺服器設定精靈]**** 中的 [指定主要同盟伺服器和服務帳戶]**** 頁面時，請輸入 WID 伺服器陣列的主要同盟伺服器的名稱，而且必須輸入準備 AD FS 移轉時所記錄的服務帳戶資訊。 如需詳細資訊，請參閱[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。
>
> 當您到達 [**指定同盟服務名稱**] 頁面時，請務必選取您在[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)的「準備遷移 WID 伺服器陣列」中所記錄的同一個 SSL 憑證。

5. 更新這個伺服器上您的 AD FS 網頁。 當您準備進行遷移時，如果您已備份自訂的 AD FS 網頁，則需要使用備份資料來覆寫 **%systemdrive%\inetpub\adfs\ls**目錄中預設建立的預設 AD FS 網頁，做為 Windows Server 2012 上 AD FS 設定的結果。

6. 將您剛升級至 Windows Server 2012 的伺服器新增至負載平衡器。

7. 為 WID 伺服器陣列中其他的次要伺服器重複步驟 1 到 6。

8. 將 WID 伺服器陣列中其中一個已升級的次要伺服器升級成主要伺服器。 若要這樣做，請開啟 Windows PowerShell，然後執行下列命令： `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。

9. 從負載平衡器移除 WID 伺服器陣列中的原始主要伺服器。

10. 使用 Windows PowerShell，將 WID 伺服器陣列中的原始主要伺服器降級為次要伺服器。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將原始的主伺服器降級為次要伺服器： `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>` 。

11. 從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012，將您在 WID 伺服器陣列中的最後一個節點上的作業系統升級 (伺服器) 。 如需相關資訊，請參閱[安裝 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11))。

> [!IMPORTANT]
>  作業系統升級會造成此伺服器上的 AD FS 設定遺失且 AD FS 2.0 伺服器角色會被移除。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須手動建立原始 AD FS 設定並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。

12. 在 WID 伺服器陣列的這個最後一個節點 (伺服器) 上建立原始 AD FS 設定。

您可以使用 [AD FS 同盟伺服器設定精靈]**** 建立原始 AD FS 設定，將同盟伺服器新增到 WID 伺服器陣列。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。

> [!NOTE]
> 當您來到 [AD FS 同盟伺服器設定精靈]**** 的 [指定主要同盟伺服器和服務帳戶]**** 頁面時，請輸入您準備 AD FS 移轉時所記錄的服務帳戶資訊。 如需詳細資訊，請參閱[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。
>
> 當您到達 [**指定同盟服務名稱**] 頁面時，請務必選取您在[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)中所記錄的同一個 SSL 憑證。

13. 更新 WID 伺服器陣列中這個最後一個伺服器上的 AD FS 網頁。 當您準備進行遷移時，如果您已備份自訂的 AD FS 網頁，請使用備份資料來覆寫 **%systemdrive%\inetpub\adfs\ls**目錄中預設建立的預設 AD FS 網頁，做為 Windows Server 2012 上 AD FS 設定的結果。

14. 將您剛升級至 Windows Server 2012 的 WID 伺服器陣列的最後一部伺服器新增至負載平衡器。

15. 還原任何剩餘的 AD FS 自訂項目，例如自訂屬性存放區。

## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)[準備遷移 AD FS 2.0 同盟伺服器 proxy](prepare-to-migrate-ad-fs-fed-proxy.md) [遷移 AD FS 2.0 同盟](migrate-the-ad-fs-fed-server.md)伺服器遷移[AD FS 2.0 同盟伺服器 proxy](migrate-the-ad-fs-2-fed-server-proxy.md) [遷移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)程式
