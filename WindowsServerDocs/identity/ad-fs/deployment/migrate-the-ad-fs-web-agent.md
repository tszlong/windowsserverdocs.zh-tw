---
title: 移轉 AD FS 網路代理程式
description: 提供 Windows Server 2012 的 AD FS 網路代理程式資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7cde62cb23c69a425522e40ed65ee2d40ef28268
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445624"
---
# <a name="migrate-the-ad-fs-web-agent"></a>移轉 AD FS 網路代理程式

若要移轉的 AD FS 1.1 Windows 權杖型代理程式或 AD FS 1.1 宣告感知代理程式，會隨 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 中，執行主控其中一個代理程式之電腦的作業系統就地升級為 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 不需要進一步的設定。  
  
> [!IMPORTANT]
>  移轉的 AD FS 1.1 Windows 權杖型代理程式只能與隨 Windows Server 2008 R2 或 Windows Server 2008 一起安裝的 AD FS 1.1 同盟服務搭配運作。 如需詳細資訊，請參閱 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
> 
>  移轉的 AD FS 1.1 宣告感知網路代理程式可與下列項目搭配運作：  
> 
> - 隨 Windows Server 2008 R2 或 Windows Server 2008 一起安裝的 AD FS 1.1 同盟服務  
>   -   安裝在 Windows Server 2008 R2 或 Windows Server 2008 上的 AD FS 2.0 同盟服務  
>   -   與 Windows Server 2012 安裝的 AD FS 同盟服務  
> 
>   如需詳細資訊，請參閱 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)