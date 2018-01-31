---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します (12/12) のトラブルシューティング |。Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a><span data-ttu-id="d8863-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します (12/12) のトラブルシューティング。</span><span class="sxs-lookup"><span data-stu-id="d8863-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Troubleshooting (12 of 12)</span></span>
====================
<span data-ttu-id="d8863-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d8863-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d8863-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d8863-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="d8863-106">この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d8863-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="d8863-107">Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d8863-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="d8863-108">系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="d8863-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示しています、および Windows Azure Web サイトを展開する方法を示しますをチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


<span data-ttu-id="d8863-110">このページでは、Visual Studio を使用して、ASP.NET web アプリケーションを展開するときに発生する可能性がある一般的な問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="d8863-110">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="d8863-111">1 つの 1 つ以上の考えられる原因と対応するソリューションは提供されます。</span><span class="sxs-lookup"><span data-stu-id="d8863-111">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="d8863-112">サーバー エラーからリモートで表示されている現在のカスタム エラー設定が、エラーの詳細を防ぐため '/' アプリケーション -</span><span class="sxs-lookup"><span data-stu-id="d8863-112">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-113">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-113">Scenario</span></span>

<span data-ttu-id="d8863-114">リモート ホストに、サイトを展開した後は、Web.config ファイルで customErrors 設定特記しているエラーの実際の原因が示されないエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="d8863-114">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-115">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-115">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-116">既定では、web アプリケーションがローカル コンピューターで実行されている場合にのみ、ASP.NET は詳細なエラー情報を示します。</span><span class="sxs-lookup"><span data-stu-id="d8863-116">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="d8863-117">一般に、web アプリケーションは、ハッカーがこの情報を使用して、アプリケーションでの脆弱性を検索することができますので、インターネット経由で公開されている使用可能なときに詳細なエラー情報を表示するしたくありません。</span><span class="sxs-lookup"><span data-stu-id="d8863-117">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="d8863-118">ただし、サイトに、サイトまたは更新プログラムを展開することがあります正しくないものになるし、実際のエラー メッセージを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-118">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="d8863-119">リモート ホストで実行されるときに、詳細なエラー メッセージを表示するアプリケーションを有効にするに設定する Web.config ファイルを編集`customErrors`モードがオフ、アプリケーションを再展開およびアプリケーションを再実行します。</span><span class="sxs-lookup"><span data-stu-id="d8863-119">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set `customErrors` mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="d8863-120">アプリケーションの Web.config ファイルがある場合、`customErrors`内の要素、`system.web`要素、変更、`mode`属性を"off"にします。</span><span class="sxs-lookup"><span data-stu-id="d8863-120">If the application Web.config file has a `customErrors` element in the `system.web` element, change the `mode` attribute to "off".</span></span> <span data-ttu-id="d8863-121">それ以外の場合、追加、`customErrors`内の要素、`system.web`を持つ要素、`mode`属性では、次の例のようにを"off"に設定。</span><span class="sxs-lookup"><span data-stu-id="d8863-121">Otherwise add a `customErrors` element in the `system.web` element with the `mode` attribute set to "off", as shown in the following example:</span></span>

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. <span data-ttu-id="d8863-122">アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="d8863-122">Deploy the application.</span></span>
3. <span data-ttu-id="d8863-123">アプリケーションを実行し、すべて以前に行っていたエラーが発生の原因となったを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="d8863-123">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="d8863-124">ここでは、実際のエラー メッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="d8863-124">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="d8863-125">エラーを解決したら、復元元`customErrors`設定とアプリケーションを再配置します。</span><span class="sxs-lookup"><span data-stu-id="d8863-125">When you have resolved the error, restore the original `customErrors` setting and redeploy the application.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="d8863-126">使用して SQL Server Compact、Web ページにアクセスが拒否されました</span><span class="sxs-lookup"><span data-stu-id="d8863-126">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-127">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-127">Scenario</span></span>

<span data-ttu-id="d8863-128">SQL Server Compact を使用するサイトを展開すると、データベースにアクセスする配置済みのサイトでページを実行する、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-128">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-129">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-129">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-130">サーバー上のネットワーク サービス アカウントをされている SQL サービス Compact のネイティブ バイナリを読み取ることができる必要があります、 *bin\amd64*または*bin\x86*フォルダーが、これは読み取り権限がないそれらのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8863-130">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="d8863-131">ネットワーク サービスに対するアクセス許可の読み取りの設定、 *bin*フォルダー、サブフォルダーへのアクセス許可を拡張することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d8863-131">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="d8863-132">十分なアクセス許可により、構成ファイルを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="d8863-132">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-133">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-133">Scenario</span></span>

