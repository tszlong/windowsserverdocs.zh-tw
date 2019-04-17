---
title: Windows Server 半年通道概觀
description: Microsoft 不斷簡化 Windows Server 維護作業，讓作業系統更新變得更容易測試、管理和部署。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/07/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 2995fca3085d6611ecce083685dca0e587913f17
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976724"
---
# Windows Server 半年通道概觀

>適用於：Windows Server (半年通道)

Windows Server 發行模型提供新的選項，以便與類似的 [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) 和 [Office 365 專業增強版](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US)發行及維護模型取得一致。 如果您一直在使用 Windows 10 或 Office 365 專業增強版，可能對這些改進已經很熟悉。

**Windows Server 客戶有兩個適用的主要發行通道：長期維護通道和半年通道。** 您可以將伺服器保留在長期維護通道 (LTSC)、將其移到半年通道，或在任一管道上各保留一些伺服器，端視何者最適合您的需求。


## 長期維護通道 (LTSC)
這是您已經熟悉的發行模型 (先前稱為「長期維護*分支*」)，Windows Server 依此模型每隔 2-3 年發行一次新的主要版本。 使用者有權接受 5 年的主要支援和 5 年的延伸支援。 此通道適用於需要較長期維護選項及功能穩定性的系統。 Windows Server 2016 及舊版 Windows Server 的部署不會受到新的半年通道發行影響。 長期維護通道仍將繼續收到安全性及非安全性更新，但無法獲得新特色與新功能。

> [!Note]  
> **目前的長期維護通道 (LTSC) 產品是 Windows Server 2016**。 如果您想要繼續留在這個通道中，就必須安裝 (或繼續使用) Windows Server 2016，而這可使用 [Server Core] 安裝選項或 [含桌面體驗的伺服器] 安裝選項來安裝。 如需詳細資訊，請參閱[開始使用 WindowsServer 2016](server-basics.md)。 



## 半年通道 
半年通道最適合正在加快步調充分利用新作業系統功能，迅速在應用程式 (特別是那些在容器和微服務上建置的應用程式) 和軟體定義的混合式資料中心兩方式創新的客戶。 半年通道中的 Windows Server 產品每年會有兩次新版本發行，分別在春季和秋季推出。 在此通道中發行的每個版本都會從初始版本開始提供 18 個月的支援。

半年通道中推出的大多數功能都會彙總到 Windows Server 的下一次長期維護通道發行。 各次發行的版本、功能及支援內容可能會依據客戶意見反應而有所改變。

半年通道可供大量授權客戶搭配[軟體保證](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)，以及透過 Azure Marketplace 或其他雲端/主機服務提供者和忠誠度方案 (例如 Visual Studio 訂閱) 來享用。

> [!Note]  
> **目前的半年通道版本是 Windows Server 版本 1803**。 如果您想將伺服器加入這個通道，就應該安裝 Windows Server 版本 1803，而您可以在 Server Core 模式下進行安裝，或是將其安裝成執行於容器中的 Nano 伺服器。 請參閱[Windows Server 版本 1803 簡介](get-started-with-1803.md)，以了解如何取得和啟用 Windows Server 版本 1803。 不支援從 Windows Server 2016 就地升級到 Windows Server 版本 1803，因為它們位於**不同發行通道**。 Windows Server 版本 1803 並非 Windows Server 2016 的更新，而是半年通道中的下一個 Windows Server 版本。



在此模型中，Windows Server 版本可依發行年份及月份來識別：例如，2017 年 9 月發行的版本會以**版本 1709** 做為識別。 半年通道中的 Windows Server 全新版本會每年發行兩次。 每個發行版本的支援週期為 18 個月。

## 您應該讓伺服器保持在 LTSC 或是移到半年通道？
以下是要考慮的關鍵差異：

