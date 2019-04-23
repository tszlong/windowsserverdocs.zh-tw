---
title: 對 DirectAccess 進行疑難排解
description: 本主題提供疑難排解 Windows Server 2016 中的 DirectAccess 部署的相關資訊。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ec725eea286c359461b0f4a7b8763b97464e7067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867089"
---
# <a name="troubleshooting-directaccess"></a>對 DirectAccess 進行疑難排解

>適用於：Windows Server （半年通道），Windows Server 2016

遵循下列步驟來針對遠端存取 (DirectAccess) 問題進行疑難排解。  
  
|||  
|-|-|  
|**問題**|**解決方法**|  
|遠端存取管理主控台會無法顯示 DirectAccess 設定|**若要還原遺失的組態資訊**<br />-如果您正在疑難排解多站台部署，請確定使用的最接近的進入點網域控制站。<br />-使用**Get DAEntrypointDC** cmdlet 來擷取最接近的進入點網域控制站的名稱。 如果網域控制站不在執行中，使用**Set-daentrypointdc**指向另一個網域控制站的 cmdlet。<br />-執行**gpresult**從提升權限的命令提示字元，以確保伺服器收到 DirectAccess 群組原則物件在伺服器上。<br />-啟用使用者介面 (UI) 的記錄。<br />-使用下列命令來啟動 Windows PowerShell 記錄：<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-關閉並重新開啟使用者介面。<br />-停用 Windows Powershell 記錄。 收集的事件追蹤記錄檔。 此外，收集從所有記錄檔 **%windir%/tracing**資料夾。|  
|套用 DirectAccess 設定失敗|**若要重新整理的 DirectAccess 設定**<br />-如果您正在疑難排解多站台部署，請確定使用的最接近的進入點網域控制站。<br />-使用**Get DAEntrypointDC** cmdlet 來擷取最接近的進入點網域控制站的名稱。 如果網域控制站不在執行中，使用**Set-daentrypointdc**指向另一個網域控制站的 cmdlet。<br />-使用下列命令來啟動 Windows Powershell 記錄：<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-按一下**套用**。<br />-在失敗發生之後，停用 Windows Powershell 記錄，並收集事件追蹤記錄檔。|  
|設定 DirectAccess，但用戶端無法連接到內部部署資源|**用戶端連線問題進行疑難排解**<br />-按一下**操作狀態**在遠端存取管理主控台中，索引標籤，並確定所有元件，會都顯示綠色圖示。 如果沒有，請查看錯誤詳細資料，並遵循的解決步驟。<br />-執行遠端存取伺服器 Best Practices Analyzer (BPA)。 如果有任何警告或錯誤，請依照若要解決此問題的解決步驟。|  
|遇到多站台的設定 （例如，啟用多站台，其中新增進入點，或設定做為進入點網域控制站） 的相關問題|請依照下列中的步驟[疑難排解多站台部署](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx)。|  
|組態狀態磚的儀表板上顯示警告或錯誤|請依照下列中的步驟[監視遠端存取伺服器的設定散佈狀態](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx)。|  
|若要設定負載平衡 （例如，當您啟用負載平衡，或有問題時您新增或移除叢集中的伺服器時，組態會失敗） 的相關遇到問題|如果您已啟用負載平衡，或新增節點，並設定重新整理，當您按下**套用**，但叢集未正確構成在伺服器上，執行下列命令： **cmd.exe /c"reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d""0xffffffff"""** 來收集使用者介面會記錄新的伺服器上。|  
|作業狀態會顯示錯誤或警告後若要更正這種情況的下列步驟|如果作業狀態會顯示不正確的資訊 （例如之後錯誤甚至您修正它們）：<br /><br />-   Enable the registry key **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d ""5"" "**.<br />-重新整理作業狀態和從中收集記錄檔 **%windir%/tracing**。|  
|Windows 8 和更新版本的 DirectAccess 用戶端電腦會回報 「 沒有網際網路 」 與 DirectAccess 連線的狀態和網路連線狀態指示器 (NCSI) 報告的連線能力有限。|這可能是強制通道中的 DirectAccess 設定已啟用，並基於此原因，要使用只有 IPHTTPS。 若要解決此問題，您可以建立及設定 proxy 伺服器。 NCSI 然後會使用 proxy 伺服器來執行網際網路連線能力檢查。 建議您新增靜態 proxy 名稱解析原則表格 (NRPT) 來使用下列程序。<br /><br />您在此程序中執行命令之前，請確定以適合您的部署的值取代所有的網域名稱、 電腦名稱和其他 Windows PowerShell 命令變數。<br /><br />**設定的 NRPT 規則的靜態 proxy**<br />1.顯示 「。 」NRPT 規則： `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.記下名稱 (GUID) 的 「。 」NRPT 規則。 (GUID) 名稱的開頭應**DA-{。}**<br />3.設定的 proxy"。"NRPT 規則**proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.顯示 「。 」透過再次執行的 NRPT 規則`Get-DnsClientNrptRule`，並確認**ProxyFQDN:port**現在已正確設定。<br />5.執行重新整理群組原則`gpupdate /force`DirectAccess 用戶端上用戶端會在內部連線，然後顯示使用 NRPT `Get-DnsClientNrptPolicy` ，並確認 」。 「 規則顯示**ProxyFQDN:port**。|  
  