<span data-ttu-id="d8863-134">クリックすると、Visual Studio 公開 ボタンをローカル コンピューター上の IIS にアプリケーションを展開する発行が失敗して、**出力**ウィンドウに次のようなエラー メッセージが表示。</span><span class="sxs-lookup"><span data-stu-id="d8863-134">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-135">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-135">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-136">1 回のクリックを使用するローカル コンピューターに IIS にパブリッシュする場合、管理者権限を持つ Visual Studio を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-136">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="d8863-137">Visual Studio を閉じ、管理者のアクセス許可を再起動します。</span><span class="sxs-lookup"><span data-stu-id="d8863-137">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="d8863-138">対象のコンピューターに接続できませんでした.指定されたプロセスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d8863-138">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-139">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-139">Scenario</span></span>

<span data-ttu-id="d8863-140">クリックすると、Visual Studio 公開 ボタン、アプリケーションの展開は失敗を発行し、**出力**ウィンドウに次のようなエラー メッセージが表示。</span><span class="sxs-lookup"><span data-stu-id="d8863-140">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-141">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-141">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-142">プロキシ サーバーは、移行先サーバーとの通信を中断することです。</span><span class="sxs-lookup"><span data-stu-id="d8863-142">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="d8863-143">Windows コントロール パネルから、または Internet Explorer では、**インターネット オプション**を選択し、**接続**タブです。**インターネット プロパティ**ダイアログ ボックスで、をクリックして**LAN の設定**です。</span><span class="sxs-lookup"><span data-stu-id="d8863-143">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="d8863-144">**ローカル エリア ネットワーク (LAN) 設定**ダイアログ ボックスで、クリア、**設定を自動的に検出**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="d8863-144">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="d8863-145">[発行] ボタンを再度クリックします。</span><span class="sxs-lookup"><span data-stu-id="d8863-145">Then click the publish button again.</span></span>

<span data-ttu-id="d8863-146">問題が解決しない場合は、プロキシまたはファイアウォールの設定で実行できる、システム管理者に問い合わせてください。</span><span class="sxs-lookup"><span data-stu-id="d8863-146">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="d8863-147">Web Deploy Web 管理サービスの展開 (8172); の非標準ポートを使用するために、問題が発生します。他の接続では、Web Deploy はポート 80 を使用します。</span><span class="sxs-lookup"><span data-stu-id="d8863-147">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="d8863-148">サード パーティのホスティング プロバイダーに配置するときに、Web 管理サービスを通常を使用しています。</span><span class="sxs-lookup"><span data-stu-id="d8863-148">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="d8863-149">既定の .NET 4.0 アプリケーション プールが存在しません</span><span class="sxs-lookup"><span data-stu-id="d8863-149">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-150">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-150">Scenario</span></span>

<span data-ttu-id="d8863-151">.NET Framework 4 が必要なアプリケーションを展開するときに、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-151">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-152">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-152">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-153">IIS で ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="d8863-153">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="d8863-154">展開しているサーバーは、開発用コンピューターに Visual Studio 2010 がインストールする場合は、ASP.NET 4 はコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-154">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="d8863-155">展開しているサーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d8863-155">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

<span data-ttu-id="d8863-156">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-156">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="d8863-157">詳細については、次を参照してください。、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d8863-157">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="d8863-158">初期化文字列の形式は、インデックス 0 から始まる仕様に準拠していません。</span><span class="sxs-lookup"><span data-stu-id="d8863-158">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-159">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-159">Scenario</span></span>

<span data-ttu-id="d8863-160">1 回のクリックを使用してアプリケーションを配置した後を発行、次のエラー メッセージを取得するデータベースにアクセスするページを実行する場合。</span><span class="sxs-lookup"><span data-stu-id="d8863-160">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-161">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-161">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-162">開く、 *Web.config*ファイルで、展開されたサイトと接続文字列の値がで始まるかどうかを確認`$(ReplacableToken_`次の例のように、します。</span><span class="sxs-lookup"><span data-stu-id="d8863-162">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with `$(ReplacableToken_`, as in the following example:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

