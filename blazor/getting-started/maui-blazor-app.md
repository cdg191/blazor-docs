---
layout: post
title: Getting Started with MAUI Blazor App in Visual Studio | Syncfusion
description: Check out the documentation for getting started with MAUI Blazor App and Syncfusion Blazor Components in Visual Studio and much more.
platform: Blazor
component: Common
documentation: ug
---

# Getting Started with .NET MAUI Blazor Application

This section explains how to create and run the first .NET Multi-platform Blazor App UI (.NET MAUI Blazor) app with Syncfusion Blazor components.

## What is .NET MAUI Blazor App?

.NET MAUI Blazor App is a .NET MAUI App where `Blazor web app` is hosted in .NET MAUI app using `BlazorWebView` control. This enable a Blazor web app to be integrated with platform features and UI controls. Also, BlazorWebView can be added to any page of .NET MAUI app, and pointed to the root of the Blazor app. The Blazor components run natively in the .NET process and render web UI to an embedded web view control. .NET MAUI Blazor apps can run on all the platforms supported by .NET MAUI. 

Visual Studio provides **.NET MAUI Blazor app** template to create .NET MAUI Blazor Apps.

## Prerequisites

* .NET SDK 6.0 (Latest .NET SDK 6.0.101 or above)

* The latest preview of Visual Studio 2022 17.1 or above, with required workloads:
   * [Mobile development with .NET](https://docs.microsoft.com/en-us/dotnet/maui/get-started/installation)
   * ASP.NET and web development 

## Create a new .NET MAUI Blazor App in Visual Studio

1.  Launch Visual Studio 2022 (Preview), and in the start window **click Create a new project** to create a new project:
![Create a new project in VS2022](images/maui/create-new-project.png)
2. For .NET MAUI Blazor app, choose the **.NET MAUI Blazor app** template. Select Next. 
![Create .NET MAUI Blazor App](images/maui/create-maui-blazor-app.png)
3. In the **Configure your new project** window, name your project, choose a location for the project and click the `Create` button.
![Configure MAUI Blazor App](images/maui/configure-maui-blazor-app.png)
4. Wait for the project to be created, and its dependencies to be restored, then the project structure looks like below.
![MAUI Blazor Project Structure](images/maui/maui-blazor-project-structure.png)

## BlazorWebView in .NET MAUI Blazor App

The above steps creates a multi-targeted .NET MAUI Blazor app that can be deployed to Android, iOS, macOS, and Windows. 

In `MainPage.xaml`, The `BlazorWebView` is added and points to the root of the Blazor app. The root Blazor component for the app is in `Main.razor`, which Razor compiles into a type named `Main` in the application's root namespace. 

{% tabs %}
{% highlight xaml tabtitle="MainPage.xaml" %}

<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:b="clr-namespace:Microsoft.AspNetCore.Components.WebView.Maui;assembly=Microsoft.AspNetCore.Components.WebView.Maui"
             xmlns:local="clr-namespace:MauiApp1"
             x:Class="MauiApp1.MainPage"
             BackgroundColor="{DynamicResource PageBackgroundColor}">

    <b:BlazorWebView HostPage="wwwroot/index.html">
        <b:BlazorWebView.RootComponents>
            <b:RootComponent Selector="app" ComponentType="{x:Type local:Main}" />
        </b:BlazorWebView.RootComponents>
    </b:BlazorWebView>

</ContentPage>

{% endhighlight %}
{% endtabs %}

For more details refer [Create a .NET MAUI Blazor app](https://docs.microsoft.com/en-us/dotnet/maui/user-interface/controls/blazorwebview#create-a-net-maui-blazor-app) topic. If you already have .NET MAUI app and want to convert use `BlazorWebView`, refer [Add a BlazorWebView to an existing app](https://docs.microsoft.com/en-us/dotnet/maui/user-interface/controls/blazorwebview#add-a-blazorwebview-to-an-existing-app) topic.

## Install Syncfusion Blazor Packages in the App

Syncfusion Blazor components are available in [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor). In order to use Syncfusion Blazor components in the application, add reference to the corresponding NuGet. Refer to [NuGet packages topic](https://blazor.syncfusion.com/documentation/nuget-packages) for available NuGet packages list with component details and [Benefits of using individual NuGet packages](https://blazor.syncfusion.com/documentation/nuget-packages#benefits-of-using-individual-nuget-packages). 

To add Blazor Calendar component in the app, open the NuGet package manager in Visual Studio (*Tools → NuGet Package Manager → Manage NuGet Packages for Solution*), search for [Syncfusion.Blazor.Calendars](https://www.nuget.org/packages/Syncfusion.Blazor.Calendars/) and then install it.

## Register Syncfusion Blazor Service

Open `~/Imports.razor` file and add Syncfusion.Blazor namespace.

{% tabs %}
{% highlight razor tabtitle="~/_Imports.razor" %}

@using Syncfusion.Blazor

{% endhighlight %}
{% endtabs %}

Now, register the Syncfusion Blazor service in the MAUI Blazor App. Here, Syncfusion Blazor Service is registered by setting the [IgnoreScriptIsolation](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.GlobalOptions.html#Syncfusion_Blazor_GlobalOptions_IgnoreScriptIsolation) property as `true` using `AddSyncfusionBlazor` service method in `~/MauiProgram.cs` file as follows,

N> From 2022 Vol1 (20.1) version - The default value of `IgnoreScriptIsolation` is changed as `true`, so, you don't have to set `IgnoreScriptIsolation` property explicitly to refer scripts externally.

{% tabs %}
{% highlight c# tabtitle="~/MauiProgram.cs" hl_lines="8 9" %}

    using Syncfusion.Blazor;

    public static class MauiProgram
    {
        public static Maui CreateMauiApp()
        {
            ...
            // Set IgnoreScriptIsolation as true to load scripts externally.
            builder.Services.AddSyncfusionBlazor(options => { options.IgnoreScriptIsolation = true; });
        }
    }

{% endhighlight %}
{% endtabs %}

## Add style sheet

Checkout the [Blazor Themes topic](https://blazor.syncfusion.com/documentation/appearance/themes) to learn different ways to refer themes in the application, and to have the expected appearance for Syncfusion Blazor components. Here, the theme is referred using [Static Web Assets](https://blazor.syncfusion.com/documentation/appearance/themes#static-web-assets).

To add theme to the app, open the NuGet package manager in Visual Studio (*Tools → NuGet Package Manager → Manage NuGet Packages for Solution*), search for [Syncfusion.Blazor.Themes](https://www.nuget.org/packages/Syncfusion.Blazor.Themes/) and then install it. Then, theme stylesheet from NuGet can be referred inside the `<head>` of **wwwroot/index.html** file in the application as follows,

{% tabs %}
{% highlight cshtml tabtitle="~/index.html" %}

   <head>
       ....
       <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   </head>

{% endhighlight %}
{% endtabs %}

## Add script reference

Checkout [Adding Script Reference topic](https://blazor.syncfusion.com/documentation/common/adding-script-references) to learn different ways to add script reference in Blazor Application. In this getting started walk-through, the required scripts are referred using [Static Web Assets](https://sfblazor.azurewebsites.net/staging/documentation/common/adding-script-references#static-web-assets) externally inside the `<head>` of **wwwroot/index.html** file.

{% tabs %}
{% highlight cshtml tabtitle="~/index.html" hl_lines="4" %}

   <head>
       ....
       <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
       <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
   </head>

{% endhighlight %}
{% endtabs %}

N> Syncfusion recommends to reference scripts using [Static Web Assets](https://blazor.syncfusion.com/documentation/common/adding-script-references#static-web-assets), [CDN](https://blazor.syncfusion.com/documentation/common/adding-script-references#cdn-reference) and [CRG](https://blazor.syncfusion.com/documentation/common/custom-resource-generator) by [disabling JavaScript isolation](https://blazor.syncfusion.com/documentation/common/adding-script-references#disable-javascript-isolation) for better loading performance of the application. 

## Add Syncfusion Blazor component

Now add Syncfusion Blazor component in any razor file. Here, the Calendar component is added in `~/Pages/index.razor` page under the `~/Pages` folder.

{% tabs %}
{% highlight razor %}

@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime"></SfCalendar>

{% endhighlight %}
{% endtabs %}

In the Visual Studio toolbar, select the **Windows Machine** button to build and run the app.
Before running the sample, make sure the mode is `Windows Machine`.

![Build and run MAUI Blazor App](images/maui/windows-machine-mode.png)

N> If you want to run the application in Android or iOS refer [MAUI Getting Started](https://docs.microsoft.com/en-us/dotnet/maui/get-started/first-app) for the setup. 

![MAUI Blazor App with Syncfusion Blazor Components](images/maui/maui-blazor-calendar.png)

N> Download demo from [GitHub](https://github.com/SyncfusionExamples/MAUI-Blazor-App-using-Syncfusion-Blazor-Components)

## How to use images in .NET MAUI Blazor Application

Add the images in the images in the `wwwroot` folder of the application and refer it using `img` tag in Blazor App. 

In the below code images are added under `images` folder in `wwwroot` folder.

{% tabs %}
{% highlight razor %}

<img src="images/welcome_picture.png" height="60" width="60" />

{% endhighlight %}
{% endtabs %}

![MAUI Blazor App with added image](images\maui\maui-image.jpeg)

## Troubleshooting

### How to solve deployment errors in Windows?

If you get error dialog like "There were deployment errors", Enable developer mode. For more details refer [Enable your device for development](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development).

![Enable developer mode in system settings](images/maui/enable-developer-mode.png)

<hr/> 

### How to solve deployment errors in iOS?

In iOS code is statically compiled ahead of time, so, configure Syncfusion Blazor assemblies in `MtouchExtraArgs` tag for the iOS Release configuration in the project when deploy on a real device. 

Below are possible errors if `MtouchExtraArgs` tag is not configured,
1. App won't load on real device with error "An unhandled error has occurred" after you compile in Release mode with Visual Studio and deploy to real device.
2. AOT related failures like [`Attempting to JIT compile method while running in aot-only mode`](https://github.com/xamarin/xamarin-macios/issues/12416)

 ```
<PropertyGroup Condition="$(TargetFramework.Contains('-ios')) And $(Configuration.Contains('Release')) ">
  <UseInterpreter>true</UseInterpreter>
  <MtouchExtraArgs>--linkskip=Syncfusion.Blazor.Themes --linkskip=Syncfusion.Blazor.Inputs</MtouchExtraArgs>
</PropertyGroup>
 ```

Reference:
* [Could not AOT the assembly of my App](https://learn.microsoft.com/en-us/answers/questions/396055/could-not-aot-the-assembly-of-my-app)

<hr/>

### How to solve "The project doesn't know how to run the profile Windows Machine" while running MAUI Blazor App?

* This issue has been fixed in most recent release of Visual Studio. For more details refer [here](https://developercommunity.visualstudio.com/t/the-project-doesnt-know-how-to-run-the-profile-win/1530395).

* You can also fix this error by installing [Single-project MSIX Packaging Tools](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftSingleProjectMSIXPackagingToolsDev17).

## See also

### MAUI Blazor Diagram

* [How to create Diagram Builder in MAUI platform?](https://www.syncfusion.com/kb/13059/how-to-create-diagram-builder-in-maui-platform)

N> [View MAUI Blazor Diagram Builder Source Code in GitHub](https://github.com/syncfusion/blazor-showcase-diagram-builder/tree/master/MAUI)
