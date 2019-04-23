---
title: 開始使用安裝與開機事件集合
description: 設定安裝與開機事件集合收集器及目標
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: 781ed17fb07d2aecd4bb0b7bc672056096ab8060
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837059"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>開始使用安裝與開機事件集合

>適用於：Windows Server

  
## <a name="overview"></a>總覽  
「安裝與開機事件集合」是 Windows Server 2016 中的新功能，可讓您指定「收集器」電腦，這部電腦會收集其他電腦開機或進行設定程序時所發生的各種重要事件。 您隨後可以使用事件檢視器、訊息分析器、Wevtutil 或 Windows PowerShell Cmdlet 來分析收集到的事件。  
  
先前，由於在電腦經過設定之前，收集事件所需的基礎結構並不存在，因此無法監視這些事件。 您可以監視的安裝與開機事件類型包括：  
  
-   載入核心模組和驅動程式  
  
-   列舉裝置和初始化其驅動程式 (包括像 CPU 類型這樣的「裝置」)  
  
-   驗證和掛接檔案系統  
  
-   啟動可執行檔  
  
-   開始和完成系統更新  
  
-   在系統可供登入的時間點上會建立與網域控制站的連線、完成服務啟動，以及提供網路共用。  
  
收集器電腦必須正在執行 Windows Server 2016 (可以在具備桌面體驗的伺服器或是 Server Core 模式的伺服器中)。 目標電腦必須正在執行 Windows 10 或 Windows Server 2016。 您也可以在裝載於**未**執行 Windows Server 2016 之電腦中的虛擬機器上執行這項服務。 下列虛擬化收集器與目標電腦的組合是已知可正常運作的組合：  
  
|虛擬化主機|收集器虛擬機器|目標虛擬機器|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|是|是|  
|Windows 10|是|是|  
|Windows Server 2016|是|是|  
|Windows Server 2012 R2|是|否|  
  
## <a name="installing-the-collector-service"></a>安裝收集器服務  
從 Windows Server 2016 開始，事件收集器服務是做為選用功能來提供。 在此版本中，您可以在提升權限的 Windows PowerShell 命令提示字元中透過此命令，使用 DISM.exe 來進行安裝：  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
此命令會建立名為 BootEventCollector 的服務，並使用空白設定檔啟動此服務。  
  
檢查 `get-service -displayname *boot*` 以確認安裝成功。 開機事件收集器應該正在執行。 它會在網路服務帳戶下執行，並在 **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config** 中建立空白設定檔 (Active.xml)。  
  
您也可以使用伺服器管理員中的 [新增角色及功能精靈] 來安裝「安裝與開機事件集合」服務。  
  
## <a name="configuration"></a>組態  
您需要設定兩個項目來收集安裝與開機事件。  
  
-   在傳送事件的目標電腦 (也就是，您要監視其安裝與開機的電腦)，啟用 KDNET/EVENT-NET 傳輸並啟用事件轉送。  
  
-   在收集器電腦上，指定要接受來自哪些電腦的事件及其儲存位置。  
  
> [!NOTE]  
> 您無法將電腦設定為傳送啟動或開機事件給其本身。 但要是您想監視兩部電腦，則可以設定彼此傳送事件給對方。  
  
### <a name="configuring-a-target-computer"></a>設定目標電腦  
在每部目標電腦上，先啟用 KDNET/EVENT-NET 傳輸，再啟用透過此傳輸傳送 ETW 事件，然後重新啟動目標電腦。 EVENT-NET 是類似於 KDNET (核心偵錯通訊協定) 的核心內傳輸通訊協定。 EVENT-NET 只會傳輸事件，而不允許偵錯工具存取。 這兩個通訊協定彼此互斥；您一次只能啟用其中一個。  
  
您可以啟用從遠端 (使用 Windows PowerShell) 或在本機事件傳輸。  
  
##### <a name="to-enable-event-transport-remotely"></a>若要從遠端啟用事件傳輸  
  
1.  如果您已設定對目標電腦的 Windows PowerShell 遠端執行功能，請跳至步驟 3。 如果沒有，則在目標電腦上開啟命令提示字元，並執行下列命令：  
  
    winrm quickconfig  
  
