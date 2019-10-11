---
title: ADBA 用戶端疑難排解
description: 逐步解說 Windows 啟用問題的疑難排解程序
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b4e31cfa892019e4f3bbcd3b67dbb42751cc58dd
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963034"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>範例：針對未啟用的 Active Directory 型啟用 (ADBA) 用戶端進行疑難排解

> [!NOTE]
> 本文最初是在 2018 年 3 月 26 日以 TechNet 部落格的形式發佈。

大家好！ 我的名字是 Mike Kammer，過去兩年來，我一直都是 Microsoft 的平台 PFE。 我最近已協助客戶在其環境中部署 Windows Server 2016。 我們藉此機會，同時將其啟用方法從 KMS 伺服器遷移到 [Active Directory 型啟用](https://docs.microsoft.com/previous-versions/windows/hh852637(v=win.10))。

作為進行所有變更的適當程序，我們已開始在客戶的測試環境中進行遷移。 我們已遵循 Charity Shelbourne 所撰寫的以下絕佳部落格文章中的指示：[Active Directory-Based Activation vs.Key Management Services](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016)。 我們的測試環境中的網域控制站全都執行 Windows Server 2012 R2，所以我們不需要準備我們的樹系。 我們已在 Windows Server 2012 R2 網域控制站上安裝角色，並選擇 Active Directory 型啟用作為大量啟用方法。 我們已安裝 KMS 金鑰並將其命名為「KMS AD 啟用 ( ** LAB)」。 我們幾乎已逐步依照部落格文章操作。

我們從建置四部虛擬機器、兩個 Windows 2016 Standard 和兩個 Windows 2016 Datacenter 開始著手。 到目前為止，一切都很好，而且每個人都很滿意。 我們建置了執行 Windows 2016 Standard 的實體伺服器，並已正常啟動機器。 我們的故事就到此為止。

哈哈！ 開玩笑的！ 從來沒有這麼容易過。 說實在的，安裝和設定超級容易，因此該部分很簡單又直接。 我在星期一回到辦公室，而我在前一週建置的所有虛擬機器都顯示並未啟用。 喂！ 這不對啊！ 我返回實體機器，其狀況良好。 我去找客戶討論發生了什麼事。 當然，第一個問題是「週末有什麼改變？」 一如往常，答案是「沒有」。 這次，真的沒有任何改變，我們必須找出發生了什麼事。

我進入其中一部有問題的服器，開啟命令提示字元，並從 **slmgr /ao-list** 命令檢查我的輸出。 **/ao-list** 參數會顯示 Active Directory 中的所有啟用物件。

![顯示 slmgr 命令的影像](./media/032618_1700_Troubleshoo1.png)

![顯示 slmgr 命令結果的影像](./media/032618_1700_Troubleshoo2.png)

結果顯示我們有兩個啟用物件：一個用於 Server 2012 R2，以及我們新建立的 KMS AD 啟用 (** LAB)，這是我們的 Windows Server 2016 授權。 這會確認我們的 Active Directory 已正確設定為啟用 Windows KMS 用戶端

知道 **slmgr** 命令是我的授權啟用的朋友，我就繼續使用不同的選項。 我嘗試了 **/dlv** 參數，這會顯示詳細的授權資訊。 我覺得很好，我執行的是 Windows Server 2016 標準版，因此有一個啟用識別碼、一個安裝識別碼、一個驗證 URL，甚至有部分的產品金鑰。

![顯示 slmgr /dlv 結果的影像](./media/ActivationTroubleshoot2b.jpg)

有人看到我在這個時間點錯過了什麼？ 我們會在其他疑難排解步驟之後回來，但是您可以在此螢幕擷取畫面中說出答案。

我現在的想法是基於某些原因，金鑰已損壞，因此我使用 **/upk** 參數來解除安裝目前的金鑰。 雖然這是移除金鑰的有效方式，但通常不是最佳的做法。 如果伺服器在取得新金鑰之前重新開機，則可能讓伺服器處於不良的狀態。 我發現使用 **/ipk** 參數 (稍後我會在疑難排解時進行) 會覆寫現有的金鑰，而且是比較安全的途徑。 從我的過失中學習！

![顯示 slmgr /upk 命令及其結果的影像](./media/032618_1700_Troubleshoo3.png)

我再次執行 **/dlv** 參數，以查看詳細的授權資訊。 不幸的是，我沒有得到任何有用的資訊，只有找不到產品金鑰錯誤。 當然，因為我剛才解除安裝，所以沒有任何金鑰！

![顯示 slmgr /dlv 命令及其結果的影像](./media/032618_1700_Troubleshoo4.png)

我認為它是 longshot，但我試過 **/ato** 參數，這應該會針對已知的 KMS 伺服器 (或 Active Directory 的情況) 啟動 Windows。 同樣地，只有找不到產品錯誤。

![顯示 slmgr /ato 命令及其結果的影像](./media/032618_1700_Troubleshoo5.png)

我的下一個想法是有時停止後啟動服務應該管用，所以我接下來嘗試這麼做。 我需要停止後啟動 Microsoft 軟體保護平台服務 (SPPSvc 服務)。 在系統管理命令提示字元中，我使用可靠的 **net stop** 和 **net start** 命令。 我先注意到服務並未執行，因此認為就是這樣！

![顯示 net stop 和 net start 命令結果的影像](./media/032618_1700_Troubleshoo6.png)

但不是這樣。 啟動服務並再次嘗試啟用 Windows 之後，仍會出現找不到產品錯誤。

我接著在其中一部有問題的伺服器上查看應用程式事件記錄檔。 我發現有關授權啟用 (事件識別碼 8198) 的錯誤，其錯誤碼為0x8007007B。

![顯示事件識別碼 8198 詳細資料的影像](./media/032618_1700_Troubleshoo7.png)

在查閱此錯誤碼時，我發現一篇文章指出我的錯誤碼表示檔案名、目錄名稱或磁碟區標籤語法不正確。 閱讀本文中所述的方法，並不是任何一種方法都符合我的情況。 當我執行 **nslookup -type=all _vlmcs._tcp** 命令時，我發現現有的 KMS 伺服器 (環境中仍有許多 Windows 7 和 Server 2008 機器，因此需要放在附近)，同時也有五個網域控制站。 這表示不是 DNS 問題，而我的問題在其他地方。

![顯示 nslookup 命令結果的影像](./media/032618_1700_Troubleshoo8.png)

所以我知道 DNS 沒問題。 Active Directory 已正確設定為 KMS 啟用來源。 我的實體伺服器已正確啟用。 這只是 VM 的問題嗎？ 此時順便提起一件有趣的事，就是客戶告知我，另一個部門也有人已決定要建置超過一打的虛擬 Windows Server 2016 機器。 所以我現在假設我還有另外一些不會啟用的伺服器要處理。 但不是這樣！ 這些伺服器都正常啟動。

我回到 **slmgr** 命令，弄清楚如何啟用這些怪物。 這次我將要使用 **/ipk** 參數，這可讓我安裝產品金鑰。 我進入了[這個網站](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11))，為我的 Windows Server 2016 標準版取得適當的金鑰。 我有些伺服器是資料中心，但我必須先修正此問題。