- 您需要迅速創新嗎？ 您需要優先取得最新的 Windows Server 功能嗎？ 您需要支援快節奏的混合式應用程式、DevOps 和 Hyper-V 網狀架構嗎？ 若是，您應該考慮透過安裝 [Windows Server 版本 1803](get-started-with-1803.md) 來**加入半年通道**。 如本主題所述，您將會每年收到兩次新版本，每個版本各有 18 個月的主要生產環境支援。 您可以透過大量授權、Azure 或 Visual Studio 訂閱服務取得項目。 目前，如果您想在生產環境中執行產品，半年通道發行需要大量授權和軟體保證。
- 您需要穩定性與可預測性嗎？ 您需要在實體伺服器上執行虛擬機器和傳統工作負載嗎？ 若是，您應該考慮**將那些伺服器保持在長期維護通道**。 目前的 LTSC 版本是 [Windows Server 2016](server-basics.md)。 如本主題所述，您可以每 2-3 年存取新版本，每個版本具備 5 年主要支援外加 5 年延伸支援。 LTSC 版本可透過所有發行機制提供。 無論使用何種授權模型，任何人都能取得 LTSC 中的發行版本。 

下表摘要說明通道之間從 Windows Server 版本 1803 開始出現的主要差異︰

|  | 長期維護通道 (Windows Server 2016) |半年通道 (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|建議的案例 | 一般用途檔案伺服器、Microsoft 和非 Microsoft 工作負載、傳統應用程式、基礎結構角色、軟體定義資料中心以及超融合式基礎結構 | 容器化應用程式、容器主機，以及因加快創新而受益的應用程式案例 |
| 最新發行 | 每隔 2-3 年一次 |每隔 6 個月一次 |
| 支援 |5 年主要支援，加上 5 年延伸支援 | 18 個月 |
| 版本 | 所有可用的 Windows Server 版本 | Standard 和 Datacenter Edition |
| 誰可以使用 | 透過所有通道更新的所有客戶 | 僅限軟體保證與雲端客戶 |
| 安裝選項 | Server Core 以及具備桌面體驗的伺服器 | 適用於容器主機和映像以及 Nano Server 容器映像的 Server Core |                |


## 裝置相容性
除非另有通知，執行半年通道發行版本的最低硬體需求與 Windows Server 最新的長期維護通道發行版本相同。 例如，**長期維護通道目前的版本是 Windows Server 2016**。 大部分的硬體驅動程式仍可繼續在這些版本中運作。

## 維護
長期維護通道和半年通道發行版本都會有安全性更新及非安全性更新的支援。 如上文所述，差別在於發行版本受支援的時間長度。

### 維護工具
IT 專業人員有許多工具可以維護 Windows Server。 每個選項都有其優點和缺點，有各種功能和控制項可以簡化和降低系統管理需求。 以下是可用來管理維護更新的維護工具範例︰

- **Windows Update (獨立)**：這個選項只適用於連線至網際網路且已啟用 Windows Update 的伺服器。
- **Windows Server Update Services (WSUS)** 提供比 Windows 10 和 Windows Server 更新更加廣泛的控制，而且內建在 Windows Server 作業系統中。 除了延遲更新的能力，組織可以新增更新的核准層，並選擇在電腦就緒時將更新部署至特定的電腦或一組電腦。
- **System Center Configuration Manager** 提供最大限度掌控維護的能力。 IT 專業人員可以延遲更新、核准更新，而且有多個選項可以設定部署，以及管理頻寬的使用方式和部署時間。

您可能已經根據您的資源、人員及專業知識，選擇使用至少其中一個選項。 您仍然可以繼續對半年通道發行版本使用同樣的程序：例如，已經使用 System Center Configuration Manager 來管理更新，還是可以繼續使用。 同樣地，如果您正在使用 WSUS，也可以繼續這麼使用。

## 透過 Windows 測試人員計畫取得預覽版
測試 Windows Server 的早期組建對 Microsoft 及其客戶都有幫助，因為這樣就有機會在發行前發現可能的問題。 同時也能讓客戶把握絕佳機會直接影響產品中的功能。 

Microsoft 對接收整個開發流程的意見反應有所依賴，藉此可以盡快進行調整。 早期測試和意見反應對快速發行模型極其重要，缺一不可。

如需如何加入 Windows 測試人員計畫的詳細資訊，請參閱[伺服器用 Windows 測試人員計畫文件](https://docs.microsoft.com/windows-insider/at-work/) (英文)。
# 相關主題
[Windows Server 2019 服務通道：LTSC 和 SAC](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19)

[Nano Server 在 Windows Server 半年通道中的變更](nano-in-semi-annual-channel.md)

[Windows Server 支援週期](https://support.microsoft.com/en-us/lifecycle)

[Windows Server 2016 系統需求](https://docs.microsoft.com/windows-server/get-started/system-requirements) 