2.  回應提示，然後重新啟動目標電腦。 如果目標電腦與收集器電腦不在相同網域中，您可能需要將這些電腦定義為受信任的主機。 請這樣做：  
  
3.  在收集器電腦上，執行下列任一命令：  
  
    -   在 Windows PowerShell 提示字元中： `Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`，後面接著`Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true`其中\<target1 > 等名稱或 IP 位址的目標電腦。  
  
    -   或在命令提示字元中： **winrm 設定 winrm/config/client @{TrustedHosts ="\<target1 >，\<target2 >，...";AllowUnencrypted ="true"}**  
  
        > [!IMPORTANT]  
        > 這會設定未加密的通訊，因此不要在實驗室環境外部執行此命令。  
  
4.  移至收集器電腦並執行下列其中一個 Windows PowerShell 命令，以測試遠端連線：  
  
    如果目標電腦與收集器電腦位於相同網域中，執行 `New-PSSession -Computer <target> | Remove-PSSession`  
  
    如果目標電腦不在相同網域中，則執行 `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`，這會提示您提供認證。  
  
    如果命令沒有傳回任何項目，遠端執行功能即已成功。  
  
5.  在目標電腦上，開啟提升權限的 Windows PowerShell 命令提示字元並執行此命令：  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    < Target_name > 以下是在目標電腦的名稱\<ip > 是收集器電腦的 IP 位址。 \<連接埠 > 是收集器執行所在的連接埠號碼。 金鑰 <a.b.c.d> 是通訊所需的加密金鑰，包含四個以點分隔的英數字元字串。 收集器電腦上會使用這同一個金鑰。 如果您沒有輸入金鑰，系統會產生隨機金鑰；收集器電腦將需要此金鑰，因此請記下。  
  
6.  如果您已設定收集器電腦，請以新目標電腦的資訊來更新收集器電腦上的設定檔。 如需詳細資訊，請參閱「設定收集器電腦」一節。  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>若要在目標電腦本機啟用事件傳輸  
  
1.  啟動提升權限的命令提示字元，然後執行下列命令：  
  
    **bcdedit /event [是]**  
  
    **bcdedit /eventsettings net hostip:1.2.3.4 連接埠： 50000 key:a.b.c.d**  
  
    其中 "1.2.3.4" 是範例，請將此取代為收集器電腦的 IP 位址。 此外，也將 "50000" 取代為收集器用來執行的連接埠號碼，並將 "a.b.c.d" 取代為通訊所需的加密金鑰。 收集器電腦上會使用這同一個金鑰。 如果您沒有輸入金鑰，系統會產生隨機金鑰；收集器電腦將需要此金鑰，因此請記下。  
  
2.  如果您已設定收集器電腦，請以新目標電腦的資訊來更新收集器電腦上的設定檔。 如需詳細資訊，請參閱「設定收集器電腦」一節。  
  
**啟用事件傳輸本身時，您必須啟用系統來實際傳送到該傳輸的 ETW 事件。**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>若要啟用透過傳輸從遠端傳送 ETW 事件的功能  
  
1.  在收集器電腦上，開啟提升權限的 Windows PowerShell 命令提示字元。  
  
2.  執行 `Enable-SbecAutologger -ComputerName <target_name>`，其中 <target_name> 是目標電腦的名稱。  
  
如果您無法設定 Windows PowerShell 遠端執行功能，則可以一直在目標電腦上啟用事件傳送。  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>若要啟用透過傳輸在本機傳送 ETW 事件的功能  
  
1.  在目標電腦上，啟動 Regedit.exe 並尋找此登錄機碼：  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. 各種記錄工作階段會列為此機碼底下的子機碼。 **\[Setup Platform\]**、**\[NT Kernel Logger\]** 和 **\[Microsoft-Windows-Setup\]** 都可能選擇來與「安裝與開機事件集合」搭配使用，但建議的選項是 **\[EventLog-System\]**。 這些機碼在[設定和啟動自動記錄工具工作階段](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx)中有詳細說明。  
  
2.  在 EventLog-System 機碼中，將 **LogFileMode** 的值從 **0x10000180** 變更為 **0x10080180**。 如需有關這些設定詳細資料的詳細資料，請參閱[記錄模式常數](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx)。  
  
