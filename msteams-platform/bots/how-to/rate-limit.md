---
title: 速率限制
description: Microsoft 团队中的速率限制和最佳做法
keywords: 团队 bot 速率限制
ms.openlocfilehash: 4e9efab539ec7817d259fd6c149c438ba02e3ce5
ms.sourcegitcommit: 4329a94918263c85d6c65ff401f571556b80307b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2020
ms.locfileid: "41673121"
---
# <a name="optimize-your-bot-rate-limiting-and-best-practices-in-microsoft-teams"></a>优化你的机器人： Microsoft 团队中的速率限制和最佳做法

通常情况下，您的应用程序应限制它在单个聊天或频道对话中发布的邮件数。 这可确保对最终用户不会感到 "垃圾" 的最佳体验。

为了保护 Microsoft 团队及其用户，机器人 Api 对传入请求进行评级。 跳过此限制的应用程序将`HTTP 429 Too Many Requests`收到错误状态。 所有请求都服从相同的速率限制策略，包括发送消息、通道枚举和名单读取。

由于汇率限制的确切值可能会发生更改，因此建议您的应用程序在 API 返回`HTTP 429 Too Many Requests`时实现适当的回退行为。

## <a name="handling-rate-limits"></a>处理速率限制

在颁发机器人生成器 SDK 操作时，您可以处理`Microsoft.Rest.HttpOperationException`和检查状态代码。

```csharp
try
{
    // Perform Bot Framework operation
    // for example, await connector.Conversations.UpdateActivityAsync(reply);
}
catch (HttpOperationException ex)
{
    if (ex.Response != null && (uint)ex.Response.StatusCode ==  429)
    {
        //Perform retry of the above operation/Action method
    }
}
```

## <a name="best-practices"></a>最佳做法

通常情况下，应采取简单的预防措施来`HTTP 429`避免收到响应。 例如，避免发出对相同个人或通道对话的多个请求。 相反，请考虑批处理 API 请求。

使用具有随机抖动的指数回退是处理429s 的推荐方法。 这可确保多个请求不会导致重试冲突。

## <a name="example-detecting-transient-exceptions"></a>示例：检测临时异常

下面是使用通过瞬时故障处理应用程序块的指数回退的示例。

您可以使用[临时错误处理库](/previous-versions/msp-n-p/hh680901(v=pandp.50))执行回退和重试。 有关获取和安装 NuGet 包的指南，请参阅[向解决方案中添加瞬时故障处理应用程序块](/previous-versions/msp-n-p/hh680891(v=pandp.50))

```csharp
public class BotSdkTransientExceptionDetectionStrategy : ITransientErrorDetectionStrategy
{
    // List of error codes to retry on
    List<int> transientErrorStatusCodes = new List<int>() { 429 };

    public bool IsTransient(Exception ex)
    {
        var httpOperationException = ex as HttpOperationException;
        if (httpOperationException != null)
        {
            return httpOperationException.Response != null &&
                    transientErrorStatusCodes.Contains((int) httpOperationException.Response.StatusCode);
        }

        return false;
    }
}
```

## <a name="example-backoff"></a>示例：回退

除了检测速率限制之外，还可以执行指数回退。

```csharp
/**
* The first parameter specifies the number of retries before failing the operation.
* The second parameter specifies the minimum and maximum backoff time respectively.
* The last parameter is used to add a randomized  +/- 20% delta to avoid numerous clients retrying simultaneously.
*/
var exponentialBackoffRetryStrategy = new ExponentialBackoff(3, TimeSpan.FromSeconds(2),
                        TimeSpan.FromSeconds(20), TimeSpan.FromSeconds(1));


// Define the Retry Policy
var retryPolicy = new RetryPolicy(new BotSdkTransientExceptionDetectionStrategy(), fixedIntervalRetryStrategy);

//Execute any bot sdk action
await retryPolicy.ExecuteAsync(() => connector.Conversations.ReplyToActivityAsync((Activity)reply)).ConfigureAwait(false);
```

您还可以使用上面`System.Action`介绍的重试策略执行方法执行。 引用的库还允许您指定固定间隔或线性回退机制。

建议将值和策略存储在配置文件中，以便在运行时微调和调整值。

有关详细信息，请参阅有关各种重试模式的此便捷指南：[重试模式](/azure/architecture/patterns/retry)。

## <a name="per-bot-per-thread-limit"></a>每个 bot 每个线程的限制

>[!NOTE]
>服务级别的邮件拆分会导致高于每秒预期的请求数（RPS）。 如果你担心接近限制，应实施上述回退策略。 下面提供的值仅用于评估。

此限制控制允许 bot 在单个会话中生成的流量。 此处的对话是在 bot 与用户、组聊天或团队中的频道之间的1:1。

| **应用场景** | **时间段（秒）** | **允许的最大操作** |
| --- | --- | --- |
| NewMessage | 1  | 7  |
| NewMessage | 2  | 8  |
| NewMessage | 30 | 60 |
| NewMessage | 3600 | 1800 |
| UpdateMessage | 1  | 7  |
| UpdateMessage | 2  | 8  |
| UpdateMessage | 30 | 60 |
| UpdateMessage | 3600 | 1800 |
| NewThread | 1  | 7  |
| NewThread | 2  | 8  |
| NewThread | 30 | 60 |
| NewThread | 3600 | 1800 |
| GetThreadMembers | 1  | 14  |
| GetThreadMembers | 2  | 16  |
| GetThreadMembers | 30 | 120 |
| GetThreadMembers | 3600 | 3600 |
| GetThread | 1  | 14  |
| GetThread | 2  | 16  |
| GetThread | 30 | 120 |
| GetThread | 3600 | 3600 |

## <a name="per-thread-limit-for-all-bots"></a>每个线程对所有 bot 的限制

此限制控制允许所有 bot 在单个对话中生成的流量。 此处的对话是在 bot 与用户、组聊天或团队中的频道之间的1:1。

| **应用场景** | **时间段（秒）** | **允许的最大操作** |
| --- | --- | --- |
| NewMessage | 1  | 14  |
| NewMessage | 2  | 16  |
| UpdateMessage | 1  | 14  |
| UpdateMessage | 2  | 16  |
| NewThread | 1  | 14  |
| NewThread | 2  | 16  |
| GetThreadMembers | 1  | 28 |
| GetThreadMembers | 2  | 32 |
| GetThread | 1  | 28 |
| GetThread | 2  | 32 |

## <a name="bot-per-data-center-limit"></a>每个数据中心的 Bot 限制

此限制控制允许机器人在数据中心中的所有线程（跨多个租户）生成的流量。

|**时间段（秒）** | **允许的最大操作** |
| --- | --- |
| 1  | 20 |
| 1800 | 8000 |
| 3600 | 15000 |