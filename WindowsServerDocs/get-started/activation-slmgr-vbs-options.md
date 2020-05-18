---
title: 用於取得大量啟用資訊的 Slmgr.vbs 選項
description: 列出 Slmgr.vbs 指令碼可用的選項並說明其使用方式
ms.date: 09/24/2019
ms.technology: server-general
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: 25c12322ef648655a301931c9273e8d941ebe62e
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235226"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>用於取得大量啟用資訊的 Slmgr.vbs 選項

以下說明 Slmgr.vbs 指令碼的語法，而本文中的表格會說明每個命令列選項。

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> 在本文中，選擇性引數會以方括弧 \[] 括住，而預留位置則以角括弧 \<> 括住。 當您鍵入這些陳述式時，請省略括弧，並使用對應的值來取代預留位置。

> [!NOTE]
> 如需其他使用大量啟用的軟體相關資訊，請參閱特別為那些應用程式撰寫的文件。

## <a name="using-slmgr-on-remote-computers"></a>在遠端電腦上使用 Slmgr

若要管理遠端用戶端，請使用大量啟用管理工具 (VAMT) 1.2 版或更新版本，或建立能夠感知平台之間差異的自訂 WMI 指令碼。 如需大量啟用的 WMI 屬性和方法詳細資訊，請參閱[大量啟用的 WMI 屬性和方法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502536(v=ws.11))。

> [!IMPORTANT]
> 由於 Windows 7 和 Windows Server 2008 R2 中的 WMI 變更，Slmgr.vbs 指令碼不能跨平台運作。 不支援從 Windows Vista&reg; 作業系統使用 Slmgr.vbs 來管理 Windows 7 或 Windows Server 2008 R2 系統。 嘗試從 Windows 7 或 Windows Server 2008 R2 管理較舊的系統將會產生特定的版本不符錯誤。 例如，執行 **cscript slmgr.vbs \<vista\_machine\_name\> /dlv** 會產生下列輸出：
>  
>> Microsoft (R) Windows Script Host 5.8 版著作權 (C) Microsoft Corporation. 著作權所有，並保留一切權利。
>>  
>> 遠端機器不支援此版本的 SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>一般 Slmgr.vbs 選項

|選項 |說明 |
| - | - |
|\[\<ComputerName\>] |遠端電腦名稱 (預設為本機電腦) |
|\[\<User\>] |遠端電腦上具備必要權限的帳戶 |
|\[\<Password\>] |遠端電腦上具備必要權限之帳戶的密碼 |

## <a name="global-options"></a>全域選項