3.  或者，您也可以啟用轉送錯誤檢查資料給收集器電腦的功能。 若要這樣做，請尋找登錄機碼 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager，並建立值為 **0x1** 的 **Debug Print Filter** 機碼。  
  
4.  重新啟動目標電腦。  
  
### <a name="choosing-the-network-adapter"></a>選擇網路介面卡  
如果目標電腦有多個網路介面卡，KDNET 驅動程式將會選擇列出的第一個支援的介面卡。 您可以使用下列步驟指定要用於轉送安裝程式事件的特定網路介面卡：  
  
##### <a name="to-specify-a-network-adapter"></a>若要指定網路介面卡  
  
1.  在目標電腦上，開啟 \[裝置管理員\]、展開 **\[網路介面卡\]**、尋找您要使用的網路介面卡，然後在其上按一下滑鼠右鍵。  
  
2.  在開啟的功能表，按一下**屬性**，然後按一下**詳細資料** 索引標籤。展開的功能表中**屬性**欄位中，捲動以尋找**位置資訊**（清單是可能不按照字母順序），然後按一下它。 值會是格式的字串**PCI 匯流排 X，Y 的裝置、 函式 Z**。請記下的 X.Y.Z;這些是您需要下列的命令匯流排參數。  
  
3.  執行下列任一命令：  
  
    從提升權限的 Windows PowerShell 提示字元中： `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    從提升權限的命令提示字元：**bcdedit /eventsettings net hostip:aaa port:50000 key:bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>驗證目標電腦設定  
若要檢查目標電腦上的設定，請開啟提升權限的命令提示字元並執行 **bcdedit /enum**。 完成此命令後，接著執行 **bcdedit /eventsettings**。 您可以仔細檢查下列值：  
  
-   Key  
  
-   Debugtype = NET  
  
-   Hostip =\<收集器的 IP 位址 >  
  
-   連接埠 =\<連接埠的號碼指定收集器來使用 >  
  
-   DHCP = yes  
  
由於 **/debug** 和 **/event** 彼此互斥，因此也要確認您已啟用 **bcdedit /event**。 您只能執行這一個或另一個。 同樣地，您不能將 /eventsettings 與 /debug，或是 /dbgsettings 與 /event 混用。  
  
另請注意，如果您將事件收集設定至序列埠，則此作業無法運作。  
  
### <a name="configuring-the-collector-computer"></a>設定收集器電腦  
收集器服務會接收事件，並將這些事件儲存在 ETL 檔案中。 這些 ETL 檔案然後可以由事件檢視器、訊息分析程式、Wevtutil 和 Windows PowerShell Cmdlet 等其他工具讀取。  
  
由於 ETW 格式不允許您指定目標電腦名稱，必須將每一部目標電腦的事件儲存到不同的檔案。 顯示工具可能會顯示電腦名稱，但這是執行此工具的電腦名稱。  
  
更確切地說，會將一個循環索引序列的 ETL 檔案指派給每部目標電腦。 每個檔案名稱都包含從 000 到您所設定之最大值 (最多 999) 的索引。 當檔案達到設定的大小上限時，就會切換到下一個檔案寫入事件。 檔案到了可能的最高索引值之後，將會切換回檔案索引 000。 如此一來，將檔案都將自動回收，並限制磁碟空間使用量。 您也可以設定其他外部保留原則，進一步限制磁碟使用量；例如，您可以刪除超過設定天數的檔案。  
  
收集的 ETL 檔案通常保留在目錄 **c:\ProgramData\Microsoft\BootEventCollector\Etl** (這可能還有其他子目錄)。 您可依上次修改時間將記錄檔排序，以便尋找最新的檔案。 也有狀態記錄檔 (通常位於 **c:\ProgramData\Microsoft\BootEventCollector\Logs**)，每當收集器切換寫入新檔案時，就會進行記錄。  
  
此外還有收集器記錄檔，這會記錄關於收集器本身的資訊。 您可依 ETW 格式建立此記錄 (若使用此格式，則會向 Windows 記錄服務通報事件；這是預設值)，或將其保留於檔案 (通常在 **c:\ProgramData\Microsoft\BootEventCollector\Logs** 中)。 如果您想要啟用產生大量資料的詳細資訊模式，使用檔案可能會很有用。 您也可以從命令列執行收集器，將記錄設定成寫入至標準輸出。  
  
**建立收集器組態檔**  
  
當您啟用服務時，會建立三個 XML 組態檔，並儲存在**c:\ProgramData\Microsoft\BootEventCollector\Config**:  
  
-   **Active.xml**：這個檔案包含收集器服務的目前作用中設定。  安裝之後，這個檔案的內容與 Empty.xml 的相同。 設定新的收集器設定時，您會將它儲存到這個檔案。  
  
-   **Empty.xml**：這個檔案包含至少需要的以其預設值設定的設定元素。 這不會啟用任何收集作業；只是允許收集器服務在閒置模式下啟動。  
  
-   **Example.xml**：這個檔案會提供可能的設定元素的範例及說明。  
  
**選擇 檔案大小限制**  
  
您必須做的其中一項決定是設定檔案大小限制。 最佳的檔案大小限制取決於事件及可用磁碟空間的預計數量。 從清除舊資料的觀點來看，檔案愈小就愈方便。 不過，每個檔案都具有 64 KB 的標頭的額外負荷，而且可能很不方便讀取許多檔案，以便取得結合的歷程記錄。 絕對最小的檔案大小限制為 256 KB。 合理可行的檔案大小限制應在 1 MB 以上，而 10 MB 可能是一般適當的值。 如果您預計會有許多事件，較高的限制可能比較合理。  
  
關於設定檔，有幾個詳細資料要牢記：  
  
-   目標電腦位址。 您可以使用其 IPv4 位址、MAC 位址或 SMBIOS GUID。 選擇要使用的位址時，請記住下列因素：  
  
    -   IPv4 位址最適合搭配靜態指派的 IP 位址使用。 不過，即使是靜態 IP 位址，也必須透過 DHCP 才能使用。  
  
    -   MAC 位址或 SMBIOS GUID 在已預先得知，而 IP 位址是動態指派的情況下，比較方便使用。  
  
    -   EVENT-NET 通訊協定不支援 IPv6 位址。  
  
    -   您可以指定多種方式來識別電腦。 例如，即將更換實體硬體時，您可以輸入同時舊的和新的 MAC 位址，兩者都會被接受。  
  
-   用於與收集器電腦通訊的加密金鑰  
  
-   目標電腦的名稱。 您可以使用 IP 位址、主機名稱或任何其他名稱做為電腦名稱。  
  
-   所要使用之 ETL 檔案的名稱，以及其循環索引序列大小設定  
  
##### <a name="to-create-the-configuration-file"></a>若要建立設定檔  
  
1.  開啟提升權限的 Windows PowerShell 命令提示字元，並將目錄變更至 %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config。  
  
2.  輸入 `notepad .\newconfig.xml` 並按 ENTER。  
  
3.  將此範例設定複製到 [記事本] 視窗：  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > 根節點是\<收集器 >。 其屬性會指定設定檔語法的版本以及狀態記錄檔的名稱。  
    >   
    > \<常見 > 項目分組在一起的多個指定的常見的組態項目，非常像使用者群組可用來指定多個使用者的一般權限的目標。  
    >   
    > \<Collectorport > 項目會定義收集器會接聽內送資料的 UDP 連接埠號碼。 這與 Bcdedit 目標組態步驟所指定的連接埠相同。 收集器僅支援一個連接埠，而所有的目標都必須連線至同一個連接埠。  
    >   
    > \<轉寄站 > 項目會指定從目標電腦收到的 ETW 事件轉送的方式。 只有一種類型的轉寄站，這會將事件寫入 ETL 檔案。 參數會指定檔案名稱模式、循環索引序列中每個檔案的大小限制，以及每部電腦的循環索引序列大小。 設定 "toxml" 會指定，收到 ETW 事件時，以二進位格式將其寫入，而不轉換為 XML。 如需有關決定是否將事件轉換為 XML 的詳細資訊，請參閱「XML 事件轉換」一節。 檔案名稱模式包含下列替代位置：{computer} 代表電腦名稱，而 {#3} 代表循環索引序列中的檔案索引。  
    >   
    > 在此範例檔案中，以定義兩個目標電腦\<目標 > 項目。 每個定義會指定 IP 位址\<ipv4 >，但您也可以使用 MAC 位址 (比方說，< mac 值 ="11:22:33:44:55:66 」\/> 或 < mac value="11-22-33-44-55-66"\/>) 或 SMBIOS GUID （適用於範例中，< guid 值 ="{269076F9-4B77-46E1-B03B-CA5003775B88}"\/>) 來識別目標電腦。 另請記下加密金鑰 (與之前使用 Bcdedit 在目標電腦上所指定或產生的一樣) 和電腦名稱。  
  
4.  輸入為不同的每一部目標電腦的詳細資料\<目標 > 組態檔，然後再儲存 Newconfig.xml 並關閉 [記事本] 的項目。  
  
5.  使用 `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` 套用新的設定。 輸出應該會傳回顯示 "true" 的 [成功] 欄位。 如果您收到其他結果，請參閱本主題的「疑難排解」一節。  
  
您隨時都可以使用 `(Get-SbecActiveConfig).text` 來檢查目前的作用中設定。  
  
您可以使用 `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result` 對設定檔執行驗證檢查。  
  
雖然套用新設定的 Windows PowerShell 命令會自動更新服務，並不需要您重新啟動服務，但是您隨時都可以使用下列任一命令自行將服務重新啟動：  
  
-   使用 Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   在一般命令提示字元：**sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>將 Nano 伺服器設定為目標電腦  
Nano 伺服器提供的基本介面有時使其發生的問題難以診斷。 您可以將 Nano 伺服器映像設定成自動參與「安裝與開機事件集合」，並傳送診斷資料給收集器電腦而不需要您進一步介入。 若要這樣做，請執行下列步驟：  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>若要將 Nano 伺服器設定為目標電腦  
  
1.  建立基本 Nano 伺服器映像。 如需詳細資訊，請參閱[開始使用 Nano 伺服器](https://technet.microsoft.com/library/mt126167.aspx)。  
  
2.  設定收集器電腦，如本主題「設定收集器電腦」一節所示。  
  
3.  新增自動記錄工具登錄機碼以啟用傳送診斷訊息功能。 若要這樣做，您可以掛接步驟 1 所建立的 Nano Server VHD、載入登錄區，然後新增特定登錄機碼。 在本範例中，Nano 伺服器映像位於 C:\NanoServer；您的路徑可能不同，請相應調整步驟。  
  
    1.  在收集器電腦上，複製 **.\Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** 資料夾，然後在您要用來修改 Nano Server VHD 的電腦上，將該資料夾貼到 **.\Windows\System32\WindowsPowerShell\v1.0\Modules** 目錄中。  
  
    2.  以提升的權限啟動 Windows PowerShell 主控台，並執行 `Import-Module BootEventCollector`。  
  
    3.  更新 Nano Server VHD 登錄以啟用自動記錄工具。 若要這樣做，請執行 `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`。 這會新增最常見安裝與開機事件的基本清單；您可以在[控制事件追蹤工作階段](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx)上研究其他事件。  
  
4.  更新 Nano 伺服器映像中的 BCD 設定以啟用事件旗標，並設定收集器電腦以確保診斷事件傳送至正確的伺服器。 記下您在收集器 Active.XML 檔案中設定的收集器電腦 IPv4 位址、TCP 連接埠及加密金鑰 (已於本主題其他位置述及) 。 Windows PowerShell 主控台中，使用提高的權限，使用下列命令： `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  將 Nano 伺服器的 IPv4 位址範圍、特定 IPv4 位址或 MAC 位址新增至收集器電腦上的 Active.XML 檔案 (請參閱本主題「設定收集器電腦」一節)，更新收集器電腦以接收 Nano 伺服器電腦所傳送的事件。  
  