<span data-ttu-id="d8863-163">接続文字列は、この例のように、プロジェクト ファイルを編集し、次のプロパティを追加、`PropertyGroup`すべてのビルド構成にある要素。</span><span class="sxs-lookup"><span data-stu-id="d8863-163">If the connection strings look like this example, edit the project file and add the following property to the `PropertyGroup` element that is for all build configurations:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

<span data-ttu-id="d8863-164">アプリケーションを再展開します。</span><span class="sxs-lookup"><span data-stu-id="d8863-164">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="d8863-165">HTTP 500 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-165">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-166">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-166">Scenario</span></span>

<span data-ttu-id="d8863-167">配置済みのサイトを実行すると、エラーの原因を示す特定の情報がない場合、次のエラー メッセージが表示します。</span><span class="sxs-lookup"><span data-stu-id="d8863-167">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-168">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-168">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-169">500 のエラーの原因の多くが、これらのチュートリアルに従う場合は、考えられる原因の 1 つは XML 変換ファイルのいずれかで間違った場所では、XML 要素を配置すること。</span><span class="sxs-lookup"><span data-stu-id="d8863-169">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the XML transformation files.</span></span> <span data-ttu-id="d8863-170">挿入する変換を配置する場合にこのエラーが得られるなど、`<location>`要素の下`<system.web>`の代わりに直下`<configuration>`です。</span><span class="sxs-lookup"><span data-stu-id="d8863-170">For example, you would get this error if you put the transformation that inserts a `<location>` element under `<system.web>` instead of directly under `<configuration>`.</span></span> <span data-ttu-id="d8863-171">ソリューションは、XML 変換ファイルを修正して再配置する場合です。</span><span class="sxs-lookup"><span data-stu-id="d8863-171">The solution in that case is to correct the XML transformation file and redeploy.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="d8863-172">HTTP 500.21 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-172">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-173">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-173">Scenario</span></span>

<span data-ttu-id="d8863-174">配置済みのサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-174">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-175">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-175">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-176">サイトが、ASP.NET 4 で、ASP.NET 4 に登録されていない IIS サーバー上のターゲットを展開しました。</span><span class="sxs-lookup"><span data-stu-id="d8863-176">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="d8863-177">サーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、ASP.NET 4 を登録します。</span><span class="sxs-lookup"><span data-stu-id="d8863-177">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

<span data-ttu-id="d8863-178">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-178">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="d8863-179">詳細については、次を参照してください。、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d8863-179">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="d8863-180">アプリには、SQL Server Express のデータベースを開くはログインできませんでした\_データ</span><span class="sxs-lookup"><span data-stu-id="d8863-180">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-181">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-181">Scenario</span></span>