|選項 |說明 |
| - | - |
|\/ipk&nbsp;&lt;ProductKey&gt; |嘗試安裝 5×5 產品金鑰。 參數所提供的產品金鑰已確認有效，且適用於安裝的作業系統。<br />如果無效，則會傳回錯誤。<br />如果金鑰有效且適用，就會安裝金鑰。 如果金鑰已安裝，則會以無訊息方式取代。<br />為了防止授權服務不穩定，最好重新啟動系統或重新啟動軟體保護服務。<br />必須從已提升權限的命令提示字元視窗中執行此作業，或者必須設定 [標準使用者作業] 登錄值，才能允許未獲授權的使用者具有軟體保護服務的額外存取權。 |
|/ato&nbsp;\[\<Activation&nbsp;ID\>] |如果是零售版和使用 KMS 主機金鑰或多次啟用金鑰 (MAK) 安裝的大量系統， **/ato** 會提示 Windows 嘗試線上啟用。<br />如果是已安裝一般大量授權金鑰 (GVLK) 的系統，這會提示進行 KMS 啟用嘗試。 已設定為暫停自動 KMS 啟用嘗試 ( **/stao**) 的系統仍會在執行 **/ato** 時嘗試 KMS 啟用。<br />**注意：** 從 Windows 8 (與 Windows Server 2012) 開始，已不再使用 **/stao** 選項。 請改用 **/act-type** 選項。<br />參數 \<**Activation ID**\> 會擴大 **/ato** 支援，以識別電腦上安裝的 Windows 版本。 指定 \<**Activation ID**\> 參數會隔離與該啟用識別碼相關聯版本的選項效果。 執行 **slmgr.vbs /dlv all** 可取得已安裝 Windows 版本的啟用識別碼。 如果您必須支援其他應用程式，請參閱該應用程式指南的進一步指示。<br />KMS 啟用不需要提高的權限。 不過，線上啟用需要提高權限，或者必須設定 [標準使用者操作] 登錄值以允許未獲授權的使用者具有軟體保護服務的額外存取權。 |
|\/dli&nbsp;\[<Activation&nbsp;ID\>&nbsp;\|&nbsp;All\] |顯示授權資訊。<br />根據預設， **/dli** 會顯示已安裝之作用中 Windows 版本的授權資訊。 指定 \<**Activation ID**\> 參數會顯示與該啟用識別碼相關聯之指定版本的授權資訊。 指定 **All** 作為參數會顯示所有適用之已安裝產品的授權資訊。<br />此操作不需要提升的權限。 |
|\/dlv&nbsp;\[<Activation&nbsp;ID\>&nbsp;\|&nbsp;All\] |顯示詳細的授權資訊。<br />根據預設， **/dlv** 會顯示已安裝作業系統的授權資訊。 指定 \<**Activation ID**\> 參數會顯示與該啟用識別碼相關聯之指定版本的授權資訊。 指定 **All** 參數會顯示所有適用之已安裝產品的授權資訊。<br />此操作不需要提升的權限。 |
|\/xpr \[\<Activation&nbsp;ID\>] |顯示產品的啟用到期日。 根據預設，這是指目前的 Windows 版本且主要用於 KMS 用戶端，因為 MAK 和零售版啟用沒有期限。<br />指定 \<**Activation ID**\> 參數會顯示與該啟用識別碼相關聯之指定版本的啟用到期日。此作業不需要提升的權限。 |

## <a name="advanced-options"></a>進階選項

|選項 |說明 |
| - | - |
|\/cpky |在全新體驗 (OOBE) 操作期間，某些服務操作需要登錄中有產品金鑰。 **/cpky** 選項會從登錄移除產品金鑰，以防止此金鑰遭惡意程式碼盜用。<br />對於部署金鑰的零售版安裝，建議的最佳作法是執行此選項。 此選項並不需要 MAK 和 KMS 主機金鑰，因為這是這些金鑰的預設行為。 當預設行為不是從登錄清除金鑰時，其他類型的金鑰才需要此選項。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/ilc&nbsp;&lt;license_file&gt; |此選項會安裝必要參數所指定的授權檔案。 這些授權可能會安裝來做為疑難排解之用，以支援權杖型啟用，或做為手動安裝初始應用程式的一部分。<br />在此程序期間內不會驗證授權：授權驗證超出 Slmgr.vbs 的範圍。 相反地，驗證會由軟體保護服務在執行階段處理。<br />必須從已提升權限的命令提示字元視窗中執行此作業，或者必須設定 [標準使用者作業]  登錄值，才能允許未獲授權的使用者具有軟體保護服務的額外存取權。 |
|\/rilc |這個選項會重新安裝存放在 %SystemRoot%\system32\oem 及 %SystemRoot%\System32\spp\tokens 中的所有授權。 這些是在安裝期間存放的「正確」複本。<br />信任存放區中任何相符的授權都會被取代。 任何其他授權 (例如信任授權單位 (TA) 發行授權 (IL)、應用程式的授權)，都不會受到影響。<br />必須在已提升權限的命令提示字元視窗中執行此作業，或者必須設定 [標準使用者作業]  登錄值，才能允許未獲授權的使用者具有軟體保護服務的額外存取權。 |
|\/rearm |這個選項會重設啟用計時器。 **/rearm** 處理程序也會由 **sysprep /generalize** 呼叫。<br />如果 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm** 登錄項目設定為 **1**，此作業就不會執行任何動作。 如需此登錄項目的詳細資訊，請參閱[大量啟用的登錄設定](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502532(v=ws.11))。<br />必須在已提升權限的命令提示字元視窗中執行此作業，或者必須設定 [標準使用者作業]  登錄值，才能允許未獲授權的使用者具有軟體保護服務的額外存取權。 |
|\/rearm-app &lt;Application&nbsp;ID&gt; |重設指定應用程式的授權狀態。 |
|\/rearm-sku &lt;Application&nbsp;ID&gt; |重設指定 SKU 的授權狀態。 |
|\/upk&nbsp;\[&lt;Application&nbsp;ID&gt;] |這個選項會解除安裝目前 Windows 版本的產品金鑰。 重新啟動之後，系統將會變成未授權狀態，除非安裝新的產品金鑰。<br />或者，您可以使用 \<**Activation ID**\> 參數來指定不同的已安裝產品。<br />此作業必須從已提升權限的命令提示字元視窗中執行。 |
|\/dti&nbsp;\[\<Activation&nbsp;ID\>] |顯示離線啟用的安裝識別碼。 |
|\/atp &lt;Confirmation&nbsp;ID&gt; |以使用者提供的確認識別碼啟用產品。 |

