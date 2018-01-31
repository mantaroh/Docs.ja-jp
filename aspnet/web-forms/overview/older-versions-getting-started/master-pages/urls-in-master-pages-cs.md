---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: "マスター ページ (c#) 内の Url |Microsoft ドキュメント"
author: rick-anderson
description: "マスター ページ ファイルがコンテンツ ページとは異なる相対ディレクトリのため、マスター ページ内の Url が壊れる可能性がどのように対処します。 リベースを見ます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b01f0ac780121c4e0941df6016220a1cb1ed2d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-c"></a><span data-ttu-id="a9afa-104">マスター ページ (c#) 内の Url</span><span class="sxs-lookup"><span data-stu-id="a9afa-104">URLs in Master Pages (C#)</span></span>
====================
<span data-ttu-id="a9afa-105">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="a9afa-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="a9afa-106">[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a9afa-106">[Download Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) or [Download PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)</span></span>

> <span data-ttu-id="a9afa-107">マスター ページ ファイルがコンテンツ ページとは異なる相対ディレクトリのため、マスター ページ内の Url が壊れる可能性がどのように対処します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-107">Addresses how URLs in the master page can break due to the master page file being in a different relative directory than the content page.</span></span> <span data-ttu-id="a9afa-108">使用して Url を再配置を見ます ~ 宣言の構文と ResolveUrl と ResolveClientUrl をプログラムで使用します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-108">Looks at rebasing URLs via ~ in the declarative syntax and using ResolveUrl and ResolveClientUrl programmatically.</span></span> <span data-ttu-id="a9afa-109">(も見てください。</span><span class="sxs-lookup"><span data-stu-id="a9afa-109">(Also look at</span></span>


## <a name="introduction"></a><span data-ttu-id="a9afa-110">はじめに</span><span class="sxs-lookup"><span data-stu-id="a9afa-110">Introduction</span></span>

<span data-ttu-id="a9afa-111">すべての例で見たこれまでは、マスターおよびコンテンツのページは、同じフォルダー (web サイトのルート フォルダー) に配置されています。</span><span class="sxs-lookup"><span data-stu-id="a9afa-111">In all the examples we've seen thus far, the master and content pages have been located in the same folder (the website's root folder).</span></span> <span data-ttu-id="a9afa-112">しかし、理由、マスター ページとコンテンツ ページが、同じフォルダーにあります。 理由はありません。</span><span class="sxs-lookup"><span data-stu-id="a9afa-112">But there's no reason why the master and content pages must be in the same folder.</span></span> <span data-ttu-id="a9afa-113">サブフォルダーにコンテンツ ページを確実に作成できます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-113">You can certainly create content pages in subfolders.</span></span> <span data-ttu-id="a9afa-114">同様に、作成する場合があります、`~/MasterPages/`サイトのマスター ページを配置するフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-114">Similarly, you might create a `~/MasterPages/` folder where you place your site's master pages.</span></span>

<span data-ttu-id="a9afa-115">別のフォルダーのコンテンツとマスター ページを配置することで潜在的な問題 1 つにはには、切断された Url が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-115">One potential issue with placing master and content pages in different folders involves broken URLs.</span></span> <span data-ttu-id="a9afa-116">マスター ページには、ハイパーリンク、画像、またはその他の要素の相対 Url が含まれています、リンクは、別のフォルダー内に存在するコンテンツ ページの有効なされません。</span><span class="sxs-lookup"><span data-stu-id="a9afa-116">If the master page contains relative URLs in hyperlinks, images, or other elements, the link will be invalid for content pages that reside in a different folder.</span></span> <span data-ttu-id="a9afa-117">このチュートリアルでは、この問題と回避策のソースを確認します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-117">In this tutorial we examine the source of this problem as well as workarounds.</span></span>

## <a name="the-problem-with-relative-urls"></a><span data-ttu-id="a9afa-118">相対 Url での問題</span><span class="sxs-lookup"><span data-stu-id="a9afa-118">The Problem with Relative URLs</span></span>

<span data-ttu-id="a9afa-119">Web ページの URL があると言われます、*相対 URL*場合は、web サイトのフォルダー構造内の web ページの場所に対する相対パスを指しているリソースの場所です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-119">A URL on a web page is said to be a *relative URL* if the location of the resource it points to is relative to the web page's location in the website's folder structure.</span></span> <span data-ttu-id="a9afa-120">先頭のスラッシュで始まっていない任意の URL (`/`) またはプロトコル (など`http://`) では相対 URL を含む web ページの場所に基づき、ブラウザーでは解決します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-120">Any URL that does not start with a leading forward slash (`/`) or a protocol (such as `http://`) is relative because it is resolved by the browser based on the location of the web page that contains the URL.</span></span>

<span data-ttu-id="a9afa-121">たとえば、当社の web サイトには、`~/Images/`単一のイメージ ファイルとフォルダー`PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-121">For example, our website has an `~/Images/` folder with a single image file, `PoweredByASPNET.gif`.</span></span> <span data-ttu-id="a9afa-122">マスター ページファイル`Site.master`が、`<img>`内の要素、`footerContent`次のマークアップを含む領域。</span><span class="sxs-lookup"><span data-stu-id="a9afa-122">The master page file `Site.master` has an `<img>` element in the `footerContent` region with the following markup:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

<span data-ttu-id="a9afa-123">`src`属性の値、`<img>`要素は、相対 URL で始まらないため`/`または`http://`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-123">The `src` attribute value in the `<img>` element is a relative URL because it does not start with `/` or `http://`.</span></span> <span data-ttu-id="a9afa-124">つまり、`src`属性値を検索するようブラウザーに指示、`Images`という名前のファイルに対応するサブフォルダー`PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-124">In short, the `src` attribute value tells the browser to look in the `Images` subfolder for a file named `PoweredByASPNET.gif`.</span></span>

<span data-ttu-id="a9afa-125">コンテンツ ページを訪問する際に上記のマークアップは、ブラウザーに直接送信されます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-125">When visiting a content page, the above markup is sent directly to the browser.</span></span> <span data-ttu-id="a9afa-126">すぐにアクセスを`About.aspx`し、ブラウザーに送信される HTML ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-126">Take a moment to visit `About.aspx` and view the HTML source sent to the browser.</span></span> <span data-ttu-id="a9afa-127">マスター ページ内の正確な同じマークアップをブラウザーに送信されたことがわかります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-127">You will find that the exact same markup in the master page was sent to the browser.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

<span data-ttu-id="a9afa-128">場合は、ルート フォルダーにコンテンツ ページ (は`About.aspx`) すべてが正しく動作があるので、`Images`ルート フォルダーの相対パスのサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-128">If the content page is in the root folder (as is `About.aspx`) everything works as expected because there is an `Images` subfolder relative to the root folder.</span></span> <span data-ttu-id="a9afa-129">ただし、処理は、コンテンツ ページがマスター ページとは異なるフォルダーにある場合に分解します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-129">However, things break down if the content page is in a different folder than the master page.</span></span> <span data-ttu-id="a9afa-130">これを示すためには、という名前のサブフォルダーを作成する`Admin`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-130">To illustrate this, create a subfolder named `Admin`.</span></span> <span data-ttu-id="a9afa-131">という名前のコンテンツ ページを次に、追加`Default.aspx`を`Admin`フォルダーで、新しいページにバインドすることを確認、`Site.master`マスター ページ。</span><span class="sxs-lookup"><span data-stu-id="a9afa-131">Next, add a content page named `Default.aspx` to the `Admin` folder, making sure to bind the new page to the `Site.master` master page.</span></span>

> [!NOTE]
> <span data-ttu-id="a9afa-132">[*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`コンテンツ ページのタイトルを自動的に設定する (場合、明示的に割り当てられていません)。</span><span class="sxs-lookup"><span data-stu-id="a9afa-132">In the [*Specifying the Title, Meta Tags, and Other HTML Headers in the Master Page*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial we created a custom base page class named `BasePage` that automatically set the content page's title (if it was not explicitly assigned).</span></span> <span data-ttu-id="a9afa-133">忘れずにから派生して、新しく作成されたページの分離コード クラスを持つ`BasePage`この機能を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-133">Don't forget to have the newly created page's code-behind class derive from `BasePage` so that it can utilize this functionality.</span></span>


<span data-ttu-id="a9afa-134">このコンテンツ ページを作成した後、ソリューション エクスプ ローラーは図 1 のようになります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-134">After you have created this content page, your Solution Explorer should look similar to Figure 1.</span></span>


![新しいフォルダーと ASP.NET ページがプロジェクトに追加されました](urls-in-master-pages-cs/_static/image1.png)

<span data-ttu-id="a9afa-136">**図 01**: 新しいフォルダーと ASP.NET ページがプロジェクトに追加されました</span><span class="sxs-lookup"><span data-stu-id="a9afa-136">**Figure 01**: A New Folder and ASP.NET Page Have Been Added to the Project</span></span>


<span data-ttu-id="a9afa-137">次に、更新、`Web.sitemap`ファイルに含め、新しい`<siteMapNode>`このレッスンのエントリ。</span><span class="sxs-lookup"><span data-stu-id="a9afa-137">Next, update the `Web.sitemap` file to include a new `<siteMapNode>` entry for this lesson.</span></span> <span data-ttu-id="a9afa-138">次の XML は、完全な`Web.sitemap`マークアップで、3 つ目の追加が含まれています`<siteMapNode>`要素。</span><span class="sxs-lookup"><span data-stu-id="a9afa-138">The following XML shows the complete `Web.sitemap` markup, which now includes the addition of a third `<siteMapNode>` element.</span></span>


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

<span data-ttu-id="a9afa-139">新しく作成された`Default.aspx` ページで次の 4 つ contentplaceholders に対応する 4 つのコンテンツ コントロールを持つ必要があります`Site.master`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-139">The newly created `Default.aspx` page should have four Content controls corresponding to the four ContentPlaceHolders in `Site.master`.</span></span> <span data-ttu-id="a9afa-140">参照するコンテンツ コントロールにテキストを追加、 `MainContent` ContentPlaceHolder し、ブラウザーでページにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-140">Add some text to the Content control referencing the `MainContent` ContentPlaceHolder and then visit the page through a browser.</span></span> <span data-ttu-id="a9afa-141">図 2 では、ブラウザーが見つかりません、`PoweredByASPNET.gif`イメージ ファイル。</span><span class="sxs-lookup"><span data-stu-id="a9afa-141">As Figure 2 shows, the browser cannot find the `PoweredByASPNET.gif` image file.</span></span> <span data-ttu-id="a9afa-142">ここで何が起こっているんですか。</span><span class="sxs-lookup"><span data-stu-id="a9afa-142">What's going on here?</span></span>

<span data-ttu-id="a9afa-143">`~/Admin/Default.aspx`同じ HTML コンテンツ ページを送信、`footerContent`が、領域、`About.aspx`ページ。</span><span class="sxs-lookup"><span data-stu-id="a9afa-143">The `~/Admin/Default.aspx` content page is sent the same HTML for the `footerContent` region as was the `About.aspx` page:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

<span data-ttu-id="a9afa-144">`<img>`要素の`src`属性は相対 URL をブラウザーが検索しようとした場合、 `Images` web ページのフォルダーの場所に対する相対フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-144">Because the `<img>` element's `src` attribute is a relative URL, the browser attempts to look for an `Images` folder relative to the web page's folder location.</span></span> <span data-ttu-id="a9afa-145">つまり、イメージ ファイルには、ブラウザーが検索`Admin/Images/PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-145">In other words, the browser is looking for the image file `Admin/Images/PoweredByASPNET.gif`.</span></span>


<span data-ttu-id="a9afa-146">[![PoweredByASPNET.gif イメージ ファイルが見つかりません](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="a9afa-146">[![The PoweredByASPNET.gif Image File Cannot Be Found](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)</span></span>

<span data-ttu-id="a9afa-147">**図 02**:`PoweredByASPNET.gif`イメージ ファイルが見つかりません ([フルサイズのイメージを表示するをクリックして](urls-in-master-pages-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a9afa-147">**Figure 02**: The `PoweredByASPNET.gif` Image File Cannot Be Found  ([Click to view full-size image](urls-in-master-pages-cs/_static/image4.png))</span></span>


### <a name="replacing-relative-urls-with-absolute-urls"></a><span data-ttu-id="a9afa-148">相対 Url を絶対 Url に置き換えます</span><span class="sxs-lookup"><span data-stu-id="a9afa-148">Replacing Relative URLs with Absolute URLs</span></span>

<span data-ttu-id="a9afa-149">相対 URL の逆に、*絶対 URL*、スラッシュで始まる 1 つである (`/`) またはなどのプロトコル`http://`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-149">The opposite of a relative URL is an *absolute URL*, which is one that starts with a forward slash (`/`) or a protocol such as `http://`.</span></span> <span data-ttu-id="a9afa-150">絶対 URL が既知の固定小数点からリソースの場所を指定するため、同じ絶対 URL は、web サイトのフォルダー構造内の web ページの場所に関係なく、任意の web ページで有効です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-150">Because an absolute URL specifies the location of a resource from a known fixed point, the same absolute URL is valid in any web page, regardless of the web page's location in the website's folder structure.</span></span>

<span data-ttu-id="a9afa-151">図 2 に示すように壊れた画像を解決する必要がありますを更新する、`<img>`要素の`src`属性ではなく、相対絶対 URL を使用するようにします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-151">To remedy the broken image shown in Figure 2, we need to update the `<img>` element's `src` attribute so that it uses an absolute URL instead of a relative one.</span></span> <span data-ttu-id="a9afa-152">正しい絶対 URL を確認するのには、いずれかの web サイトで web ページを参照してくださいをアドレス バーを確認します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-152">To determine the correct absolute URL, visit one of the web pages in your website and examine the Address bar.</span></span> <span data-ttu-id="a9afa-153">Web アプリケーションへの完全修飾パスは、図 2 に、アドレス バーに示す`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-153">As the Address bar in Figure 2 shows, the fully qualified path to the web application is `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`.</span></span> <span data-ttu-id="a9afa-154">更新すること、したがって、`<img>`要素の`src`属性を次の 2 つの絶対 Url のいずれか。</span><span class="sxs-lookup"><span data-stu-id="a9afa-154">Therefore, we could update the `<img>` element's `src` attribute to either of the following two absolute URLs:</span></span>

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

<span data-ttu-id="a9afa-155">更新する、`<img>`要素の`src`属性上に示した形式のいずれかを使用して絶対 URL をし、アクセス、`~/Admin/Default.aspx`ブラウザーを使用してページ。</span><span class="sxs-lookup"><span data-stu-id="a9afa-155">Take a moment to update the `<img>` element's `src` attribute to an absolute URL using one of the forms shown above and then visit the `~/Admin/Default.aspx` page through a browser.</span></span> <span data-ttu-id="a9afa-156">今度は、ブラウザーの検索し、表示が正しく、`PoweredByASPNET.gif`イメージ ファイル (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9afa-156">This time the browser will correctly find and display the `PoweredByASPNET.gif` image file (see Figure 3).</span></span>


<span data-ttu-id="a9afa-157">[![PoweredByASPNET.gif イメージが表示されるようになりました](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a9afa-157">[![The PoweredByASPNET.gif Image is Now Displayed](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)</span></span>

<span data-ttu-id="a9afa-158">**図 03**:`PoweredByASPNET.gif`イメージが表示されるようになりました ([フルサイズのイメージを表示するをクリックして](urls-in-master-pages-cs/_static/image7.png))</span><span class="sxs-lookup"><span data-stu-id="a9afa-158">**Figure 03**: The `PoweredByASPNET.gif` Image is Now Displayed  ([Click to view full-size image](urls-in-master-pages-cs/_static/image7.png))</span></span>


<span data-ttu-id="a9afa-159">ハード コーディングの絶対 URL で動作しますが、密に結合、HTML web サイトのサーバーおよびフォルダーの場所は、変更することがあります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-159">While hard coding in the absolute URL works, it tightly couples your HTML to the website's server and folder location, which may change.</span></span> <span data-ttu-id="a9afa-160">フォームの絶対 URL を使用して`http://localhost:3908/...`は当てため前のポート番号`localhost`Visual Studio の組み込みの ASP.NET 開発 Web サーバーを起動するたびに自動的に選択されます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-160">Using an absolute URL of the form `http://localhost:3908/...` is brittle because the port number preceding `localhost` is selected automatically each time Visual Studio's built-in ASP.NET Development Web Server is started.</span></span> <span data-ttu-id="a9afa-161">同様に、`http://localhost`部分は、ローカルでテストする場合にのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-161">Similarly, the `http://localhost` part is only valid when testing locally.</span></span> <span data-ttu-id="a9afa-162">URL のベースと同様に変更は、別のものに、コードを実稼働サーバーに展開すると後、`http://www.yourserver.com`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-162">Once the code is deployed to a production server, the URL base will change to something else, like `http://www.yourserver.com`.</span></span> <span data-ttu-id="a9afa-163">フォームの絶対 URL`/ASPNET_MasterPages_Tutorial_04_CS/...`開発と実稼働サーバー間でこのアプリケーションのパスが異なることがよくあるために、同じの脆弱性からも低下します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-163">The absolute URL in the form `/ASPNET_MasterPages_Tutorial_04_CS/...` also suffers from the same brittleness because oftentimes this application path differs between development and production servers.</span></span>

<span data-ttu-id="a9afa-164">良いニュースは、ASP.NET が実行時に有効な相対 URL を生成するためのメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-164">The good news is that ASP.NET offers a method for generating a valid relative URL at runtime.</span></span>

## <a name="usingandresolveclienturl"></a><span data-ttu-id="a9afa-165">使用して`~`と`ResolveClientUrl`</span><span class="sxs-lookup"><span data-stu-id="a9afa-165">Using`~`and`ResolveClientUrl`</span></span>

<span data-ttu-id="a9afa-166">はなく絶対 URL をハード コーディングするよりも ASP.NET では、チルダを使用するページの開発者 (`~`) web アプリケーションのルートを表します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-166">Rather than hard code an absolute URL, ASP.NET allows page developers to use the tilde (`~`) to indicate the root of the web application.</span></span> <span data-ttu-id="a9afa-167">たとえば、このチュートリアルで既に使用表記`~/Admin/Default.aspx`を指すテキストで、 `Default.aspx`  ページで、`Admin`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-167">For example, earlier in this tutorial I used the notation `~/Admin/Default.aspx` in the text to refer to the `Default.aspx` page in the `Admin` folder.</span></span> <span data-ttu-id="a9afa-168">`~`ことを示します、`Admin`フォルダーは、web アプリケーションのルートのサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-168">The `~` indicates that the `Admin` folder is a subfolder of the web application's root.</span></span>

<span data-ttu-id="a9afa-169">`Control`クラスの[`ResolveClientUrl`メソッド](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)URL は、コントロールが存在する web ページの適切な相対 URL を変更します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-169">The `Control` class's [`ResolveClientUrl` method](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) takes a URL and modifies it to a relative URL appropriate for the web page on which the control resides.</span></span> <span data-ttu-id="a9afa-170">たとえば、呼び出す`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`から`About.aspx`返します`Images/PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-170">For example, calling `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` from `About.aspx` returns `Images/PoweredByASPNET.gif`.</span></span> <span data-ttu-id="a9afa-171">呼び出す`~/Admin/Default.aspx`、ただしを返します`../Images/PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-171">Calling it from `~/Admin/Default.aspx`, however, returns `../Images/PoweredByASPNET.gif`.</span></span>

> [!NOTE]
> <span data-ttu-id="a9afa-172">すべての ASP.NET サーバー コントロールがから派生するため、`Control`クラス、すべてのサーバー コントロールにアクセスする、`ResolveClientUrl`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-172">Because all ASP.NET server controls derive from the `Control` class, all server controls have access to the `ResolveClientUrl` method.</span></span> <span data-ttu-id="a9afa-173">でも、`Page`クラスから派生します`Control`クラス、ASP.NET ページの分離コード クラスから直接には、このメソッドを使用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-173">Even the `Page` class derives from the `Control` class, meaning that you can use this method directly from your ASP.NET pages' code-behind classes.</span></span>


### <a name="usingin-the-declarative-markup"></a><span data-ttu-id="a9afa-174">使用して`~`宣言型マークアップ</span><span class="sxs-lookup"><span data-stu-id="a9afa-174">Using`~`in the Declarative Markup</span></span>

<span data-ttu-id="a9afa-175">複数の ASP.NET Web コントロールには、URL に関連するプロパティが含まれます: ハイパーリンク コントロールは、`NavigateUrl`プロパティ以外のコントロールにイメージ、`ImageUrl`プロパティです。 というようにします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-175">Several ASP.NET Web controls include URL-related properties: the HyperLink control has a `NavigateUrl` property; the Image control has an `ImageUrl` property; and so on.</span></span> <span data-ttu-id="a9afa-176">これらのコントロールがそれらの URL に関連するプロパティ値を渡すレンダリングされると、`ResolveClientUrl`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-176">When rendered, these controls pass their URL-related property values to `ResolveClientUrl`.</span></span> <span data-ttu-id="a9afa-177">したがって、これらのプロパティが含まれている場合、 `~` web アプリケーションのルートを示すときに、URL が有効な相対 URL に変更されます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-177">Consequently, if these properties contain a `~` to indicate the root of the web application, the URL will be modified to a valid relative URL.</span></span>

<span data-ttu-id="a9afa-178">ASP.NET サーバー コントロールのみを変換することに留意してください、`~`でその URL に関連するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-178">Keep in mind that only ASP.NET server controls transform the `~` in their URL-related properties.</span></span> <span data-ttu-id="a9afa-179">場合、`~`などの静的な HTML マークアップに表示されます`<img src="~/Images/PoweredByASPNET.gif" />`、ASP.NET エンジンに送信、 `~` HTML コンテンツの残りの部分と共にブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-179">If a `~` appears in static HTML markup, such as `<img src="~/Images/PoweredByASPNET.gif" />`, the ASP.NET engine sends the `~` to the browser along with the rest of the HTML content.</span></span> <span data-ttu-id="a9afa-180">ブラウザーが想定する、 `~` URL の一部です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-180">The browser assumes that the `~` is part of the URL.</span></span> <span data-ttu-id="a9afa-181">例では、ブラウザーが、マークアップを受け取った場合の`<img src="~/Images/PoweredByASPNET.gif" />`という名前のサブフォルダーがあると想定`~`がサブフォルダーに`Images`イメージ ファイルを格納している`PoweredByASPNET.gif`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-181">For example, if the browser receives the markup `<img src="~/Images/PoweredByASPNET.gif" />` it assumes there is a subfolder named `~` with a subfolder `Images` that contains the image file `PoweredByASPNET.gif`.</span></span>

<span data-ttu-id="a9afa-182">内のイメージのマークアップを修正する`Site.master`、既存の置換`<img>`ASP.NET イメージ Web コントロールを持つ要素。</span><span class="sxs-lookup"><span data-stu-id="a9afa-182">To fix the image markup in `Site.master`, replace the existing `<img>` element with an ASP.NET Image Web control.</span></span> <span data-ttu-id="a9afa-183">イメージの Web コントロールの設定`ID`に`PoweredByImage`、その`ImageUrl`プロパティを`~/Images/PoweredByASPNET.gif`、およびその`AlternateText`「によって強化された ASP.NET!」をプロパティ</span><span class="sxs-lookup"><span data-stu-id="a9afa-183">Set the Image Web control's `ID` to `PoweredByImage`, its `ImageUrl` property to `~/Images/PoweredByASPNET.gif`, and its `AlternateText` property to "Powered by ASP.NET!"</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

<span data-ttu-id="a9afa-184">マスター ページにこの変更を行った後、再アクセス、`~/Admin/Default.aspx`ページをもう一度です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-184">After making this change to the master page, revisit the `~/Admin/Default.aspx` page again.</span></span> <span data-ttu-id="a9afa-185">この時間、`PoweredByASPNET.gif`イメージ ファイル、ページに表示されます (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9afa-185">This time the `PoweredByASPNET.gif` image file appears in the page (see Figure 3).</span></span> <span data-ttu-id="a9afa-186">表示する際に、イメージの Web コントロールが、使用して、`ResolveClientUrl`を解決する方法、`ImageUrl`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="a9afa-186">When the Image Web control is rendered it uses the `ResolveClientUrl` method to resolve its `ImageUrl` property value.</span></span> <span data-ttu-id="a9afa-187">`~/Admin/Default.aspx` 、 `ImageUrl` HTML ソースの表示の次のスニペットに、適切な相対 URL に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-187">In `~/Admin/Default.aspx` the `ImageUrl` is converted into an appropriate relative URL, as the following snippet of HTML source shows:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> <span data-ttu-id="a9afa-188">URL ベースの Web コントロールのプロパティで使用されているだけでなく、`~`を呼び出すときにも使用できます、`Response.Redirect`と`Server.MapPath`の他のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-188">In addition to being used in URL-based Web control properties, the `~` can also be used when calling the `Response.Redirect` and `Server.MapPath` methods, among others.</span></span> <span data-ttu-id="a9afa-189">また、`ResolveClientUrl`メソッドが呼び出された場合、ASP.NET またはマスター ページの宣言型のマークアップから直接必要な場合は、参照してください[Fritz タマネギ](https://www.pluralsight.com/blogs/fritz/)のブログ エントリ[Using`ResolveClientUrl`マークアップで](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-189">Also, the `ResolveClientUrl` method may be invoked directly from an ASP.NET or master page's declarative markup, if needed; see [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)'s blog entry [Using `ResolveClientUrl` in Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).</span></span>


## <a name="fixing-the-master-pages-remaining-relative-urls"></a><span data-ttu-id="a9afa-190">マスター ページの相対 Url の残りの修正</span><span class="sxs-lookup"><span data-stu-id="a9afa-190">Fixing the Master Page's Remaining Relative URLs</span></span>

<span data-ttu-id="a9afa-191">加え、`<img>`内の要素、`footerContent`を修正しました、マスター ページに注目を必要とする 1 つのより相対 URL が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a9afa-191">In addition to the `<img>` element in the `footerContent` that we just fixed, the master page contains one more relative URL that requires our attention.</span></span> <span data-ttu-id="a9afa-192">`topContent`領域には、「マスター ページ チュートリアル、」をポイントするリンクが含まれています。`Default.aspx`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-192">The `topContent` region includes the link "Master Pages Tutorials," which points to `Default.aspx`.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

<span data-ttu-id="a9afa-193">ユーザーには送信この URL は相対であるため、`Default.aspx`フォルダーへのアクセスは、コンテンツ ページのページです。</span><span class="sxs-lookup"><span data-stu-id="a9afa-193">Because this URL is relative, it will send the user to the `Default.aspx` page in the folder of the content page they are visiting.</span></span> <span data-ttu-id="a9afa-194">このリンクを常にポイントして`Default.aspx`に置き換える必要があります、ルート フォルダーで、`<a>`ハイパーリンク Web を持つ要素を制御できるように、`~`表記します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-194">To have this link always point to `Default.aspx` in the root folder we need to replace the `<a>` element with a HyperLink Web control so that we can use the `~` notation.</span></span>

<span data-ttu-id="a9afa-195">削除、`<a>`要素マークアップし、代わりに、ハイパーリンク コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-195">Remove the `<a>` element markup and add a HyperLink control in its place.</span></span> <span data-ttu-id="a9afa-196">設定のハイパーリンクの`ID`に`lnkHome`、その`NavigateUrl`プロパティを`~/Default.aspx`、およびその`Text`「マスター ページ チュートリアル」プロパティ</span><span class="sxs-lookup"><span data-stu-id="a9afa-196">Set the HyperLink's `ID` to `lnkHome`, its `NavigateUrl` property to `~/Default.aspx`, and its `Text` property to "Master Pages Tutorials."</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

<span data-ttu-id="a9afa-197">これで完了です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-197">That's it!</span></span> <span data-ttu-id="a9afa-198">この時点で、マスター ページとコンテンツ ページにどのようなフォルダーに関係なく、コンテンツ ページによってレンダリングされるときに、マスター ページ内の Url が基づく正しくすべてにあります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-198">At this point all the URLs in our master page are properly based when rendered by a content page regardless of what folders the master page and content page are located in.</span></span>

### <a name="automatic-url-resolution-in-theheadsection"></a><span data-ttu-id="a9afa-199">自動の URL の解決方法、`<head>`セクション</span><span class="sxs-lookup"><span data-stu-id="a9afa-199">Automatic URL Resolution in the`<head>`Section</span></span>

<span data-ttu-id="a9afa-200">[ *、サイト全体のレイアウトを使用してマスター ページを作成する*](creating-a-site-wide-layout-using-master-pages-cs.md)チュートリアルが追加されました、`<link>`を`Styles.css`ファイルで、`<head>`領域。</span><span class="sxs-lookup"><span data-stu-id="a9afa-200">In the [*Creating a Site-Wide Layout Using Master Pages*](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial we added a `<link>` to the `Styles.css` file in the `<head>` region:</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

<span data-ttu-id="a9afa-201">中に、`<link>`要素の`href`属性は相対が実行時に適切なパスを自動的に変換します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-201">While the `<link>` element's `href` attribute is relative, it's automatically converted to an appropriate path at runtime.</span></span> <span data-ttu-id="a9afa-202">説明したよう、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)チュートリアルでは、`<head>`地域が実際には、サーバー側のコントロール、変更できるようにする、その内部のコントロールが表示される場合の内容。</span><span class="sxs-lookup"><span data-stu-id="a9afa-202">As we discussed in the [*Specifying the Title, Meta Tags, and Other HTML Headers in the Master Page*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, the `<head>` region is actually a server-side control, which enables it to modify the contents of its inner controls when it is rendered.</span></span>

<span data-ttu-id="a9afa-203">これを確認する再アクセス、`~/Admin/Default.aspx`ページし、ブラウザーに送信される HTML ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-203">To verify this, revisit the `~/Admin/Default.aspx` page and view the HTML source sent to the browser.</span></span> <span data-ttu-id="a9afa-204">次のスニペットに示すように、`<link>`要素の`href`属性は、適切な相対 URL を自動的に変更された`../Styles.css`です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-204">As the snippet below illustrates, the `<link>` element's `href` attribute has been automatically modified to an appropriate relative URL, `../Styles.css`.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a><span data-ttu-id="a9afa-205">まとめ</span><span class="sxs-lookup"><span data-stu-id="a9afa-205">Summary</span></span>

<span data-ttu-id="a9afa-206">非常に多くの場合、マスター ページには、リンク、イメージ、およびその他の外部リソースの URL を使用して指定する必要がありますが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a9afa-206">Master pages very often include links, images, and other external resources that must be specified via a URL.</span></span> <span data-ttu-id="a9afa-207">同じフォルダーに、マスター ページとページのコンテンツが存在しない可能性がありますので、abstain から相対 Url を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-207">Because the master page and content pages might not exist in the same folder, it is important to abstain from using relative URLs.</span></span> <span data-ttu-id="a9afa-208">ハード コーディングされた絶対 Url を使用することはできますは、web アプリケーションに絶対 URL を結合ため緊密を実行します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-208">While it is possible to use hard coded absolute URLs, doing so tightly couples the absolute URL to the web application.</span></span> <span data-ttu-id="a9afa-209">ように移動するか、web アプリケーションを展開するときに多くの場合は、絶対 URL が変更された場合は、戻ってして絶対 Url を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9afa-209">If the absolute URL changes - as it often does when moving or deploying a web application - you'll have to remember to go back and update the absolute URLs.</span></span>

<span data-ttu-id="a9afa-210">チルダを使用する最適な方法 (`~`) アプリケーション ルートを表します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-210">The ideal approach is to use the tilde (`~`) to indicate the application root.</span></span> <span data-ttu-id="a9afa-211">URL に関連するプロパティを格納している ASP.NET Web コントロールのマップ、`~`実行時にアプリケーションのルートにします。</span><span class="sxs-lookup"><span data-stu-id="a9afa-211">ASP.NET Web controls that contain URL-related properties map the `~` to the application root at runtime.</span></span> <span data-ttu-id="a9afa-212">内部的には、Web コントロールを使用して、`Control`クラスの`ResolveClientUrl`有効な相対 URL を生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-212">Internally, the Web controls use the `Control` class's `ResolveClientUrl` method to generate a valid relative URL.</span></span> <span data-ttu-id="a9afa-213">このメソッドはパブリックであり、すべてのサーバー コントロールから使用可能な (など、`Page`クラス) を使用できるようにプログラムによって、分離コード クラスから必要な場合は、します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-213">This method is public and available from every server control (including the `Page` class), so you can use it programmatically from your code-behind classes, if needed.</span></span>

<span data-ttu-id="a9afa-214">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="a9afa-214">Happy Programming!</span></span>

### <a name="further-reading"></a><span data-ttu-id="a9afa-215">関連項目</span><span class="sxs-lookup"><span data-stu-id="a9afa-215">Further Reading</span></span>

<span data-ttu-id="a9afa-216">このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a9afa-216">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="a9afa-217">ASP.NET マスター ページ</span><span class="sxs-lookup"><span data-stu-id="a9afa-217">Master Pages in ASP.NET</span></span>](http://www.odetocode.com/Articles/419.aspx)
- [<span data-ttu-id="a9afa-218">マスター ページの URL が再配置</span><span class="sxs-lookup"><span data-stu-id="a9afa-218">URL Rebasing in a Master Page</span></span>](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [<span data-ttu-id="a9afa-219">使用して`ResolveClientUrl`マークアップ</span><span class="sxs-lookup"><span data-stu-id="a9afa-219">Using `ResolveClientUrl` in Markup</span></span>](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a><span data-ttu-id="a9afa-220">作成者について</span><span class="sxs-lookup"><span data-stu-id="a9afa-220">About the Author</span></span>

<span data-ttu-id="a9afa-221">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-221">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of multiple ASP/ASP.NET books and founder of 4GuysFromRolla.com, has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="a9afa-222">Scott は、コンサルタント、トレーナー、ライターとして機能します。</span><span class="sxs-lookup"><span data-stu-id="a9afa-222">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="a9afa-223">最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-223">His latest book is [*Sams Teach Yourself ASP.NET 3.5 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="a9afa-224">Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-224">Scott can be reached at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) or via his blog at [http://ScottOnWriting.NET](http://scottonwriting.net/).</span></span>

### <a name="special-thanks-to"></a><span data-ttu-id="a9afa-225">感謝の特別な</span><span class="sxs-lookup"><span data-stu-id="a9afa-225">Special Thanks To</span></span>

<span data-ttu-id="a9afa-226">今後、MSDN の記事を確認することに関心のあるですか。</span><span class="sxs-lookup"><span data-stu-id="a9afa-226">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="a9afa-227">場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。</span><span class="sxs-lookup"><span data-stu-id="a9afa-227">If so, drop me a line at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a9afa-228">[前へ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
[次へ](control-id-naming-in-content-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a9afa-228">[Previous](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
[Next](control-id-naming-in-content-pages-cs.md)</span></span>