<span data-ttu-id="d8863-182">更新する、 *Web.config*ファイルとして SQL Server Express データベースを指す接続文字列、 *.mdf*ファイルで、*アプリ\_データ*フォルダー、および最初次のエラー メッセージが表示アプリケーションを実行する時間:</span><span class="sxs-lookup"><span data-stu-id="d8863-182">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-183">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-183">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-184">名前、 *.mdf*ファイルは、削除した場合でもがこれまで、コンピューター上に存在していた SQL Server Express のデータベースの名前に一致ことはできません、 *.mdf*が既存のデータベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8863-184">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="d8863-185">名前を変更、 *.mdf*変更とデータベースの名前として使用されていない名前にファイル、 *Web.config*ファイルを新しい名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="d8863-185">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="d8863-186">代わりに、使用することができます[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存の SQL Server Express を削除するデータベースします。</span><span class="sxs-lookup"><span data-stu-id="d8863-186">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="d8863-187">チェックするモデルの互換性ことはできません。</span><span class="sxs-lookup"><span data-stu-id="d8863-187">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-188">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-188">Scenario</span></span>

<span data-ttu-id="d8863-189">更新する、 *Web.config*ファイルを新しい SQL Server Express データベースを指す接続文字列と初めてアプリケーションを実行する次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="d8863-189">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-190">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-190">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-191">Web.config ファイルに配置するデータベース名がこれまで使用された場合、コンピューターにデータベースが既に存在するいくつかのテーブルを含む前にします。</span><span class="sxs-lookup"><span data-stu-id="d8863-191">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="d8863-192">前に、のコンピューターや変更に使用されていない新しい名前を選択、 *Web.config*ファイルをこの新しいデータベース名を使用 をポイントします。</span><span class="sxs-lookup"><span data-stu-id="d8863-192">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="d8863-193">代わりに、使用することができます[SQL Server Express ユーティリティ](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)または[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存のデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="d8863-193">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="d8863-194">スクリプトがユーザーまたはロールを作成しようとしたときに、SQL エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-194">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-195">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-195">Scenario</span></span>

<span data-ttu-id="d8863-196">構成されているデータベースの配置を使用している、**パッケージ化/発行 SQL**  タブで、展開時に実行する SQL スクリプトを含める Create User またはロールの作成のコマンドとスクリプトの実行が失敗したこれらのコマンドを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="d8863-196">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="d8863-197">次のように、メッセージの詳細を参照して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-197">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

<span data-ttu-id="d8863-198">このエラーが発生のデータベースの配置を構成したときに、 **Web の発行**ウィザードではなく、**パッケージ化/発行 SQL**  タブで、内のスレッドを作成、[構成と展開](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)フォーラム、およびソリューションは、このトラブルシューティングのページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d8863-198">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-199">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-199">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-200">展開の実行を使用しているユーザー アカウントには、ユーザーまたはロールを作成する権限がありません。</span><span class="sxs-lookup"><span data-stu-id="d8863-200">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="d8863-201">たとえば、ホスティング企業が割り当てることが、 `db_datareader`、 `db_datawriter`、および`db_ddladmin`するを設定するユーザー アカウントをロールします。</span><span class="sxs-lookup"><span data-stu-id="d8863-201">For example, the hosting company might assign the `db_datareader`, `db_datawriter`, and `db_ddladmin` roles to the user account that it sets up for you.</span></span> <span data-ttu-id="d8863-202">これらは、十分なほとんどのデータベース オブジェクトを作成するためのではなくユーザーまたはロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8863-202">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="d8863-203">エラーを回避する方法の 1 つは、データベースの配置からのユーザーおよびロールを除外することでです。</span><span class="sxs-lookup"><span data-stu-id="d8863-203">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="d8863-204">編集することによってこれを行う、`PreSource`次の属性が含まれるように、データベースの要素のスクリプトの生成をされた自動的に。</span><span class="sxs-lookup"><span data-stu-id="d8863-204">You can do this by editing the `PreSource` element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

<span data-ttu-id="d8863-205">編集する方法については、`PreSource`プロジェクト ファイル内の要素を参照してください[する方法: プロジェクト ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-205">For information about how to edit the `PreSource` element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="d8863-206">ユーザーまたはロールの開発用データベースでは、転送先データベース内にある必要がある場合、は、ホスティング プロバイダーに問い合わせてしてください。</span><span class="sxs-lookup"><span data-stu-id="d8863-206">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="d8863-207">配置時にカスタム スクリプトを実行するときに、SQL Server のタイムアウト エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-207">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-208">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-208">Scenario</span></span>

<span data-ttu-id="d8863-209">展開時に、実行するカスタムの SQL スクリプトを指定したし、Web 配置の実行時にそれらはタイムアウトが発生します。</span><span class="sxs-lookup"><span data-stu-id="d8863-209">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-210">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-210">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-211">別のトランザクション モードを持つ複数のスクリプトを実行すると、タイムアウト エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="d8863-211">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="d8863-212">既定では、自動的に生成されたスクリプトは、トランザクションで実行が、カスタム スクリプトはこれはできません。</span><span class="sxs-lookup"><span data-stu-id="d8863-212">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="d8863-213">選択した場合、**または既存のデータベースからスキーマのデータをプル** オプションを選択、**パッケージ化/発行 SQL**  タブで、カスタム SQL スクリプトを追加する場合は、いくつかのスクリプトでのトランザクション設定を変更する必要がありますとようにすべてのスクリプトは、同じトランザクション設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="d8863-213">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="d8863-214">詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトでのデータベースを配置](https://msdn.microsoft.com/library/dd465343.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-214">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="d8863-215">すべてが同じように、トランザクションの設定を構成した引き続きこのエラーが発生した場合、可能な回避策とは別に、スクリプトの実行を開始します。</span><span class="sxs-lookup"><span data-stu-id="d8863-215">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="d8863-216">**データベース スクリプト**グリッドで、**パッケージ化/発行**SQL タブで、、 **Include**タイムアウト エラーが発生したスクリプトのチェック ボックスは、プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="d8863-216">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="d8863-217">移動し、**データベース スクリプト**グリッドで、そのスクリプトの選択**Include**チェック ボックスをオンおよびオフに、 **Include**他のスクリプトのチェック ボックスをします。</span><span class="sxs-lookup"><span data-stu-id="d8863-217">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="d8863-218">続いて、プロジェクトをもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="d8863-218">Then publish the project again.</span></span> <span data-ttu-id="d8863-219">この時間を発行すると、選択したカスタム スクリプトのみを実行します。</span><span class="sxs-lookup"><span data-stu-id="d8863-219">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="d8863-220">サイトのマニフェストのストリーム データはまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="d8863-220">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-221">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-221">Scenario</span></span>

<span data-ttu-id="d8863-222">使用してパッケージをインストールする場合、 *deploy.cmd*ファイルと、 `t` (テスト) オプションでは、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-222">When you are installing a package using the *deploy.cmd* file with the `t` (test) option, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-223">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-223">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-224">エラー メッセージの意味、コマンドが、テスト レポートを生成できません。</span><span class="sxs-lookup"><span data-stu-id="d8863-224">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="d8863-225">ただし、コマンド実行可能性がありますを使用する、 `y` (実際のインストール) オプション。</span><span class="sxs-lookup"><span data-stu-id="d8863-225">However, the command might run if you use the `y` (actual installation) option.</span></span> <span data-ttu-id="d8863-226">メッセージは、テスト モードでコマンドを実行に問題があることにのみを示します。</span><span class="sxs-lookup"><span data-stu-id="d8863-226">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="d8863-227">このアプリケーションに ManagedRuntimeVersion v4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="d8863-227">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-228">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-228">Scenario</span></span>

<span data-ttu-id="d8863-229">展開しようとすると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-229">When you attempt to deploy, you see the following error message:</span></span>

 <span data-ttu-id="d8863-230">エラー: のストリーム データが、' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' はまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="d8863-230">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span> <span data-ttu-id="d8863-231">'V2.0' に設定された 'managedRuntimeVersion' プロパティを使用しようとしているアプリケーション プールに、します。</span><span class="sxs-lookup"><span data-stu-id="d8863-231">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="d8863-232">このアプリケーションには、'v4.0' が必要です。</span><span class="sxs-lookup"><span data-stu-id="d8863-232">This application requires 'v4.0'.</span></span> 

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-233">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-233">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-234">IIS で ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="d8863-234">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="d8863-235">展開しているサーバーは、開発用コンピューターに Visual Studio 2010 がインストールする場合は、ASP.NET 4 はコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-235">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="d8863-236">展開しているサーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d8863-236">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="d8863-237">Microsoft.Web.Deployment.DeploymentProviderOptions をキャストできません。</span><span class="sxs-lookup"><span data-stu-id="d8863-237">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-238">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-238">Scenario</span></span>

<span data-ttu-id="d8863-239">パッケージを展開しているときに、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-239">When you are deploying a package, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-240">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-240">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-241">Web Deploy 2.0 がインストールされているサーバーに Web 配置 1.1 UI を使用して IIS マネージャーからを展開しようとするとします。</span><span class="sxs-lookup"><span data-stu-id="d8863-241">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="d8863-242">チェック、パッケージをインポートして展開する IIS のリモート管理ツールを使用している場合、**機能利用可能な新しい** ダイアログ ボックスの接続を確立するときにします。</span><span class="sxs-lookup"><span data-stu-id="d8863-242">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="d8863-243">(このダイアログ ボックスがありますのみ表示されます 1 回の接続が最初に確立されている場合。</span><span class="sxs-lookup"><span data-stu-id="d8863-243">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="d8863-244">接続を消去し、最初からやり直す、IIS マネージャーを閉じるし、開始してもう一度入力して`inetmgr /reset`コマンド プロンプトでします)。機能のいずれかがある場合は**Web 展開 UI**、および 8 よりも低いバージョン番号を展開しているサーバーは、Web Deploy のインストールの 1.1 および 2.0 の両方のバージョンを必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-244">To clear the connection and start over, close IIS Manager and start it up again by entering `inetmgr /reset` at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="d8863-245">2.0 をインストールしているクライアントから展開するには、のみ Web Deploy 2.0 インストールがサーバーに必要です。</span><span class="sxs-lookup"><span data-stu-id="d8863-245">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="d8863-246">この問題を解決するのには、ホスティング プロバイダーに連絡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-246">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="d8863-247">SQL Server Compact のネイティブ コンポーネントを読み込めません。</span><span class="sxs-lookup"><span data-stu-id="d8863-247">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-248">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-248">Scenario</span></span>

<span data-ttu-id="d8863-249">配置済みのサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8863-249">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-250">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-250">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-251">展開されたサイトがない*amd64*と*x86*アプリケーションの下にネイティブ アセンブリを含むサブフォルダー *bin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8863-251">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="d8863-252">SQL Server Compact がインストールされているコンピューターでのネイティブ アセンブリ内にある*C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private*です。</span><span class="sxs-lookup"><span data-stu-id="d8863-252">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="d8863-253">Visual Studio プロジェクトに適切なフォルダーに正しいファイルを取得する最善の方法では、NuGet SqlServerCompact パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d8863-253">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="d8863-254">パッケージのインストールにネイティブ アセンブリをコピーするビルド後のスクリプトを追加する*amd64*と*x86*です。</span><span class="sxs-lookup"><span data-stu-id="d8863-254">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="d8863-255">ただし、これらを展開するためには、手動で、プロジェクトに含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-255">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="d8863-256">詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d8863-256">For more information, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="d8863-257">Entity Framework Code First のアプリケーションを展開した後、「パスが正しくありません」エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-257">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-258">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-258">Scenario</span></span>

<span data-ttu-id="d8863-259">SQL Server Compact、アプリ内のファイルにそのデータベースを格納するなど、Entity Framework Code First Migrations と DBMS を使用するアプリケーションを展開する\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8863-259">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="d8863-260">Code First Migrations が、最初の展開の後、データベースを作成するように構成があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-260">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="d8863-261">アプリケーションを実行するときに、次の例のようなエラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="d8863-261">When you run the application you get an error message like the following example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-262">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-262">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-263">コード最初はしようとすると、データベースが、アプリを作成する\_データ フォルダーは存在しません。</span><span class="sxs-lookup"><span data-stu-id="d8863-263">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="d8863-264">任意ファイルがあるいないか、*アプリ\_データ*フォルダーを展開したか、選択したときに**除外アプリ\_データ**上、**パッケージ化/発行 Web**のタブ、**プロジェクト プロパティ**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="d8863-264">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the **Project Properties** window.</span></span> <span data-ttu-id="d8863-265">サーバーにコピーするフォルダーにファイルがない場合、展開プロセス サーバーのフォルダーは作成されません。</span><span class="sxs-lookup"><span data-stu-id="d8863-265">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="d8863-266">展開プロセスがファイルを削除、データベースのサイトのセットアップが既に存在する場合、*アプリ\_データ*フォルダー自体を選択した場合**先で追加ファイルを削除する**で発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="d8863-266">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="d8863-267">問題を解決するで .txt ファイルなどのプレース ホルダー ファイルを格納、*アプリ\_データ*フォルダーがないことを確認**を除外するアプリ\_データ**選択され、再展開します。</span><span class="sxs-lookup"><span data-stu-id="d8863-267">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span> 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="d8863-268">「基になる RCW から分割された COM オブジェクト使用できません。」</span><span class="sxs-lookup"><span data-stu-id="d8863-268">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-269">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-269">Scenario</span></span>

<span data-ttu-id="d8863-270">正常にされているアプリケーションを配置する発行 1 回のクリックを使用して開始してこのエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="d8863-270">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-271">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-271">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-272">閉じると、Visual Studio を再起動は通常このエラーを解決するのには必要なすべてのです。</span><span class="sxs-lookup"><span data-stu-id="d8863-272">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="d8863-273">展開が失敗するためのユーザー資格情報が使用がない公開 setACL 機関</span><span class="sxs-lookup"><span data-stu-id="d8863-273">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-274">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-274">Scenario</span></span>

<span data-ttu-id="d8863-275">示すエラーで失敗する発行フォルダーのアクセス許可 (を使用しているユーザー アカウントは setACL 機関がありません) を設定する権限がありません。</span><span class="sxs-lookup"><span data-stu-id="d8863-275">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-276">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-276">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-277">既定では、Visual Studio の設定、サイトのルート フォルダーに対するアクセス許可の読み取りおよび書き込みアクセス許可アプリ\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8863-277">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="d8863-278">追加することで、この動作を無効にするサイト フォルダの既定のアクセス許可が正しいし、設定する必要はありませんがわかっている場合 **&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 発行プロファイル ファイル (1 つのプロファイルに影響を与える)、または (影響を与えるすべてのプロファイル) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8863-278">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="d8863-279">これらのファイルを編集する方法については、次を参照してください。[する方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-279">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="d8863-280">アプリケーションがアプリケーション フォルダーへの書き込みを行うとき、アクセス拒否エラー</span><span class="sxs-lookup"><span data-stu-id="d8863-280">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-281">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-281">Scenario</span></span>

<span data-ttu-id="d8863-282">アプリケーション エラーそのフォルダーに対する書き込み権限があるないために作成やアプリケーション フォルダーのいずれかのファイルを編集しようとします。</span><span class="sxs-lookup"><span data-stu-id="d8863-282">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-283">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-283">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-284">既定では、Visual Studio の設定、サイトのルート フォルダーに対するアクセス許可の読み取りおよび書き込みアクセス許可アプリ\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d8863-284">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="d8863-285">示すように、そのフォルダーのアクセス許可を設定するアプリケーションでは、サブ フォルダーへの書き込みアクセスを必要とする場合、[フォルダー アクセス許可の設定](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)と[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d8863-285">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the [Setting Folder Permissions](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) and [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorials.</span></span> <span data-ttu-id="d8863-286">ルート フォルダーに追加することで読み取り専用アクセスを設定することを防ぐことがある場合は、アプリケーションでは、サイトのルート フォルダーへの書き込みアクセスが必要な **&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 発行プロファイル ファイル (1 つのプロファイルに影響を与える)、または (影響を与えるすべてのプロファイル) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8863-286">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="d8863-287">これらのファイルを編集する方法については、次を参照してください。[する方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-287">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="d8863-288">構成エラー - targetFramework 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。</span><span class="sxs-lookup"><span data-stu-id="d8863-288">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="d8863-289">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d8863-289">Scenario</span></span>

<span data-ttu-id="d8863-290">ASP.NET 4.5 を対象とする web プロジェクトを正常に発行されたアプリケーションを実行すると、(で、`customErrors`モードは、Web.config ファイルには、「無効」に設定)、次のエラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="d8863-290">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the `customErrors` mode set to "off" in the Web.config file) you get the following error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

<span data-ttu-id="d8863-291">エラー ページの [ソースのエラー] ボックスでは、エラーの原因として web.config ファイルから次の行を示しています。</span><span class="sxs-lookup"><span data-stu-id="d8863-291">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="d8863-292">考えられる原因と解決策</span><span class="sxs-lookup"><span data-stu-id="d8863-292">Possible Cause and Solution</span></span>

<span data-ttu-id="d8863-293">サーバーは、ASP.NET 4.5 をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="d8863-293">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="d8863-294">ASP.NET 4.5 用のサポートを追加できるかどうかを決定するホスティング プロバイダーに問い合わせてください。</span><span class="sxs-lookup"><span data-stu-id="d8863-294">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="d8863-295">ASP.NET 4 以降を対象とする web プロジェクトを配置する必要がある場合は、サーバーをアップグレードすることは、オプションではありません、代わりにします。同じ送信先に ASP.NET 4 または以前の web プロジェクトを展開する場合は、選択、**先で追加ファイルを削除する**チェック ボックスをオン、**設定**のタブ、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="d8863-295">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="d8863-296">選択しない場合**先で追加ファイルを削除する**、引き続き構成エラー ページを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="d8863-296">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="d8863-297">プロジェクト**プロパティ**windows には、ターゲット フレームワーク ドロップダウン リストが含まれていますが、だけを変更することによってこの問題を解決することはできません**.NET Framework 4.5**に**.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="d8863-297">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="d8863-298">以前のフレームワーク バージョンにターゲット フレームワークを変更する場合、プロジェクトが framework の以降のバージョンのアセンブリへの参照が残っているしは実行されません。</span><span class="sxs-lookup"><span data-stu-id="d8863-298">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="d8863-299">手動でこれらの参照を変更または .NET Framework 4 以前を対象とする新しいプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8863-299">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="d8863-300">詳細については、次を参照してください。 [Web サイトの .NET Framework のターゲット](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d8863-300">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d8863-301">前へ</span><span class="sxs-lookup"><span data-stu-id="d8863-301">Previous</span></span>](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)