## <a name="kms-client-options"></a>KMS 用戶端選項

|選項 |說明 |
| - | - |
|\/skms \<Name\[:Port]&nbsp;\|&nbsp;\:&nbsp;port\> \[\<Activation&nbsp;ID\>] |此選項指定要連線之 KMS 主機電腦的名稱和連接埠 (選擇性)。 設定此值會停用 KMS 主機的自動偵測。<br />如果 KMS 主機使用網際網路通訊協定第 6 版 (IPv6)，必須以 \<hostname\>:\<port\>格式指定位址。 IPv6 位址包含冒號 (:)，以致 Slmgr.vbs 指令碼會剖析錯誤。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/skms-domain&nbsp;&lt;FQDN&gt; \[\<Activation&nbsp;ID\>] |設定可在其中找到所有 KMS SRV 記錄的特定 DNS 網域。 如果使用 **/skms** 選項設定特定單一 KMS 主機，此設定就不會有效果。 使用此選項 (尤其在脫離的命名空間環境中) 可強制 KMS 略過 DNS 尾碼搜尋清單，並且改在指定的 DNS 網域中尋找 KMS 主機記錄。 |
|\/ckms&nbsp;\[\<Activation&nbsp;ID\>] |此選項會從登錄移除指定的 KMS 主機名稱、位址以及連接埠資訊，並還原 KMS 自動探索行為。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/skhc |此選項會啟用 KMS 主機快取 (預設值)。 在用戶端探索到運作中的 KMS 主機之後，此設定可防止網域名稱系統 (DNS) 優先順序和權數影響與主機的進一步通訊。 如果系統無法再連絡運作中的 KMS 主機，用戶端會嘗試探索新的主機。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/ckhc |此選項會停用 KMS 主機快取。 此設定指示用戶端每次嘗試 KMS 啟用時都使用 DNS 自動探索 (使用優先順序和權數時建議使用此設定)。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |

## <a name="kms-host-configuration-options"></a>KMS 主機設定選項

|選項 |說明 |
| - | - |
|\/sai&nbsp;&lt;Interval&gt; |此選項會設定未啟用的用戶端嘗試連線至 KMS 的間隔 (以分鐘為單位)。 啟用間隔必須介於 15 分鐘到 30 天之間，但建議使用預設值 (2 小時)。<br />KMS 用戶端一開始會從登錄取得此間隔，但在接收到第一個 KMS 回應之後便會切換到 KMS 設定。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/sri &lt;Interval&gt; |此選項會設定已啟用的用戶端嘗試連線至 KMS 的更新間隔 (以分鐘為單位)。 更新間隔必須介於 15 分鐘到 30 天之間。 一開始會同時在 KMS 伺服器與用戶端上設定此選項。 預設值是 10,080 分鐘 (7 天)。<br />KMS 用戶端一開始會從登錄取得此間隔，但在接收到第一個 KMS 回應之後便會切換到 KMS 設定。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/sprt&nbsp;&lt;Port&gt; |此選項會設定 KMS 主機接聽用戶端啟用要求的連接埠。 預設的 TCP 連接埠為 1688。<br />此作業必須從已提升權限的命令提示字元視窗中執行。 |
|\/sdns |啟用由 KMS 主機發佈 DNS 的功能 (預設值)。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/cdns |停用由 KMS 主機發佈 DNS 的功能。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/spri |將 KMS 優先順序設定為標準 (預設值)。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/cpri |將 KMS 優先順序設定為低。<br />使用此選項可將共同代管環境中爭用 KMS 的情況降到最低。 請注意，視哪些其他的應用程式或伺服器角色在作用中而定，這可能會導致 KMS 耗盡。 請小心使用。<br />此作業必須在已提升權限的命令提示字元視窗中執行。 |
|\/act-type \[\<Activation-Type\>] \[\<Activation&nbsp;ID\>] |此選項會在登錄中設定限制單一類型大量啟用的值。 啟用類型 **1** 會限制僅啟用 Active Directory；**2** 會限制僅使用 KMS 啟用；**3** 則會限制僅使用權杖型啟用。 **0** 選項允許任何啟用類型，而且是預設值。 |

