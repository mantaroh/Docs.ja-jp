---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "このドキュメントでは、SignalR、WPF、Silverlight、および短所を Windows ストア (WinRT) など、.NET クライアントでは、バージョン 2 のハブ API を使用して紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="0cefa-103">ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="0cefa-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="0cefa-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0cefa-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="0cefa-105">このドキュメントでは、SignalR バージョン 2 で Windows ストア (WinRT)、WPF、Silverlight、およびコンソール アプリケーションなどの .NET クライアントのハブ API を使用してに紹介します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="0cefa-106">SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="0cefa-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="0cefa-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="0cefa-109">すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="0cefa-110">SignalR では、永続的な接続と呼ばれる、下位の API も提供します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="0cefa-111">SignalR、ハブ、および永続的な接続の概要または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照して[SignalR - はじめ](../getting-started/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="0cefa-112">概要</span><span class="sxs-lookup"><span data-stu-id="0cefa-112">Overview</span></span>

<span data-ttu-id="0cefa-113">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="0cefa-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="0cefa-114">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="0cefa-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="0cefa-115">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="0cefa-116">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="0cefa-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="0cefa-117">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="0cefa-118">WPF クライアントに同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="0cefa-119">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="0cefa-120">転送方法を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="0cefa-121">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="0cefa-122">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="0cefa-123">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="0cefa-124">サーバーが呼び出すことができるクライアント上のメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="0cefa-125">パラメーターのないメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="0cefa-126">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="0cefa-127">動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="0cefa-128">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="0cefa-129">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="0cefa-130">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="0cefa-131">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="0cefa-132">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="0cefa-133">WPF、Silverlight、および、サーバーを呼び出すことができるクライアント メソッドのコンソール アプリケーションのコード サンプル</span><span class="sxs-lookup"><span data-stu-id="0cefa-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="0cefa-134">サンプル .NET クライアント プロジェクトでは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cefa-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="0cefa-135">[gustavo armenta/SignalR サンプル](https://github.com/gustavo-armenta/SignalR-Samples)GitHub.com (WinRT、Silverlight、コンソール アプリケーション例) にします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="0cefa-136">[DamianEdwards/SignalR MoveShapeDemo/MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) GitHub.com (WPF など) にします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="0cefa-137">[SignalR/Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) GitHub.com (コンソール アプリなど) にします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="0cefa-138">サーバーまたは JavaScript クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cefa-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="0cefa-139">SignalR ハブ API ガイド - サーバー</span><span class="sxs-lookup"><span data-stu-id="0cefa-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="0cefa-140">SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="0cefa-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="0cefa-141">API リファレンス トピックへのリンクは、.NET 4.5 のバージョンの API といます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="0cefa-142">.NET 4 を使用している場合は、次を参照してください。[の API のトピックは、.NET 4 バージョン](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="0cefa-143">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="0cefa-143">Client setup</span></span>

<span data-ttu-id="0cefa-144">インストール、 [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet パッケージ (されません、 [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)パッケージ)。</span><span class="sxs-lookup"><span data-stu-id="0cefa-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="0cefa-145">このパッケージは、.NET 4 および .NET 4.5 の両方の WinRT、Silverlight、WPF、コンソール アプリケーション、および Windows Phone クライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="0cefa-146">クライアント上にある SignalR のバージョンがサーバー上にあるバージョンと異なる場合は、SignalR は違いに合わせて調整することが多くの場合です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="0cefa-147">たとえば、SignalR バージョン 2.0 が解放され、サーバー上にインストールして、サーバーは 1.1.x 2.0 がインストールされているクライアントだけでなくインストールされているクライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="0cefa-148">サーバー上のバージョンと、クライアントのバージョンの違いが高すぎる場合は、SignalR をスロー、`InvalidOperationException`クライアントが接続を確立しようとした場合に例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="0cefa-149">エラー メッセージは"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="0cefa-150">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-150">How to establish a connection</span></span>

<span data-ttu-id="0cefa-151">作成する必要が接続を確立することができます、前に、`HubConnection`オブジェクトおよびプロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="0cefa-152">接続を確立するために呼び出す、`Start`メソッドを`HubConnection`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0cefa-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="0cefa-153">JavaScript クライアントを呼び出す前に、少なくとも 1 つのイベント ハンドラーを登録する必要がある、`Start`接続を確立する方法です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="0cefa-154">これは、.NET クライアントの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0cefa-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="0cefa-155">JavaScript クライアントは、生成されたプロキシ コードが自動的に作成が存在するすべてのハブのプロキシ サーバーで、およびどのハブを指定する方法は、ハンドラーの登録を使用しているつもりのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="0cefa-156">.NET クライアントのハブ プロキシ手動で作成するため、SignalR のプロキシを作成するすべてのハブを使用することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="0cefa-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="0cefa-157">既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。</span><span class="sxs-lookup"><span data-stu-id="0cefa-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0cefa-158">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="0cefa-159">`Start`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="0cefa-160">接続が確立された後にするまで以降の行のコードを実行しないようにするには、次のように使用します。 `await` 、ASP.NET 4.5 非同期メソッドまたは`.Wait()`同期メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="0cefa-161">使用しない`.Wait()`WinRT クライアントにします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="0cefa-162">`HubConnection` クラスはスレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="0cefa-163">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="0cefa-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="0cefa-164">Silverlight クライアントからドメイン間の接続を有効にする方法については、次を参照してください。 [、サービス利用可能なドメインの境界を越えてを行う](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="0cefa-165">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-165">How to configure the connection</span></span>

<span data-ttu-id="0cefa-166">接続を確立する前に、次のオプションのいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="0cefa-167">同時接続数を制限します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="0cefa-168">クエリ文字列パラメーター。</span><span class="sxs-lookup"><span data-stu-id="0cefa-168">Query string parameters.</span></span>
- <span data-ttu-id="0cefa-169">トランスポート メソッド。</span><span class="sxs-lookup"><span data-stu-id="0cefa-169">The transport method.</span></span>
- <span data-ttu-id="0cefa-170">HTTP ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="0cefa-170">HTTP headers.</span></span>
- <span data-ttu-id="0cefa-171">クライアント証明書。</span><span class="sxs-lookup"><span data-stu-id="0cefa-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="0cefa-172">WPF クライアントに同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="0cefa-173">WPF クライアントには、既定値から 2 の同時接続の最大数を増やす必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cefa-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="0cefa-174">推奨値は 10 です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="0cefa-175">詳細については、次を参照してください。 [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="0cefa-176">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-176">How to specify query string parameters</span></span>

<span data-ttu-id="0cefa-177">クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="0cefa-178">次の例では、クライアント コードでクエリ文字列パラメーターを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="0cefa-179">次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="0cefa-180">転送方法を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-180">How to specify the transport method</span></span>

<span data-ttu-id="0cefa-181">接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定するサーバーとネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="0cefa-182">使用するトランスポートが既に把握している場合は、このネゴシエーション プロセスをバイパスできます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="0cefa-183">転送方法を指定するには、Start メソッドをトランスポート オブジェクトで渡します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="0cefa-184">次の例では、クライアント コードで転送方法を指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="0cefa-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)名前空間には、次のトランスポートを指定に使用できるクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cefa-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="0cefa-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="0cefa-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="0cefa-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="0cefa-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="0cefa-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (サーバーとクライアントの両方が .NET 4.5 を使用する場合にのみ使用できます)。</span><span class="sxs-lookup"><span data-stu-id="0cefa-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="0cefa-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (クライアントとサーバーの両方でサポートされている最適なトランスポートと自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="0cefa-190">これは、既定のトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-190">This is the default transport.</span></span> <span data-ttu-id="0cefa-191">渡すをこれには、`Start`メソッドは何もで渡されていない場合と同様です)。</span><span class="sxs-lookup"><span data-stu-id="0cefa-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="0cefa-192">ブラウザーでのみ使用されているために、ForeverFrame トランスポートはこの一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="0cefa-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="0cefa-193">サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="0cefa-194">トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="0cefa-195">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-195">How to specify HTTP headers</span></span>

<span data-ttu-id="0cefa-196">HTTP ヘッダーを設定するには、使用、`Headers`接続オブジェクトのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="0cefa-197">次の例では、HTTP ヘッダーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="0cefa-198">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-198">How to specify client certificates</span></span>

<span data-ttu-id="0cefa-199">クライアント証明書を追加するには、使用、`AddClientCertificate`接続オブジェクトのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="0cefa-200">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-200">How to create the Hub proxy</span></span>

<span data-ttu-id="0cefa-201">ハブは、サーバーから呼び出すことができるクライアントのメソッドを定義するのには、サーバー側ハブのメソッドを呼び出すには、呼び出すことによって、ハブのプロキシを作成`CreateHubProxy`接続オブジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="0cefa-202">文字列に渡す`CreateHubProxy`が、クラスの名前、ハブ、または指定した名前、`HubName`場合は、サーバーで使用されていた 1 つの属性です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="0cefa-203">名前の一致が区別されません。</span><span class="sxs-lookup"><span data-stu-id="0cefa-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="0cefa-204">**サーバーの hub クラス**</span><span class="sxs-lookup"><span data-stu-id="0cefa-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="0cefa-205">**Hub クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="0cefa-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="0cefa-206">使用してハブ クラスを装飾する場合、`HubName`属性、その名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="0cefa-207">**サーバーの hub クラス**</span><span class="sxs-lookup"><span data-stu-id="0cefa-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="0cefa-208">**Hub クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="0cefa-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="0cefa-209">プロキシ オブジェクトは、スレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="0cefa-210">実際には、呼び出した場合`HubConnection.CreateHubProxy`で複数回、同じ`hubName`、同じキャッシュ`IHubProxy`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0cefa-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="0cefa-211">サーバーが呼び出すことができるクライアント上のメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="0cefa-212">サーバーが呼び出すことができるメソッドを定義するには、プロキシを使用`On`メソッドをイベント ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="0cefa-213">メソッド名に一致する小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="0cefa-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="0cefa-214">たとえば、`Clients.All.UpdateStockPrice`サーバーで実行`updateStockPrice`、 `updatestockprice`、または`UpdateStockPrice`クライアントにします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="0cefa-215">別のクライアント プラットフォームには、UI を更新するメソッドのコードを記述する方法のさまざまな要件があります。</span><span class="sxs-lookup"><span data-stu-id="0cefa-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="0cefa-216">WinRT (Windows ストア .NET) クライアントの例を示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="0cefa-217">WPF、Silverlight、およびコンソール アプリケーションの例がで提供される[このトピックの後半の個別のセクション](#wpfsl)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="0cefa-218">パラメーターのないメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-218">Methods without parameters</span></span>

<span data-ttu-id="0cefa-219">処理しているメソッドがパラメーターを持たない場合は、非ジェネリック オーバー ロードを使用して、`On`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0cefa-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="0cefa-220">**パラメーターなしのクライアント メソッドを呼び出すとサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="0cefa-221">**パラメーターなしのサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0cefa-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="0cefa-222">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="0cefa-223">処理しているメソッドは、パラメーターを持ち場合のジェネリック型としてパラメーターの種類の指定、`On`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="0cefa-224">ジェネリック オーバー ロードがあります、`On`メソッドを使用すると、最大 8 個のパラメーター (Windows Phone 7 で 4) を指定します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="0cefa-225">次の例では、1 つのパラメーターに送信され、`UpdateStockPrice`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="0cefa-226">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="0cefa-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="0cefa-227">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="0cefa-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="0cefa-228">**パラメーターを持つサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0cefa-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="0cefa-229">動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="0cefa-230">ジェネリック型としてパラメーターを指定する代わりに、`On`メソッドを動的オブジェクトとしてパラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="0cefa-231">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="0cefa-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="0cefa-232">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="0cefa-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="0cefa-233">**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="0cefa-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="0cefa-234">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-234">How to remove a handler</span></span>

<span data-ttu-id="0cefa-235">削除するハンドラーを呼び出してその`Dispose`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="0cefa-236">**サーバーからメソッド用のクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="0cefa-237">**クライアント コードで、ハンドラーを削除するには**</span><span class="sxs-lookup"><span data-stu-id="0cefa-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="0cefa-238">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-238">How to call server methods from the client</span></span>

<span data-ttu-id="0cefa-239">サーバー上のメソッドを呼び出すを使用して、`Invoke`ハブ プロキシのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="0cefa-240">サーバー メソッドが戻り値を持たない場合は、非ジェネリック オーバー ロードを使用して、`Invoke`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="0cefa-241">**メソッドの戻り値がないサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="0cefa-242">**戻り値を持たないメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="0cefa-243">サーバー メソッドの戻り値の場合は、戻り値の型を指定のジェネリック型として、`Invoke`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="0cefa-244">**サーバー コードを戻り値を持つ複合型パラメーターを受け取るメソッド**</span><span class="sxs-lookup"><span data-stu-id="0cefa-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="0cefa-245">**パラメーターと戻り値に使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="0cefa-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="0cefa-246">**戻り値を持ち、ASP.NET 4.5 非同期メソッドでは、複合型パラメーターを受け取るメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="0cefa-247">**戻り値を同期メソッドの複合型パラメーターを受け取るメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="0cefa-248">`Invoke`メソッドが非同期的に実行し、返します、`Task`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0cefa-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="0cefa-249">指定しない場合は`await`または`.Wait()`、呼び出すメソッドの実行が完了する前に、次のコード行は実行されます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="0cefa-250">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="0cefa-251">SignalR では、有効期間イベントを処理することができますを次の接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="0cefa-252">`Received`: 接続ですべてのデータが受信したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="0cefa-253">受信したデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-253">Provides the received data.</span></span>
- <span data-ttu-id="0cefa-254">`ConnectionSlow`: クライアントが低速または頻繁に削除の接続を検出したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="0cefa-255">`Reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="0cefa-256">`Reconnected`: 基になるトランスポートが再接続されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="0cefa-257">`StateChanged`: 接続の状態が変更されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="0cefa-258">以前の状態と新しい状態を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-258">Provides the old state and the new state.</span></span> <span data-ttu-id="0cefa-259">接続状態の値について[表す ConnectionState 列挙](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="0cefa-260">`Closed`: 接続を切断したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="0cefa-261">たとえば、致命的ではありませんが、断続的な接続の問題が発生したエラーを示す警告メッセージを表示する場合は、パフォーマンスの低下や頻繁に行われるなど、接続の削除処理、`ConnectionSlow`イベント。</span><span class="sxs-lookup"><span data-stu-id="0cefa-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="0cefa-262">詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)です。</span><span class="sxs-lookup"><span data-stu-id="0cefa-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="0cefa-263">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-263">How to handle errors</span></span>

<span data-ttu-id="0cefa-264">場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、エラーが発生した SignalR を返す例外オブジェクトには、エラーについての最小限の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cefa-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="0cefa-265">たとえばへの呼び出し`newContosoChatMessage`失敗した場合、エラー オブジェクト内のエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを実稼働環境でクライアントには推奨されませんトラブルシューティングの目的では、サーバーで次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="0cefa-266">SignalR が発生したエラーを処理するためのハンドラーを追加できる、`Error`接続オブジェクトのイベントです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="0cefa-267">メソッドの呼び出しからのエラーを処理するには、try catch ブロックでコードをラップします。</span><span class="sxs-lookup"><span data-stu-id="0cefa-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="0cefa-268">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="0cefa-268">How to enable client-side logging</span></span>

<span data-ttu-id="0cefa-269">クライアント側のログ記録を有効にするには設定、`TraceLevel`と`TraceWriter`接続オブジェクトのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="0cefa-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="0cefa-270">WPF、Silverlight、および、サーバーを呼び出すことができるクライアント メソッドのコンソール アプリケーションのコード サンプル</span><span class="sxs-lookup"><span data-stu-id="0cefa-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="0cefa-271">サーバーが呼び出すことができるクライアント メソッドを定義する前に示したコード サンプルは、WinRT クライアントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="0cefa-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="0cefa-272">次のサンプルでは、WPF、Silverlight、およびクライアントのコンソール アプリケーションのコードは同等のコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="0cefa-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="0cefa-273">パラメーターのないメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-273">Methods without parameters</span></span>

<span data-ttu-id="0cefa-274">**パラメーターなしのサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="0cefa-275">**パラメーターなしのサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="0cefa-276">**パラメーターなしのサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="0cefa-277">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="0cefa-278">**パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="0cefa-279">**パラメーターを持つサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="0cefa-280">**パラメーターを持つサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="0cefa-281">動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="0cefa-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="0cefa-282">**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0cefa-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="0cefa-283">**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="0cefa-284">**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="0cefa-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]