## <a name="starting-the-event-collector-service"></a>啟動事件收集器服務  
將有效的設定檔儲存在收集器電腦並設定好目標電腦之後，只要重新啟動目標電腦，就會立即對收集器進行連線並開始收集事件。  
  
收集器服務本身的記錄 (這不同於服務所收集的安裝與開機資料) 可以在 Microsoft-Windows-BootEvent-Collector/Admin 底下找到。 如需事件的圖形化介面，請使用事件檢視器。 建立新的檢視；展開 **\[應用程式及服務記錄檔\]**，然後展開 **\[Microsoft\]** 和 **\[Windows\]**。 找到 **\[BootEvent-Collector\]**、將其展開，然後尋找 **\[Admin\]**。  

-   使用 Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   在一般命令提示字元：**wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>疑難排解  
  
### <a name="troubleshooting-installation-of-the-feature"></a>功能安裝疑難排解  
  
||錯誤|錯誤描述|徵兆|潛在問題|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|功能名稱選項在此內容中無法辨識||- 如果拼錯功能名稱，即可能發生此情況。 確認您的拼字正確無誤，然後再試一次。<br />- 確認此功能可在您使用中的作業系統版本上使用。 在 Windows PowerShell 中，執行 **dism /online /get-features &#124; ?{$_ -match "boot"}**。 如果沒有傳回相符項目，您可能執行的是不支援此功能的版本。|  
|Dism.exe|0x800f080c|功能\<名稱 > 不明。||與上述相同|  
  
