---
redirect_url: /windows-server/windows-server
ms.openlocfilehash: 9ef5565af748b4dd592e71ec4bd34a2be58003d9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "75947290"
---
# <a name="windows-server-2016"></a>Windows Server 2016

這個文件庫提供專業 IT 人員評估、規劃、部署、保護和管理 Windows Server 2016 所需的資訊。

> [!Note] 
> 下一個 Windows Server 版本要改變了！ 您可以瀏覽 [Windows Server 半年度管道概觀](./get-started/semi-annual-channel-overview.md)，深入了解即將出現哪些變化。 

[![Windows Server 2016 概觀影片](media/front-page-video.png)](https://www.youtube-nocookie.com/embed/V8oF0JpDzaM)

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/what-s-new-in-windows-server-2016"> &lt;img height=145 src=&quot;media/whats-new-highlight.png&quot; alt=&quot;What&#39;s new icon&quot; title=&quot;Whats new in Windows Server 16?&quot;/&gt;</a>
        <br/>新增功能
    </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/server-basics"> &lt;img height=145 src=&quot;media/1-getstarted.png&quot; alt=&quot;get started icon&quot; title=&quot;Get Started with Windows Server 16&quot; /&gt;</a>
      <br/>開始使用 </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/administration/index"> &lt;img height=145 src=&quot;media/8-management.png&quot; alt=&quot;administer icon&quot; title=&quot;Administer Windows Server&quot; /&gt;</a>
      <br/>管理 </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/failover-clustering/failover-clustering-overview"> &lt;img height=145 src=&quot;media/3-failover.png&quot; alt=&quot;Failover clustering icon&quot; title=&quot;Windows Server Failover clustering&quot; /&gt;</a>
      <br/>容錯移轉叢集 </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/identity/identity-and-access"> &lt;img height=145 src=&quot;media/4-identity.png&quot; alt=&quot;Identity and access icon&quot; title=&quot;Windows Server Identity and Access&quot; /&gt;</a>
      <br>身分識別與存取 </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/networking/networking"> &lt;img height=145 src=&quot;media/6-networking.png&quot; alt=&quot;Networking icon&quot; title=&quot;Windows Server Networking&quot; /&gt; </a>
      <br/>網路功能 </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/remote/index"> &lt;img height=145 src=&quot;media/remote.png&quot; alt=&quot;remote icon&quot; title=&quot;Remote access and server management&quot; /&gt; </a>
      <br/>遠端存取 </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/security/security-and-assurance"> &lt;img height=145 src=&quot;media/5-security.png&quot; alt=&quot;Security icon&quot; title=&quot;Windows Server Security and Assurance&quot; /&gt; </a>
      <br/>安全性和保證 </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">&nbsp;</td>
    <td align='center' style="width:25%; border:0;"><br>
      <a href="/windows-server/storage/storage"> &lt;img height=145 src=&quot;media/7-storage.png&quot; alt=&quot;Storage icon&quot; title=&quot;Windows Server Storage&quot; /&gt; </a>
      <br/>存放裝置 </td>
   <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/virtualization/virtualization"> &lt;img height=145 src=&quot;media/virtualization.png&quot; alt=&quot;virtualization icon&quot; title=&quot;Windows Server Virtualization&quot; /&gt;</a>
      <br/>虛擬化 </td>
    <td align='center' style="width:25%; border:0;">[https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/](&nbsp;) </td>
  </tr>
</table>

<br/>

> [!Note] 
> 若要體驗 Windows Server 2016 中提供的最新功能，您可以瀏覽 [Windows Server 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)來下載評估版。 


## <a name="windows-server-2016-editions"></a>Windows Server 2016 版本

Standard、Datacenter 和 Essentials 版本中都提供 Windows Server 2016。 Windows Server 2016 Datacenter 包含無限制的虛擬化權限，以及建置軟體定義資料中心的新功能。 Windows Server 2016 Standard 提供具有有限虛擬化權限的企業級功能。 Windows Server Essentials 是理想的連接雲端的第一部伺服器。 它有自己的[大量文件](https://go.microsoft.com/fwlink/?LinkID=827171)：這裡的內容著重於 Standard 和 Datacenter 版本。 下表簡單地摘要說明 Standard 與 Datacenter 版本之間的主要差異︰

|功能|Datacenter|Standard|  
|-------------------|----------|-----------------------|  
|Windows Server 的核心功能| 是| 是|
|OSE/Hyper-V 容器|無限制|   2|
|Windows Server 容器|無限制|   無限制|
|主機守護者服務| 是| 是|
|Nano 伺服器安裝選項| 是| 是|
|儲存體功能 (包括儲存空間直接存取和儲存體複本)| 是| 否|
|受防護的虛擬機器| 是| 否|
|軟體定義網路基礎結構 (網路控制器、軟體負載平衡器和多租用戶閘道)| 是| 否|

如需詳細資訊，請參閱 [Pricing and licensing for Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server-pricing) (Windows Server 2016 的價格和授權) 和 [Compare features in Windows Server versions](https://www.microsoft.com/cloud-platform/windows-server-comparison) (比較 Windows Server 版本的功能)。

## <a name="installation-options"></a>安裝選項

Standard 和 Datacenter 版本都提供三個安裝選項︰

- [Server Core]  可減少磁碟上所需的空間、潛在的受攻擊面，以及特別是服務需求。 除非有額外使用者介面元素和圖形化管理工具的特別需求，否則這是**建議**選項。
- [含桌面體驗的伺服器]  會安裝標準使用者介面和所有工具，包括需要在 Windows Server 2012 R2 中另外安裝的用戶體驗功能。 伺服器角色及功能可由伺服器管理員或其他方法安裝。
- [Nano 伺服器]  是一個遠端管理的伺服器作業系統，已針對私人雲端和資料中心最佳化。 它類似於 Server Core 模式的 Windows Server，但明顯較小、沒有本機登入功能，而且只支援 64 位元應用程式、工具和代理程式。 比起其他選項，它佔用的磁碟空間更少、設定的速度明顯地更快，而且所需的更新和重新啟動次數更少。

>[!Note]
> 與 Windows Server 某些更早的版本不同，您無法在安裝完成後在 Server Core 與桌面體驗伺服器之間進行轉換。 例如，如果您安裝 Server Core，並稍後決定使用桌面體驗伺服器，您應該執行全新安裝 (反之亦然)。


現在您已知道哪一個版本和安裝選項最適合您，請按一下下方開始使用 Windows Server 2016。
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:33%; border:0;">
      <a  href="/windows-server/get-started/getting-started-with-nano-server"> <img width="175" src="media/nano.png" alt="Icon representing Nano server" title="Nano Server - 最輕量型" /><br/>Nano Server - <br/>最輕量型</a>
    </td>
    <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-core"> <img width="175" src="media/servercore.png" alt="Icon representing the Server Core installation" title="Server Core - 建議使用" /><br/>Server Core - <br/>建議使用</a></td>
   <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-with-desktop-experience"><img width="175" src="media/desktop.png" alt="Icon representing the full desktop experience installation option for Windows Server" title="桌面體驗 - 完整體驗" /><br/>桌面體驗 - <br/>完整介面</a></td>
  </tr>
</table>

## <a name="windows-server-software-defined-datacenter-sddc"></a>Windows Server 軟體定義資料中心 (SDDC)

虛擬化儲存體、網路功能、安全性與管理技術是 Windows Server 軟體定義資料中心 (SDDC) 的建置組塊。
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:10%; border:0;"></td>
    <td align='center' style="width:50%; border:0;"><a href="/windows-server/sddc"><img width="400" src="media/sddc/WS16-heading.png" alt="Icon representing SDDC" title="Windows Server 軟體定義資料中心 (SDDC)" /><br/>Windows Server 軟體定義資料中心 (SDDC)</a></td>
    <td align='center' style="width:10%; border:0;"></td>
  </tr>
</table>
