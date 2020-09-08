# Blazor Kontent Issue

![.NET Core](https://github.com/jonathantwite/BlazorBoats/workflows/.NET%20Core/badge.svg)
[![Netlify Status](https://api.netlify.com/api/v1/badges/4158c420-c8f6-4219-a0f8-4989002669a4/deploy-status)](https://app.netlify.com/sites/blazorboats/deploys)

Minimally-viable example

## Visual Studio 16.6.4

1. New Project > Blazor App
1. Blazor WebAssembly, Configured for HTTPS, no other options.
1. NuGet > Install *Kentico.Kontent.Delivery*
1. In Project folder, run
    
    ```powershell
    KontentModelGenerator -p "168e7186-c7d9-00db-38d9-641daf004c44" -n "BlazorBoats.Models" -o "Models\."
    ```

1. In `Program.cs` add

    ```csharp
    builder.Services.AddDeliveryClient(new DeliveryOptions()
    {
        ProjectId = "168e7186-c7d9-00db-38d9-641daf004c44"
    });
    ```

1. In `Pages\Index.razor` add

    ```csharp
    @using Models
    @using Kentico.Kontent.Delivery.Abstractions
    @inject IDeliveryClient deliveryClient

    @if (dinghyClasses != null)
    {
    <ol>
        @foreach (var dinghyClass in dinghyClasses)
        {
            <li>@dinghyClass.ClassName</li>
        }
    </ol>
    }
    
    @code{
        private IEnumerable<DinghyClass> dinghyClasses;
        protected override async Task OnInitializedAsync()
        {
            dinghyClasses = (await deliveryClient.GetItemsAsync<DinghyClass>()).Items;
        }
    }
    ```

Running this should show a list of 5 items with names.  However, 16 items are shown, 11 of which have no text.

When looking at the Network tab on the chrome debugger, all items are being returned, not just the `DinghyClass` items.
