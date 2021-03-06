---
title: 如何使用 Node.js-Azure 存储中的 Azure 队列存储
description: 了解如何使用 Azure 队列存储来创建和删除队列。 学习使用 Node.js 插入、获取和删除消息。
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: dineshm
ms.date: 12/21/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 12ae05e10cdf0fa9a5f0725acaa1784eedc3612c
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803674"
---
# <a name="how-to-use-azure-queue-storage-from-nodejs"></a>如何从 Node.js 使用 Azure 队列存储

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>概述

本指南演示如何使用 Azure 队列存储完成常见方案。 相关示例是使用 Node.js API 编写的。 涵盖的方案包括插入、速览、获取和删除队列消息。 还将学习创建和删除队列。

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>创建 Node.js 应用程序

若要创建空白 Node.js 应用程序，请参阅[在 Azure App Service 中创建 Node.js web 应用](../../app-service/quickstart-nodejs.md)，使用 PowerShell 或[Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)[构建 Node.js 应用程序并将其部署到 Azure 云服务](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)。

## <a name="configure-your-application-to-access-storage"></a>配置应用程序以访问存储

[用于 JavaScript 的 Azure 存储客户端库](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage#azure-storage-client-library-for-javascript)包括一组便于与存储 REST 服务通信的库。

<!-- docutune:ignore Terminal -->

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>使用节点包管理器 (npm) 获取包

1. 使用 PowerShell (Windows)、Terminal (Mac) 或 Bash (Unix) 等命令行界面，导航到在其中创建了示例应用程序的文件夹。

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

1. `npm install @azure/storage-queue`在 "命令" 窗口中键入。

1. 验证文件夹是否 `node_modules` 已创建。 在该文件夹中，你将找到 `@azure/storage-queue` 包，其中包含访问存储所需的客户端库。

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

1. `npm install azure-storage`在 "命令" 窗口中键入。

1. 验证文件夹是否 `node_modules` 已创建。 在该文件夹中，会找到 `azure-storage` 包，其中包含访问存储所需的库。

---

### <a name="import-the-package"></a>导入包

使用你的代码编辑器，将以下内容添加到要在其中使用队列的 JavaScript 文件的顶部。

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ImportStatements":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

```javascript
var azure = require('azure-storage');
```

---

## <a name="how-to-create-a-queue"></a>如何创建队列

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

下面的代码获取名为的环境变量的值 `AZURE_STORAGE_CONNECTION_STRING` ，并使用它来创建 [`QueueServiceClient`](/javascript/api/@azure/storage-queue/queueserviceclient) 对象。 然后，将使用此对象创建一个 [`QueueClient`](/javascript/api/@azure/storage-queue/queueclient) 对象，该对象可用于处理特定队列。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_CreateQueue":::

如果队列已存在，则会引发异常。

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Azure 模块将读取环境变量 `AZURE_STORAGE_ACCOUNT` `AZURE_STORAGE_ACCESS_KEY` 、或，以 `AZURE_STORAGE_CONNECTION_STRING` 获取连接到 Azure 存储帐户所需的信息。 如果未设置这些环境变量，则在调用时必须指定帐户信息 `createQueueService` 。

下面的代码创建一个 `QueueService` 对象，该对象使你可以使用队列。

```javascript
var queueSvc = azure.createQueueService();
```

调用 `createQueueIfNotExists` 方法以创建具有指定名称的新队列，如果该队列已存在，则返回该队列。

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

如果创建了队列，则 `result.created` 为 true。 如果队列已存在，则 `result.created` 为 false。

---

## <a name="how-to-insert-a-message-into-a-queue"></a>如何在队列中插入消息

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

若要将消息添加到队列，请调用 [`sendMessage`](/javascript/api/@azure/storage-queue/queueclient#sendmessage-string--queuesendmessageoptions-) 方法。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_AddMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

若要在队列中插入消息，请调用 `createMessage` 方法来创建新消息并将其添加到队列中。

```javascript
queueSvc.createMessage('myqueue', "Hello, World", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

---

## <a name="how-to-peek-at-the-next-message"></a>如何速览下一条消息

通过调用方法，可以查看队列中的消息，而不必从队列中将其删除 `peekMessages` 。

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

默认情况下， [`peekMessages`](/javascript/api/@azure/storage-queue/queueclient#peekmessages-queuepeekmessagesoptions-) 扫视单个消息。 以下示例速览队列中的前五条消息。 如果可见消息少于五条，则只返回这些可见消息。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_PeekMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

默认情况下，`peekMessages` 扫视单条消息。

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

`result` 包含该消息。

---

`peekMessages`当队列中没有消息时调用，将不会返回错误。 但是，不会返回消息。

## <a name="how-to-change-the-contents-of-a-queued-message"></a>如何更改已排队消息的内容

以下示例将更新消息的文本。

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

通过调用在队列中就地更改消息的内容 [`updateMessage`](/javascript/api/@azure/storage-queue/queueclient#updatemessage-string--string--string--number--queueupdatemessageoptions-) 。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_UpdateMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

通过调用在队列中就地更改消息的内容 `updateMessage` 。

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

---

## <a name="how-to-dequeue-a-message"></a>如何将消息取消排队

将消息取消排队是一个两阶段过程：

1. 获取消息。

1. 删除消息。

以下示例获取一条消息，然后将其删除。

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

若要获取消息，请调用 [`receiveMessages`](/javascript/api/@azure/storage-queue/queueclient#receivemessages-queuereceivemessageoptions-) 方法。 此调用会使消息在队列中不可见，使其他客户端无法处理它们。 应用程序处理完消息后，调用 [`deleteMessage`](/javascript/api/@azure/storage-queue/queueclient#deletemessage-string--string--queuedeletemessageoptions-) 将其从队列中删除。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessage":::

默认情况下，消息只会隐藏 30 秒。 30 秒后，其他客户端就会看到它。 您可以通过在调用时设置来指定其他值 [`options.visibilityTimeout`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout) `receiveMessages` 。

`receiveMessages`当队列中没有消息时调用，将不会返回错误。 但是，不会返回消息。

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

若要获取消息，请调用 `getMessages` 方法。 此调用会使消息在队列中不可见，使其他客户端无法处理它们。 应用程序处理完消息后，调用 `deleteMessage` 将其从队列中删除。

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

默认情况下，消息只会隐藏 30 秒。 30 秒后，其他客户端就会看到它。 您可以使用 with 来指定不同的 `options.visibilityTimeout` 值 `getMessages` 。

`getMessages`当队列中没有消息时，不会返回错误。 但是，不会返回消息。

---

## <a name="additional-options-for-dequeuing-messages"></a>用于取消对消息进行排队的其他选项

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

可以通过两种方式自定义队列中的消息检索：

- [`options.numberOfMessages`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#numberofmessages)：检索 (最多 32) 的一批消息。
- [`options.visibilityTimeout`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout)：设置更长或更短的不可见超时。

下面的示例使用 `receiveMessages` 方法在一次调用中获取5条消息。 然后，使用 `for` 循环处理每条消息。 它还会将通过此方法返回的所有消息的不可见性超时设置为 5 分钟。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessages":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

可以通过两种方式自定义队列中的消息检索：

- `options.numOfMessages`：检索 (最多 32) 的一批消息。
- `options.visibilityTimeout`：设置更长或更短的不可见超时。

下面的示例使用 `getMessages` 方法在一次调用中获取15条消息。 然后，使用 `for` 循环处理每条消息。 它还会将通过此方法返回的所有消息的不可见性超时设置为 5 分钟。

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in results){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

---

## <a name="how-to-get-the-queue-length"></a>如何获取队列长度

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

[`getProperties`](/javascript/api/@azure/storage-queue/queueclient#getproperties-queuegetpropertiesoptions-)方法返回有关队列的元数据，包括队列中等待的消息的大致数目。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_QueueLength":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

`getQueueMetadata`方法返回有关队列的元数据，包括队列中等待的消息的大致数目。

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

---

## <a name="how-to-list-queues"></a>如何列出队列

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

若要检索队列的列表，请调用 [`QueueServiceClient.listQueues`](/javascript/api/@azure/storage-queue/servicelistqueuesoptions#prefix) 。 若要检索按特定前缀筛选的列表，请在对的调用中设置[选项。](/javascript/api/@azure/storage-queue/servicelistqueuesoptions#prefix) `listQueues`

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ListQueues":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

若要检索队列列表，请使用 `listQueuesSegmented` 。 若要检索按特定前缀筛选的列表，请使用 `listQueuesSegmentedWithPrefix` 。

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

如果无法返回所有队列，请 `result.continuationToken` 将传递为的第一个参数，或者传递的 `listQueuesSegmented` 第二个参数 `listQueuesSegmentedWithPrefix` 来检索更多结果。

---

## <a name="how-to-delete-a-queue"></a>如何删除队列

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

若要删除队列及其中包含的所有消息，请对 [`DeleteQueue`](/javascript/api/@azure/storage-queue/queueclient#delete-queuedeleteoptions-) 对象调用方法 `QueueClient` 。

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DeleteQueue":::

若要清除队列中的所有消息而不删除它，请调用 [`ClearMessages`](/javascript/api/@azure/storage-queue/queueclient#clearmessages-queueclearmessagesoptions-) 。

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

若要删除队列及其中包含的所有消息，请对 `deleteQueue` 队列对象调用方法。

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

若要清除队列中的所有消息而不删除它，请调用 `clearMessages` 。

---

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="next-steps"></a>后续步骤

现在，你已了解有关队列存储的基础知识，请单击下面的链接了解更复杂的存储任务。

- 访问 [Azure 存储团队博客](https://techcommunity.Microsoft.com/t5/Azure-storage/bg-p/azurestorageblog) ，了解新增功能
- 访问 GitHub 上的[用于 JavaScript 的 Azure 存储客户端库](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage#Azure-storage-client-library-for-JavaScript)存储库