### <a name="troubleshooting-the-collector"></a>收集器疑難排解  
  
**記錄：**  
收集器將其本身的事件記錄為 ETW 提供者 Microsoft-Windows-BootEvent-Collector。 對收集器發生的問題進行疑難排解時，這是您應該尋找的第一個地方。 您可以在事件檢視器的 [應用程式及服務記錄檔] > [Microsoft] > [Windows] > [BootEvent-Collector] > [Admin] 下找到它們，或者也可以在命令視窗中使用下列任一命令來讀取：  
  
在一般命令提示字元：**wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
在 Windows PowerShell 命令提示字元：`Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (您可附加 `-Oldest`，以傳回依時間先後順序第一個列出最早事件的清單)  
  
您可以從「錯誤」到「警告」、「資訊」(預設)、「詳細資訊」及「偵測」，調整記錄檔的詳細等級。 比「資訊」更詳細的等級有助於診斷未連線目標電腦發生的問題，但可能會產生大量的資料，因此要小心使用。  
  
設定最小記錄層級\<收集器 > 組態檔的項目。 例如： < 行程 configVersionMajor ="1"minlog\="verbose">。  
  
詳細資料等級會記錄每個在處理時所收到封包的記錄。 偵錯等級會新增更多處理詳細資料，也會傾印所有收到的 ETW 封包的內容。  
  
使用偵錯等級時，將記錄寫入檔案而不嘗試在一般記錄系統中檢視記錄，可能會很有用。 若要這樣做，請新增其他項目中的\<收集器 > 組態檔的項目：  
  
<collector configVersionMajor="1" minlog="debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
 **若要疑難排解收集器的建議的方法：**  
   
 1. 首先，確認收集器已接收到目標的連線 (只有在目標開始傳送訊息時，才會建立檔案)   
```  
Get-SbecForwarding  
```  
如果傳回的訊息應表示有來自此目標的連線，則問題可能出在自動記錄工具設定。 如果沒有傳回任何訊息，問題在於要開始使用的 KDNET 連線。 若要診斷 KDNET 連線問題，請嘗試檢查兩端的連線 (亦即來自收集器和來自目標的連線)。  
  
2. 若要確認擴充的診斷從收集器，將此選項可\<收集器 > 組態檔的項目：  
\<collector ... minlog="verbose">  
這會允許提供有關每個收到封包的訊息。  
3. 檢查是否任何封包都收到了。 或者，您可能還需要在詳細資訊模式下將記錄直接寫入記錄檔，而不是透過 ETW 來查看。 若要這樣做，請將此選項可\<收集器 > 組態檔的項目：  
\<collector ... minlog="verbose" log="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
4. 檢查事件記錄檔是否有關於所收到封包的資訊。 檢查是否任何封包都收到了。 如果封包已收到但不正確，請檢查事件訊息以取得詳細資料。  
5. KDNET 會從目標端將一些診斷資訊寫入至登錄。 查詢   
**HKLM\SYSTEM\CurrentControlSet\Services\kdnet** 是否有訊息。  
  KdInitStatus (DWORD) 在成功時為 0，而錯誤時，則顯示錯誤碼  
  KdInitErrorString = 錯誤的說明 (如果沒有錯誤，也會包含參考訊息)  
  
6. 對目標執行 Ipconfig.exe，並檢查其回報的裝置名稱。 如果 KDNET 正確載入，裝置名稱應該類似 "kdnic" 這樣，而不是原始廠商的介面卡名稱。  
7. 檢查是否已為目標設定 DHCP。 KDNET 絕對需要 DHCP。  
8. 確認收集器與目標位於相同網路。 如果不是，請檢查是否正確設定路由，特別是 DHCP 的預設閘道設定。  
  
  
**連線狀態**  
  
您可以使用 `Get-SbecForwarding` 來檢查目前已建立連線的清單以及資料要轉送到的位置。  
  
您還可以使用 `Get-SbecHistory` 來取得最新的連線狀態變更歷程記錄。  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>有關設定新設定的疑難排解  
如果您使用 Windows PowerShell 命令 `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` 來套用設定，則變數 `$result` 將包含部署的相關資訊。 您可以查詢此變數以從中取得其他資訊：  
  
使用 `$result.ErrorString` 取得錯誤的相關資訊。 如果其中報告了任何錯誤，則不會套用新設定，而舊設定將保持不變。  
  
使用 `$result.WarningString` 取得警告。  
  
使用 `$result.InfoString` 取得有關組態設定詳細資料的資訊。  
  
您可以使用 `$result | fl *` 取得完整的結果。  
或者，如果您不想要將結果儲存在變數中，也可以使用 `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`。  
  
### <a name="troubleshooting-target-computers"></a>目標電腦疑難排解  
  
||錯誤|錯誤描述|徵兆|潛在問題|  
|-|---------|---------------------|-----------|---------------------|  
|目標電腦||目標未連線至收集器||- 目標電腦無法在進行設定之後重新啟動。 重新啟動目標電腦。<br />- 目標電腦的 BCD 設定不正確。 檢查「驗證目標電腦設定」一節中的設定。 視需要進行修正，然後再重新啟動目標電腦。<br />- KDNET/EVENT-NET 驅動程式無法連線至網路介面卡，或已連線至錯誤的網路介面卡。 在 Windows PowerShell 中，執行 `gwmi Win32_NetworkAdapter`，並檢查輸出是否有一個 ServiceName 為 **kdnic** 的介面卡。 如果選取了錯誤的網路介面卡，請重新執行「若要指定網路介面卡」中的步驟。 如果完全沒有出現網路介面卡，可能是驅動程式不支援您的任何網路介面卡。<br>**另請參閱**前述「建議的收集器疑難排解方法」，特別是步驟 5 到 8。|  
|收集器||移轉裝載收集器的 VM 之後，我再也無法查看事件。||確認收集器電腦的 IP 位址沒有變更。 如果已變更，請檢閱「若要啟用透過傳輸從遠端傳送 ETW 事件的功能」。|  
|收集器||未建立 ETL 檔案。|`Get-SbecForwarding` 顯示已連線的目標，而未產生錯誤，但不是會建立 ETL 檔案。|目標電腦可能尚未傳送任何資料，只有在接收到資料時，才會建立 ETL 檔案。|  
|收集器||有事件未顯示在 ETL 檔案中。|目標電腦已傳送事件，但是當使用訊息分析程式的事件檢視器讀取 ETL 檔案時，事件卻不存在。|- 事件可能仍然在緩衝區中。 除非收集到 64 KB 緩衝區已滿，或因 10-15 秒沒有新事件而發生逾時之前，系統不會將事件寫入 ETL 檔案。 等待逾時過期，或是使用 `Save-SbecInstance` 排清緩衝區。<br />- 事件資訊清單無法在收集器電腦上，或在執行事件檢視器或訊息分析程式的電腦上使用。  在此情況下，收集器可能無法處理事件 (檢查收集器記錄檔)，或檢視器可能無法顯示事件。  最好是將所有的資訊清單都安裝在收集器電腦，並且先將更新安裝在收集器電腦上，再安裝到目標電腦。|
