---
title: 准备 Microsoft 365 租户
description: 如何开始使用 Microsoft 365 中的团队
keywords: 配置 Microsoft 365 租户团队上载
ms.openlocfilehash: 67a0342a7e8605097eed53dd1b0bdf273d537c0e
ms.sourcegitcommit: d61f14053fc695bc1956bf50e83956613c19ccca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "48452762"
---
# <a name="prepare-your-microsoft-365-tenant"></a>准备 Microsoft 365 租户

如果你是 Microsoft 365 订阅者，则可以使用以下 [计划](https://products.office.com/business/compare-more-office-365-for-business-plans)之一为 Microsoft 团队开发应用程序：

* 基本
* 标准
* 企业版 E1、E3 和 E5
* 开发人员版
* 教育版、教育版和教育版 E5

Microsoft 团队还将对在其 [退休](https://support.office.com//article/important-information-for-office-365-enterprise-e4-customers-f9572348-43a2-43fa-a3d8-3b6c9c042147)前订阅 E4 的客户提供。

## <a name="just-need-a-development-environment"></a>只需要开发环境？

如果你当前没有 Microsoft 365 帐户，你可以注册 [microsoft 365 开发人员计划](https://developer.microsoft.com/microsoft-365/dev-program) 订阅。 它在90天内 *免费* 使用，并且将持续更新，只要你将其用于开发活动即可。 如果你有 Visual Studio *Enterprise* 或 *专业版* 订阅，这两个程序都包括一个免费的 Microsoft 365 [开发人员订阅](https://aka.ms/MyVisualStudioBenefits)，在 Visual Studio 订阅的生命周期内处于活动状态。 *请参阅*[设置 Microsoft 365 开发人员订阅](https://docs.microsoft.com/office/developer-program/office-365-developer-program-get-started)。

## <a name="enable-microsoft-teams-for-your-organization"></a>为你的组织启用 Microsoft 团队 

如果尚未为你的组织启用 Microsoft 团队，你将需要先执行此操作。 请参阅我们为 [你的组织启用团队](/microsoftteams/enable-features-office-365)的详细指南。

## <a name="enable-custom-teams-apps-and-turn-on-custom-app-uploading"></a>启用自定义团队应用并启用自定义应用上传

按如下方式为开发人员租户启用自定义应用旁加载：

1. 使用管理员凭据登录到 [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home?source=applauncher#/homepage#/) 。 

2. 选择 "**显示所有**  -->  **团队**"。 

![管理中心菜单的图像](~/assets/images/prepare-test-tenant/admin-center.png)

> [!Note] 
> "团队" 选项最长可能需要24小时才能显示。 在中间阶段，您可以 [将自定义应用程序上载到团队环境](/microsoftteams/upload-custom-apps#validate) 以进行测试和验证。

3. 导航到**团队应用**  -->  **程序设置策略**  -->  **全局 (组织范围默认) **  

![打开旁加载视图](~/assets/images/prepare-test-tenant/turn-on-sideload.png)

4. 将 " **上载自定义应用** " 切换到 " **开** " 位置。

就是这么简单。 你的测试租户现在将允许自定义应用旁加载。

> [!Note] 
> 在启用旁加载之前，可能需要长达24小时。 在临时期间，您可以使用**上 \<your tenant> 传**来测试您的应用程序。

![updload 应用程序视图](~/assets/images/prepare-test-tenant/upload-for-contoso.png)

有关这些设置如何进行交互的完整信息， *请参阅*在 microsoft 团队中 [管理自定义应用策略和设置](https://docs.microsoft.com/microsoftteams/teams-custom-app-policies-and-settings) 和 [管理 microsoft 团队中的应用程序安装策略](https://docs.microsoft.com/microsoftteams/teams-app-setup-policies)。
