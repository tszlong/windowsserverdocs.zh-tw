---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendMSFT
keywords: OpenSSH, SSH, Win32-OpenSSH
title: 在 Windows 中的 OpenSSH
ms.product: w10
ms.openlocfilehash: c6563fbe4fe69acad4d295a3f7fe166e92d38444
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280055"
---
# <a name="openssh-in-windows"></a>在 Windows 中的 OpenSSH

OpenSSH 是開放原始碼版本的 Linux 系統管理員和其他非 Windows 用於跨平台管理遠端系統的安全殼層 (SSH) 工具。 OpenSSH 自 2018 年秋季起已新增至 Windows，並包含在 Windows 10 和 Windows Server 2019。 

SSH 為基礎的用戶端-伺服器架構，其中使用者正在使用的系統是用戶端，而遠端受管理的系統是伺服器。 OpenSSH 包含的元件和設計工具可提供安全且直接的方法，遠端系統管理範圍包括：

* sshd.exe，也就是必須從遠端管理系統執行的 SSH 伺服器元件 
* ssh.exe，這是在使用者的本機系統執行的 SSH 用戶端元件
* ssh-keygen.exe 產生、 管理以及將適用於 ssh 連線的驗證金鑰 
* ssh-agent.exe 儲存針對公開金鑰驗證所使用的私密金鑰
* ssh-add.exe 將私密金鑰加入至允許伺服器所使用的清單
* 收集的公用 SSH 主機金鑰，從多個主機的 ssh-keyscan.exe 輔助工具
* sftp.exe 是提供安全檔案傳輸通訊協定，並透過 SSH 執行的服務
* scp.exe 是 SSH 執行的檔案複製公用程式

在本節中的文件著重於 OpenSSH 上 Windows，包括安裝和 Windows 特定設定和使用案例的使用方式。 以下是主題：
* 安裝及解除安裝適用於 Windows Server 2019 和 Windows 10 的 OpenSSH

其他常見的 OpenSSH 功能的詳細文件位於線上[OpenSSH.com](https://www.openssh.com/manual.html)。 

主要[OpenSSH 開放原始碼專案](https://www.openssh.com)由開發人員在原本是 OpenBSD 專案管理。 此專案的 Microsoft 分叉處於[GitHub](https://github.com/PowerShell/openssh-portable)。
Windows OpenSSH 反應此與大家分享，而且可以藉由建立 GitHub 問題提供我們[OpenSSH GitHub 存放庫](https://github.com/PowerShell/openssh-portable)。 
