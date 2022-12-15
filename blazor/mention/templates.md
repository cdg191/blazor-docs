---
layout: post
title: Templates in Blazor Mention Component | Syncfusion
description: Checkout and learn here all about Templates in Syncfusion Blazor Mention component and much more.
platform: Blazor
control: Mention
documentation: ug
---

# Templates in Mention

The Mention has been provided with several options to customize each suggestion list item, display item, and data loading indication.

## Item template

The content of each list item in the Mention can be customized using the `ItemTemplate` property.

In the following sample, each list item is split into two columns to display relevant data using `ItemTemplate`.

{% highlight razor %}

{% include_relative code-snippet/item-template.razor %}

{% endhighlight %}

![Blazor Mention with item template](./images/blazor-mention-item-template.png)

## Display template

You can customize the mentioned value’s display appearance using the `DisplayTemplate` property.

In the following sample, the selected value is displayed as a combined text of both FirstName and City in the mention element, which is separated by a hyphen.

{% highlight razor %}

{% include_relative code-snippet/display-template.razor %}

{% endhighlight %}

![Blazor Mention with display template](./images/blazor-mention-display-template.png)

## No records template

You can show the custom design of the popup list content when no data and matches are found on the search with the help of `NoRecordsTemplate`
property.

{% highlight razor %}

{% include_relative code-snippet/no-record-template.razor %}

{% endhighlight %}

![Blazor Mention with no record template](./images/blazor-mention-noRecord-template.png)

## Spinner template

Display the customized waiting spinner, when data fetching takes time to load in the suggestion list by using the `SpinnerTemplate` property.

{% highlight razor %}

{% include_relative code-snippet/spinner-template.razor %}

{% endhighlight %}

![Blazor Mention with spinner template](./images/blazor-mention-spinner-template.png)

## See also

* [How to achieve filtering](./filtering)