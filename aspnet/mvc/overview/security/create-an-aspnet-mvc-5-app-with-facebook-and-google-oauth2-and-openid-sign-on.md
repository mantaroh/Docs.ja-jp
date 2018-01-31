---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "作成する MVC 5 アプリを Facebook、Twitter、LinkedIn および Google OAuth2 サイン オン (c#) |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、OAuth 2.0 を使用して認証を使用する外部からの資格情報でログインできるようにする ASP.NET MVC 5 web アプリケーションをビルドする方法を示します."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: ccf4329e6684d07570bfaabfaa1a570664fb2ca3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="860b0-103">Facebook、Twitter、LinkedIn および Google OAuth2 サイン オン (c#) で ASP.NET MVC 5 アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="860b0-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="860b0-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="860b0-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="860b0-105">このチュートリアルを使用してユーザーにログインできるようにする ASP.NET MVC 5 web アプリケーションを構築する方法を示します[OAuth 2.0](http://oauth.net/2/) Facebook、Twitter、LinkedIn、Microsoft、Google など、外部認証プロバイダーからの資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="860b0-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="860b0-106">わかりやすくするため、このチュートリアルは、Facebook、Google からの資格情報の操作について説明します。</span><span class="sxs-lookup"><span data-stu-id="860b0-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="860b0-107">数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるために、大きな利点を提供して web サイトでこれらの資格情報を有効にします。</span><span class="sxs-lookup"><span data-stu-id="860b0-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="860b0-108">これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="860b0-109">関連項目[SMS と 2 要素認証の電子メールを使って ASP.NET MVC 5 アプリ](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="860b0-110">このチュートリアルでは、メンバーシップ API を使用してロールを追加する方法と、ユーザーのプロファイル データを追加する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="860b0-111">このチュートリアルは、によって書き込まれました[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter me に従って操作してください: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="860b0-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="860b0-112">作業の開始</span><span class="sxs-lookup"><span data-stu-id="860b0-112">Getting Started</span></span>

<span data-ttu-id="860b0-113">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="860b0-114">Visual Studio のインストール[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="860b0-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="860b0-115">Dropbox、GitHub、Linkedin、Instagram、バッファー、salesforce、ストリーム、スタック Exchange、Tripit、twitch、Twitter、yahoo などのヘルプを参照してください[ワンストップ ガイド](http://www.oauthforaspnet.com/)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-115">For help with Dropbox, GitHub, Linkedin, Instagram, buffer, salesforce, STEAM, Stack Exchange, Tripit, twitch, Twitter, Yahoo and more, see this [one stop guide](http://www.oauthforaspnet.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="860b0-116">Visual Studio をインストールする必要があります[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)以上 Google OAuth 2 を使用して、SSL の警告せずにローカルでデバッグします。</span><span class="sxs-lookup"><span data-stu-id="860b0-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="860b0-117">をクリックして**新しいプロジェクト**から、**開始** ページで、またはメニューを使用して選択、**ファイル**、し**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="860b0-118">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="860b0-118">Creating Your First Application</span></span>

<span data-ttu-id="860b0-119">をクリックして**新しいプロジェクト**選択してから、 **Visual c#**し、左側の**Web**し、 **ASP.NET Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="860b0-120">プロジェクト"MvcAuth"の名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="860b0-121">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="860b0-122">認証がない場合**個々 のユーザー アカウント**をクリックして、**認証の変更**ボタンをクリックして**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="860b0-123">チェックして**クラウド内のホスト**アプリが非常に簡単に Azure でホストされます。</span><span class="sxs-lookup"><span data-stu-id="860b0-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="860b0-124">選択した場合は**クラウド内のホスト**構成 ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="860b0-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="860b0-125">NuGet を使用して、最新の OWIN ミドルウェアを更新</span><span class="sxs-lookup"><span data-stu-id="860b0-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="860b0-126">更新する NuGet パッケージ マネージャーを使用して、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="860b0-127">選択**更新**左側のメニュー。</span><span class="sxs-lookup"><span data-stu-id="860b0-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="860b0-128">クリックすると、**すべて更新**(の次の図に示す) OWIN パッケージのみのボタンをクリックするかを検索できます。</span><span class="sxs-lookup"><span data-stu-id="860b0-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="860b0-129">次の図では、OWIN パッケージのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="860b0-130">入力したパッケージ マネージャー コンソール (PMC) から、`Update-Package`コマンドでは、すべてのパッケージが更新されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="860b0-131">キーを押して**f5 キーを押して**または**ctrl キーを押しながら f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="860b0-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="860b0-132">次の図では、ポート番号は、1234 です。</span><span class="sxs-lookup"><span data-stu-id="860b0-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="860b0-133">アプリケーションを実行するときに、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="860b0-134">ブラウザー ウィンドウのサイズに応じて、ナビゲーション アイコンを表示する をクリックする必要があります、**ホーム**、**に関する**、**連絡先**、 **登録**と**ログイン**リンクします。</span><span class="sxs-lookup"><span data-stu-id="860b0-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="860b0-135">プロジェクトで SSL を設定します。</span><span class="sxs-lookup"><span data-stu-id="860b0-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="860b0-136">Google、Facebook などの認証プロバイダーに接続するには、SSL を使用する IIS Express を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="860b0-137">ユーザー名とパスワードと、ネットワーク経由でクリア テキストで送信する SSL を使用せずに、シークレットと同様、ログイン cookie が、引き続きログイン後に SSL を使用して、HTTP にバックアップを削除しないに重要です。</span><span class="sxs-lookup"><span data-stu-id="860b0-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="860b0-138">ハンドシェイクを実行し、(であるため、HTTPS HTTP よりも低速の一括) チャネルをセキュリティで保護する時間を既に撮影する以外にも、MVC パイプラインの実行前にログインしているために HTTP リダイレクトする行いません将来、現在の要求要求がはるかに高速です。</span><span class="sxs-lookup"><span data-stu-id="860b0-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="860b0-139">**ソリューション エクスプ ローラー**をクリックして、 **MvcAuth**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="860b0-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="860b0-140">プロジェクトのプロパティを表示する F4 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="860b0-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="860b0-141">またから、**ビュー**メニューを選択できます**プロパティ ウィンドウ**します。</span><span class="sxs-lookup"><span data-stu-id="860b0-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="860b0-142">変更**SSL を有効に**True に設定します。</span><span class="sxs-lookup"><span data-stu-id="860b0-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="860b0-143">SSL URL のコピー (されます`https://localhost:44300/`SSL の他のプロジェクトを作成した場合を除く)。</span><span class="sxs-lookup"><span data-stu-id="860b0-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="860b0-144">**ソリューション エクスプ ローラー**を右クリックして、 **MvcAuth**プロジェクトし、選択**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="860b0-145">選択、 **Web**タブをクリックしに SSL URL を貼り付け、**プロジェクト Url**ボックス。</span><span class="sxs-lookup"><span data-stu-id="860b0-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="860b0-146">(Ctl + S) ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="860b0-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="860b0-147">この URL は、Facebook、Google の認証アプリを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="860b0-148">追加、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)属性を`Home`コント ローラーのすべての要求を要求するのには、HTTPS を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="860b0-149">安全な方法は、追加する、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)アプリケーションをフィルターします。</span><span class="sxs-lookup"><span data-stu-id="860b0-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="860b0-150">セクションを参照して&quot;SSL および承認属性を持つアプリケーションを保護する&quot;my tutoral で[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="860b0-151">Home コント ローラーの一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="860b0-152">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="860b0-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="860b0-153">過去の証明書をインストールしている場合は、このセクションの残りの部分をスキップし、ジャンプする[OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続する](#goog)、それ以外の場合、手順については、自己署名を信頼するにはIIS Express が生成した証明書です。</span><span class="sxs-lookup"><span data-stu-id="860b0-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="860b0-154">読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。</span><span class="sxs-lookup"><span data-stu-id="860b0-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="860b0-155">IE を示しています、*ホーム*ページし、SSL の警告はありません。</span><span class="sxs-lookup"><span data-stu-id="860b0-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="860b0-156">Google Chrome も証明書を受け入れるし、警告を表示せずに HTTPS コンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="860b0-157">警告に表示されるように、Firefox は独自の証明書ストアを使用します。</span><span class="sxs-lookup"><span data-stu-id="860b0-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="860b0-158">アプリケーションを安全にクリックして、**リスクを理解しました**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="860b0-159">OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続します。</span><span class="sxs-lookup"><span data-stu-id="860b0-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

1. <span data-ttu-id="860b0-160">移動し、 [Google Developers Console](https://console.developers.google.com/)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-160">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
1. <span data-ttu-id="860b0-161">前にプロジェクトを作成していない場合は、選択**資格情報**クリックし、左側のタブで**作成**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-161">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
1. <span data-ttu-id="860b0-162">左側のタブをクリックして**資格情報**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-162">In the left tab, click **Credentials**.</span></span>
1. <span data-ttu-id="860b0-163">をクリックして**資格情報を作成する**し**OAuth クライアント ID**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-163">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="860b0-164">**クライアント ID を作成**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。</span><span class="sxs-lookup"><span data-stu-id="860b0-164">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="860b0-165">設定、**承認 JavaScript** SSL URL が上記で使用する配信元 (`https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)</span><span class="sxs-lookup"><span data-stu-id="860b0-165">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="860b0-166">設定、**承認されているリダイレクト URI**に。</span><span class="sxs-lookup"><span data-stu-id="860b0-166">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="860b0-167">OAuth 同意画面のメニュー項目をクリックし、電子メール アドレスと製品名を設定します。</span><span class="sxs-lookup"><span data-stu-id="860b0-167">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="860b0-168">フォームのクリックを完了しているとき**保存**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-168">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="860b0-169">ライブラリのメニュー項目をクリックすると、検索**Google + API**、それをクリックし、有効にするキーを押します。</span><span class="sxs-lookup"><span data-stu-id="860b0-169">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 <span data-ttu-id="860b0-170">次の図は、有効になっている Api を示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-170">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="860b0-171">Google Api API マネージャーからアクセスして、**資格情報**を取得するには、タブ、**クライアント ID**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-171">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="860b0-172">ダウンロードすると、アプリケーション シークレットの JSON ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="860b0-172">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="860b0-173">コピーし、貼り付け、 **ClientId**と**ClientSecret**に、`UseGoogleAuthentication`については、メソッド、 *Startup.Auth.cs*ファイルを*App_Start*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="860b0-173">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="860b0-174">**ClientId**と**ClientSecret**の下に表示される値のサンプルし、動作しません。</span><span class="sxs-lookup"><span data-stu-id="860b0-174">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="860b0-175">セキュリティ - ソース コード内の機密データはストアことはありません。</span><span class="sxs-lookup"><span data-stu-id="860b0-175">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="860b0-176">アカウントと資格情報は、サンプルをシンプルにする上記のコードに追加されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-176">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="860b0-177">参照してください[ASP.NET と Azure App Service へのパスワードなどの機密データの展開のベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-177">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="860b0-178">キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="860b0-178">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="860b0-179">クリックして、**ログイン**リンクします。</span><span class="sxs-lookup"><span data-stu-id="860b0-179">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="860b0-180">下にある**のログインに別のサービスを使用して**をクリックして**Google**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-180">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="860b0-181">上記の手順のいずれかがない場合は、HTTP 401 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-181">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="860b0-182">上記の手順を再確認します。</span><span class="sxs-lookup"><span data-stu-id="860b0-182">Recheck your steps above.</span></span> <span data-ttu-id="860b0-183">必要な設定がない場合 (たとえば**製品名**)、欠落している項目と保存を追加、認証が機能するまで、しばらく時間がかかることができます。</span><span class="sxs-lookup"><span data-stu-id="860b0-183">If you miss a required setting (for example **product name**), add the missing item and save, it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="860b0-184">資格情報を入力する google サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="860b0-184">You will be redirected to the google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="860b0-185">資格情報を入力すると後、は、作成した web アプリケーションへのアクセス許可を付与するよう求められます。</span><span class="sxs-lookup"><span data-stu-id="860b0-185">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="860b0-186">をクリックして**受け入れる**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-186">Click **Accept**.</span></span> <span data-ttu-id="860b0-187">今すぐにリダイレクトされます、**登録**Google アカウントを登録する MvcAuth アプリケーションのページです。</span><span class="sxs-lookup"><span data-stu-id="860b0-187">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="860b0-188">Gmail アカウントで使用するローカル電子メール登録名を変更することがあるが、通常、既定の電子メール エイリアス (つまり、認証に使用するもの) を保持します。</span><span class="sxs-lookup"><span data-stu-id="860b0-188">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="860b0-189">**[登録]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="860b0-189">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="860b0-190">Facebook でアプリを作成し、プロジェクトに、アプリを接続します。</span><span class="sxs-lookup"><span data-stu-id="860b0-190">Creating the app in Facebook and connecting the app to the project</span></span>

<span data-ttu-id="860b0-191">Facebook の OAuth2 認証では、Facebook で作成したアプリケーションから一部の設定をプロジェクトにコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-191">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="860b0-192">ブラウザーでに移動します[https://developers.facebook.com/apps](https://developers.facebook.com/apps)して Facebook 資格情報を入力してログインします。</span><span class="sxs-lookup"><span data-stu-id="860b0-192">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="860b0-193">既に、Facebook 開発者として登録されていない場合にクリックして**開発者として登録**し登録するための指示に従います。</span><span class="sxs-lookup"><span data-stu-id="860b0-193">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="860b0-194">**アプリ** タブで、をクリックして**Create New App**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-194">On the **Apps** tab, click **Create New App**.</span></span>

    ![新しいアプリを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="860b0-196">入力、**アプリ名**と**カテゴリ**、順にクリックして**のアプリの作成**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-196">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="860b0-197">Facebook で一意であるこの必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-197">This must be unique across Facebook.</span></span> <span data-ttu-id="860b0-198">**アプリ Namespace**を使って、アプリは Facebook アプリケーションの認証 (たとえば、https://apps.facebook.com/{アプリ Namespace}) にアクセスする URL の一部です。</span><span class="sxs-lookup"><span data-stu-id="860b0-198">The **App Namespace** is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="860b0-199">指定しない場合は、**アプリ Namespace**、**アプリ ID** URL に使用されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-199">If you don't specify an **App Namespace**, the **App ID** will be used for the URL.</span></span> <span data-ttu-id="860b0-200">**アプリ ID**はシステムによって生成された時間の数が次の手順で表示されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-200">The **App ID** is a long system-generated number that you will see in the next step.</span></span>

    ![新しいアプリ ダイアログを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="860b0-202">標準的なセキュリティ チェックを送信します。</span><span class="sxs-lookup"><span data-stu-id="860b0-202">Submit the standard security check.</span></span>

    ![セキュリティ チェック](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="860b0-204">選択**設定**左側のメニュー バーの![Facebook 開発者のメニュー バー](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="860b0-204">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="860b0-205">**基本**ページの設定 セクションを選択**プラットフォームを追加**を web サイト アプリケーションを追加することを指定します。</span><span class="sxs-lookup"><span data-stu-id="860b0-205">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="860b0-206">![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="860b0-206">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="860b0-207">選択**web サイト**のプラットフォームのいずれか。</span><span class="sxs-lookup"><span data-stu-id="860b0-207">Select **Website** from the platform choices.</span></span>  
  
    ![プラットフォームの選択肢](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="860b0-209">メモ、**アプリ ID**と**アプリ シークレット**できるように、このチュートリアルで後ほど、MVC アプリケーションに両方を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="860b0-209">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="860b0-210">また、サイトの URL を追加 (`https://localhost:44300/`)、MVC アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="860b0-210">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="860b0-211">また、追加、 **Contact Email**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-211">Also, add a **Contact Email**.</span></span> <span data-ttu-id="860b0-212">次に、選択**Save Changes**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-212">Then, select **Save Changes**.</span></span>   

    ![基本的なアプリケーションの詳細 ページ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="860b0-214">登録されている電子メールのエイリアスを使用して認証だけあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="860b0-214">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="860b0-215">その他のユーザーとテスト用のアカウントを登録することができなきます。</span><span class="sxs-lookup"><span data-stu-id="860b0-215">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="860b0-216">Facebook のアプリケーションにその他の Facebook アカウントのアクセスを許可できます**開発者ロール**タブです。</span><span class="sxs-lookup"><span data-stu-id="860b0-216">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="860b0-217">Visual Studio で開く*アプリ\_Start\Startup.Auth.cs*です。</span><span class="sxs-lookup"><span data-stu-id="860b0-217">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="860b0-218">コピーし、貼り付け、 **AppId**と**アプリ シークレット**に、`UseFacebookAuthentication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="860b0-218">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="860b0-219">**AppId**と**アプリ シークレット**の下に表示される値のサンプルし、は機能しません。</span><span class="sxs-lookup"><span data-stu-id="860b0-219">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="860b0-220">をクリックして**変更を保存**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-220">Click **Save Changes**.</span></span>
13. <span data-ttu-id="860b0-221">キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="860b0-221">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="860b0-222">選択**ログイン**ログイン ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-222">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="860b0-223">をクリックして**Facebook** **のログインに別のサービスを使用します。**</span><span class="sxs-lookup"><span data-stu-id="860b0-223">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="860b0-224">Facebook 資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="860b0-224">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="860b0-225">パブリック プロファイルと友人リストにアクセスするためにアプリケーションのアクセス許可を付与するように促されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-225">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook アプリの詳細](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="860b0-227">ログインしているようになりました。</span><span class="sxs-lookup"><span data-stu-id="860b0-227">You are now logged in.</span></span> <span data-ttu-id="860b0-228">アプリケーションでこのアカウントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="860b0-228">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="860b0-229">エントリを追加登録するときに、*ユーザー*メンバーシップ データベースのテーブルです。</span><span class="sxs-lookup"><span data-stu-id="860b0-229">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="860b0-230">メンバーシップ データを調べる</span><span class="sxs-lookup"><span data-stu-id="860b0-230">Examine the Membership Data</span></span>

<span data-ttu-id="860b0-231">**ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-231">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="860b0-232">展開**DefaultConnection (MvcAuth)**、展開**テーブル**を右クリックして**AspNetUsers**  をクリック**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-232">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers テーブルのデータ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="860b0-234">プロファイル データ、ユーザー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="860b0-234">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="860b0-235">このセクションで追加の生年月日とホーム町のユーザー データを登録の際に次の図のようにします。</span><span class="sxs-lookup"><span data-stu-id="860b0-235">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![ホーム町と Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="860b0-237">開く、 *Models\IdentityModels.cs*ファイル birth date とホーム町のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="860b0-237">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="860b0-238">開く、 *Models\AccountViewModels.cs*ファイルとセット birth で日付とホームの町プロパティ`ExternalLoginConfirmationViewModel`です。</span><span class="sxs-lookup"><span data-stu-id="860b0-238">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="860b0-239">開く、 *Controllers\AccountController.cs*ファイルし、の生年月日の日付とホーム町にコードを追加、`ExternalLoginConfirmation`ようにアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="860b0-239">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="860b0-240">生年月日と出身地に追加、 *Views\Account\ExternalLoginConfirmation.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="860b0-240">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="860b0-241">もう一度アプリケーションを使用して、Facebook アカウントを登録し、新しい生年月日とホーム町プロファイル情報を追加することを確認できるように、メンバーシップ データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="860b0-241">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="860b0-242">**ソリューション エクスプ ローラー**をクリックして、 **[すべてのファイル**アイコン、し、右クリック*追加\_Data\aspnet-MvcAuth -&lt;日付スタンプ&gt;.mdf* ] をクリック**削除**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-242">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="860b0-243">**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**をクリックし、 **Package Manager Console** (PMC)。</span><span class="sxs-lookup"><span data-stu-id="860b0-243">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="860b0-244">PMC に次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="860b0-244">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="860b0-245">有効な移行</span><span class="sxs-lookup"><span data-stu-id="860b0-245">Enable-Migrations</span></span>
2. <span data-ttu-id="860b0-246">追加の移行の初期化</span><span class="sxs-lookup"><span data-stu-id="860b0-246">Add-Migration Init</span></span>
3. <span data-ttu-id="860b0-247">データベースの更新</span><span class="sxs-lookup"><span data-stu-id="860b0-247">Update-Database</span></span>

<span data-ttu-id="860b0-248">アプリケーションを実行し、FaceBook、Google を使用してログインし、一部のユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="860b0-248">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="860b0-249">メンバーシップ データを調べる</span><span class="sxs-lookup"><span data-stu-id="860b0-249">Examine the Membership Data</span></span>

<span data-ttu-id="860b0-250">**ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-250">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="860b0-251">右クリックして**AspNetUsers**  をクリック**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="860b0-251">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="860b0-252">`HomeTown`と`BirthDate`フィールドを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-252">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="860b0-253">アプリからログオフして、別のアカウントでのログ記録</span><span class="sxs-lookup"><span data-stu-id="860b0-253">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="860b0-254">Facebook、およびでのアプリへのログオンをログアウトしとにログインしようとしています。 もう一度 (同じブラウザーを使用)、別の Facebook アカウントではログインする必要がすぐに使用した前の Facebook アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="860b0-254">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="860b0-255">別のアカウントを使用するのには、Facebook に移動し、Facebook にログアウトする必要があります。</span><span class="sxs-lookup"><span data-stu-id="860b0-255">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="860b0-256">その他のサード パーティ認証プロバイダーに、同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="860b0-256">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="860b0-257">別のブラウザーを使用して、別のアカウントでログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="860b0-257">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="860b0-258">次の手順</span><span class="sxs-lookup"><span data-stu-id="860b0-258">Next Steps</span></span>

<span data-ttu-id="860b0-259">参照してください[OWIN の Yahoo および LinkedIn OAuth のセキュリティ プロバイダーの導入](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)Jerrie Pelser Yahoo および LinkedIn の手順でします。</span><span class="sxs-lookup"><span data-stu-id="860b0-259">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="860b0-260">Jerrie を参照してください。 非常に有効にするソーシャル ログイン ボタンを取得する ASP.NET MVC 5 のソーシャル ログイン ボタン。</span><span class="sxs-lookup"><span data-stu-id="860b0-260">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="860b0-261">チュートリアルに従って[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)、このチュートリアルを続行して、次に示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-261">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="860b0-262">アプリを Azure に配置する方法。</span><span class="sxs-lookup"><span data-stu-id="860b0-262">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="860b0-263">役割を持つアプリをセキュリティで保護する方法。</span><span class="sxs-lookup"><span data-stu-id="860b0-263">How to secure you app with roles.</span></span>
3. <span data-ttu-id="860b0-264">使用してアプリを保護する方法、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)と[Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)フィルター。</span><span class="sxs-lookup"><span data-stu-id="860b0-264">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="860b0-265">メンバーシップ API を使用して、ユーザーおよびロールを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="860b0-265">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="860b0-266">このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="860b0-266">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="860b0-267">新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-267">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="860b0-268">要求し、ASP.NET に追加する新しい機能に投票もできます。</span><span class="sxs-lookup"><span data-stu-id="860b0-268">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="860b0-269">などのツールには、返信できます[を作成し、ユーザーおよびロールを管理します。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="860b0-269">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="860b0-270">ASP.NET の外部認証サービスの動作の適切な詳細については、Robert McMurray を参照してください。[外部認証サービス](https://asp.net/web-api/overview/security/external-authentication-services)です。</span><span class="sxs-lookup"><span data-stu-id="860b0-270">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="860b0-271">Robert の記事は、Microsoft および Twitter 認証を有効にする方法の詳細にも移動します。</span><span class="sxs-lookup"><span data-stu-id="860b0-271">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="860b0-272">Tom Dykstra の優れた[EF と MVC チュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Entity Framework を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="860b0-272">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>