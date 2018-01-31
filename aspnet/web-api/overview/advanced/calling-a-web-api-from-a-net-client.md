---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: ".NET クライアント (c#) から Web API を呼び出す |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8156bd1c7cfc111a6a121a89d845ca284ee1b7af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="e8d7d-102">.NET クライアント (c#) から Web API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="e8d7d-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="e8d7d-103">によって[Mike Wasson](https://github.com/MikeWasson)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e8d7d-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="e8d7d-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="e8d7d-105">このチュートリアルでは、.NET アプリケーションから web API を呼び出す方法を使用して[System.Net.Http.HttpClient です。](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="e8d7d-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="e8d7d-106">このチュートリアルでは、次の web API を使用するクライアント アプリケーションが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="e8d7d-107">アクション</span><span class="sxs-lookup"><span data-stu-id="e8d7d-107">Action</span></span> | <span data-ttu-id="e8d7d-108">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="e8d7d-108">HTTP method</span></span> | <span data-ttu-id="e8d7d-109">相対 URI</span><span class="sxs-lookup"><span data-stu-id="e8d7d-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e8d7d-110">ID の製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-110">Get a product by ID</span></span> | <span data-ttu-id="e8d7d-111">GET</span><span class="sxs-lookup"><span data-stu-id="e8d7d-111">GET</span></span> | <span data-ttu-id="e8d7d-112">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e8d7d-112">/api/products/*id*</span></span> |
| <span data-ttu-id="e8d7d-113">新しい製品を作成します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-113">Create a new product</span></span> | <span data-ttu-id="e8d7d-114">POST</span><span class="sxs-lookup"><span data-stu-id="e8d7d-114">POST</span></span> | <span data-ttu-id="e8d7d-115">/api/products</span><span class="sxs-lookup"><span data-stu-id="e8d7d-115">/api/products</span></span> |
| <span data-ttu-id="e8d7d-116">製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-116">Update a product</span></span> | <span data-ttu-id="e8d7d-117">PUT</span><span class="sxs-lookup"><span data-stu-id="e8d7d-117">PUT</span></span> | <span data-ttu-id="e8d7d-118">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e8d7d-118">/api/products/*id*</span></span> |
| <span data-ttu-id="e8d7d-119">製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-119">Delete a product</span></span> | <span data-ttu-id="e8d7d-120">Del</span><span class="sxs-lookup"><span data-stu-id="e8d7d-120">DELETE</span></span> | <span data-ttu-id="e8d7d-121">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e8d7d-121">/api/products/*id*</span></span> |

<span data-ttu-id="e8d7d-122">この API は、ASP.NET Web API を実装する方法については、次を参照してください。 [CRUD 操作をサポートする Web API を作成する](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)です。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="e8d7d-123">わかりやすくするため、このチュートリアルでは、クライアント アプリケーションは、Windows コンソール アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="e8d7d-124">**HttpClient** Windows Phone や Windows ストア アプリでもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="e8d7d-125">詳細については、次を参照してください[複数のプラットフォームを使用してポータブル ライブラリの Web API クライアント コードの記述。](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="e8d7d-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="e8d7d-126">コンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-126">Create the Console Application</span></span>

<span data-ttu-id="e8d7d-127">Visual Studio で、という名前の新しい Windows コンソール アプリを作成する**HttpClientSample**し、次のコードに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="e8d7d-128">上記のコードは、完全なクライアント アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="e8d7d-129">`RunAsync`実行され、完了するまでブロックされます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="e8d7d-130">ほとんど**HttpClient**ネットワーク I/O を実行するため、メソッドは非同期、します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="e8d7d-131">内で行うすべての非同期タスクが`RunAsync`です。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="e8d7d-132">通常、アプリは、メイン スレッドをブロックしませんが、このアプリはユーザーとの対話を許可しません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="e8d7d-133">Web API のクライアント ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="e8d7d-134">NuGet パッケージ マネージャーを使用して、Web API Client Libraries パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="e8d7d-135">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="e8d7d-136">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="e8d7d-137">上記のコマンドは、プロジェクトに次の NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="e8d7d-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="e8d7d-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="e8d7d-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="e8d7d-139">Newtonsoft.Json</span></span>

<span data-ttu-id="e8d7d-140">Json.NET は、.NET の人気のある高パフォーマンス JSON フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="e8d7d-141">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-141">Add a Model Class</span></span>

<span data-ttu-id="e8d7d-142">確認、`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="e8d7d-143">このクラスでは、web API によって使用されるデータ モデルと一致します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="e8d7d-144">アプリが使用できる**HttpClient**を読み取る、 `Product` HTTP 応答からインスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="e8d7d-145">逆シリアル化コードを記述する、アプリはありません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="e8d7d-146">作成および HttpClient の初期化</span><span class="sxs-lookup"><span data-stu-id="e8d7d-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="e8d7d-147">静的なを調べて**HttpClient**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="e8d7d-148">**HttpClient**は 1 回インスタンス化するためのものであり、アプリケーションの有効期間全体で再利用します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="e8d7d-149">次の条件が**SocketException**エラー。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="e8d7d-150">新たに作成する**HttpClient**要求ごとのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="e8d7d-151">負荷の高いサーバーです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-151">Server under heavy load.</span></span>

<span data-ttu-id="e8d7d-152">新たに作成する**HttpClient**要求ごとにインスタンスが使用可能な sockets を使い果たしてしまうことができます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="e8d7d-153">次のコードを初期化します、 **HttpClient**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="e8d7d-154">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-154">The preceding code:</span></span>

* <span data-ttu-id="e8d7d-155">HTTP 要求のベース URI を設定します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="e8d7d-156">サーバー アプリケーションで使用されるポートにポート番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="e8d7d-157">サーバー アプリケーション用のポートを使用しない場合、アプリは機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="e8d7d-158">"Application/json"に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="e8d7d-159">このヘッダーを設定するには、JSON 形式でデータを送信するサーバーがように指示します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="e8d7d-160">リソースを取得する GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="e8d7d-161">次のコードは、製品の GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="e8d7d-162">**されます**メソッドは、HTTP GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="e8d7d-163">完了時のメソッドを返します、 **HttpResponseMessage** HTTP 応答を格納しています。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="e8d7d-164">応答にステータス コードが成功コードの場合は、応答本文には、製品の JSON 表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="e8d7d-165">呼び出す**ReadAsAsync** JSON ペイロードを逆シリアル化する、`Product`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="e8d7d-166">**ReadAsAsync**メソッドでは非同期応答の本体は任意の大きさを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="e8d7d-167">**HttpClient** HTTP 応答には、エラー コードが含まれている場合、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="e8d7d-168">代わりに、 **IsSuccessStatusCode**プロパティは**false**状態がエラー コードの場合。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="e8d7d-169">HTTP エラー コードを例外として処理する場合は、呼び出す[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) response オブジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="e8d7d-170">`EnsureSuccessStatusCode`ステータス コードが 200 の範囲外になった場合に例外をスロー&ndash;299 です。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="e8d7d-171">注意してください**HttpClient**の他の理由から例外をスローできます&mdash;要求がタイムアウトになる場合などです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="e8d7d-172">逆シリアル化するメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="e8d7d-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="e8d7d-173">ときに**ReadAsAsync**が呼び出されたの既定のセットを使用して、パラメーターなしで*メディア フォーマッタ*応答本体を読み取る。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="e8d7d-174">既定のフォーマッタは、JSON、XML、およびフォームの url エンコード データをサポートします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="e8d7d-175">既定のフォーマッタを使用する代わりに、フォーマッタの一覧を指定することができます、 **ReadAsAsync**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="e8d7d-176">使用して、フォーマッタの一覧は、カスタムのメディア タイプ フォーマッタがある場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="e8d7d-177">詳細については、次を参照してください[ASP.NET Web API 2 に、メディア フォーマッタ。](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="e8d7d-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="e8d7d-178">リソースを作成する POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="e8d7d-179">次のコードを含む POST 要求を送信する、 `Product` JSON 形式でインスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="e8d7d-180">**PostAsJsonAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="e8d7d-181">オブジェクトを JSON にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="e8d7d-182">POST 要求で JSON ペイロードを送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="e8d7d-183">要求が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-183">If the request succeeds:</span></span>

* <span data-ttu-id="e8d7d-184">201 (Created) 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="e8d7d-185">応答は、Location ヘッダーの作成済みのリソースの URL を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="e8d7d-186">リソースを更新する PUT 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="e8d7d-187">次のコードでは、製品を更新する PUT 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="e8d7d-188">**PutAsJsonAsync**のような方法は、機能**PostAsJsonAsync**POST の代わりに PUT 要求を送信する点を除いて、します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="e8d7d-189">リソースを削除する削除要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="e8d7d-190">次のコードは、製品を削除する削除要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="e8d7d-191">GET と同様に DELETE 要求しても、要求本文はありません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="e8d7d-192">DELETE で JSON または XML の形式を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="e8d7d-193">このサンプルをテストします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-193">Test the sample</span></span>

<span data-ttu-id="e8d7d-194">クライアント アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-194">To test the client app:</span></span>

1. <span data-ttu-id="e8d7d-195">[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)server アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="e8d7d-196">[ダウンロード命令の](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample)します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-196">[Download instructions](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="e8d7d-197">サーバー アプリが動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-197">Verify the server app is working.</span></span> <span data-ttu-id="e8d7d-198">Exaxmple の`http://localhost:64195/api/products`製品の一覧を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="e8d7d-199">HTTP 要求のベース URI を設定します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="e8d7d-200">サーバー アプリケーションで使用されるポートにポート番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="e8d7d-201">クライアント アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-201">Run the client app.</span></span> <span data-ttu-id="e8d7d-202">次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e8d7d-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```