![顯示 KMS 用戶端安裝金鑰清單的影像](./media/032618_1700_Troubleshoo9.png)

我使用了 **/ipk** 參數來安裝產品金鑰，並選擇 Windows Server 2016 標準金鑰。

![顯示 slmgr /ipk 命令及其結果的影像](./media/032618_1700_Troubleshoo10.png)

從這裡開始，我只從我的資料中心體驗中擷取結果，但是相同。 我使用了 **/ato** 參數來強制啟用。 我們會得到產品已成功啟用的絕佳訊息！

![顯示 slmgr /ato 命令及其結果的影像](./media/032618_1700_Troubleshoo11.png)

再次使用 **/dlv** 參數，我們可以看到現在已由 Active Directory 啟用。

![顯示 slmgr /dlv 命令及其結果的影像](./media/032618_1700_Troubleshoo12.png)

接下來，出了什麼錯？ 為什麼我必須移除已安裝的金鑰並新增這些一般金鑰，才能讓這些機器正常啟動？ 為什麼其他一些機器在啟動時不會有任何問題？ 如先前所述，我在查看問題的最初階段中，錯過了一些關鍵。 我已全然困惑，因此聯繫了最初部落格文章的 Charity，看看她是否可以幫助我。 她立即看到問題，並協助我瞭解早期錯過了什麼。

當我執行第一個 **/dlv** 參數時，說明中是索引鍵。 說明是 Windows® 作業系統、零售通路。 我已看過並認為零售通路表示已經購買，而且是有效金鑰。

![顯示 slmgr /dlv 結果的影像，其中反白顯示通路資訊](./media/032618_1700_Troubleshoo13.png)

當我們從已正確啟用的伺服器查看 **/dlv** 參數的輸出時，請注意說明現在陳述 VOLUME_KMSCLIENT 通路。 這讓我們知道這確實是大量授權。

![顯示 slmgr /dlv 結果的影像，其中反白顯示通路資訊](./media/032618_1700_Troubleshoo14.png)

那麼，零售通路又代表什麼？ 嗯，這表示用來安裝作業系統的媒體是 MSDN ISO。 我回頭找我的客戶，詢問網路上是否基於某種原因而有第二個 Windows Server 2016 ISO 流通。 結果是網路上有另一個 ISO，且已用於建立其他一些機器。 它們會比較這兩個 ISO，且不出所料，事實上提供給我建置虛擬伺服器的是 MSDN ISO。 他們會從其網路中移除該 MSDN ISO，現在我們已啟動所有現有的伺服器，而不再擔心未來組建會啟用失敗。

我希望這有所幫助並讓可讓您之後省下一些時間！

Mike