## <a name="token-based-activation-configuration-options"></a>權杖型啟用設定選項

|選項 |說明 |
| - | - |
|/lil |列出已安裝的權杖型啟用發行授權。 |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |移除已安裝的權杖型啟用發行授權。<br />此作業必須從已提升權限的命令提示字元視窗中執行。 |
|\/stao |設定 **Token-based Activation Only** 旗標，停用自動 KMS 啟用。<br />此作業必須在已提升權限的命令提示字元視窗中執行。<br />此選項已在 Windows Server 2012 R2 和 Windows 8.1 中移除。 請改用 **/act–type** 選項。 |
|\/ctao |清除 **Token-based Activation Only** 旗標 (預設值)，啟用自動 KMS 啟用。<br />此作業必須在已提升權限的命令提示字元視窗中執行。<br />此選項已在 Windows Server 2012 R2 和 Windows 8.1 中移除。 請改用 **/act–type**</strong> 選項。 |
|\/ltc |列出可啟用已安裝軟體的有效權杖型啟用憑證。 |
|\/fta &lt;Certificate&nbsp;Thumbprint&gt; \[&lt;PIN&gt;] |使用識別的憑證強制進行權杖型啟用。 如果使用由硬體 (例如智慧卡) 所保護的憑證，則會提供選擇性的個人識別碼 (PIN) 來於不顯示 PIN 提示的情況下解除鎖定私密金鑰。 |

## <a name="active-directory-based-activation-configuration-options"></a>Active Directory 型啟用設定選項

|選項 |說明 |
| - | - |
|\/ad-activation-online &lt;Product&nbsp;Key&gt; \[\<Activation&nbsp;Object&nbsp;name\>] |使用命令提示字元中執行的認證收集 Active Directory 資料並開始 Active Directory 樹系啟用。 不需要本機系統管理員存取權。 不過，需要樹系根網域中啟用物件容器的讀取/寫入存取權。 |
|\/ad-activation-get-IID &lt;Product&nbsp;Key&gt; |此選項會以電話模式啟動 Active Directory 樹系啟用。 輸出為安裝識別碼 (IID)，如果無法使用網際網路連線，可用來透過電話啟用樹系。 在啟用電話中提供 IID 後，就會傳回用於完成啟用的 CID。 |
|\/ad-activation-apply-cid &lt;Product&nbsp;Key&gt; &lt;Confirmation&nbsp;ID&gt; \[\<Activation&nbsp;Object&nbsp;name>] |使用此選項時，請輸入啟用電話中提供的 CID 以完成啟用。 |
|\[/name:&lt;AO_Name&gt;] |或者，您可以對這些命令的任何一個附加 **/name** 選項，以指定儲存在 Active Directory 中之啟用物件的名稱。 名稱不能超過 40 個 Unicode 字元。 使用雙引號來明確定義名稱字串。<br />在 Windows Server 2012 R2 和 Windows 8.1 中，您可以在 **/ad-activation-online &lt;Product&nbsp;Key&gt;** 和 **/ad-activation-apply-cid**之後直接附加名稱，而不必使用 **/name** 選項。 |
|\/ao-list |顯示可供本機電腦使用的所有啟用物件。 |
|\/del-ao &lt;AO_DN&gt;<br />\/del-ao &lt;AO_RDN&gt; |從樹系刪除指定的啟用物件。 |

## <a name="see-also"></a>另請參閱

- [大量啟用技術參考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529%28v%3dws.11%29)
- [大量啟用概觀](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612%28v%3dws.11%29)

