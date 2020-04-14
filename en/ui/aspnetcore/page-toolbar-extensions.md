# Page Toolbar Extensions for ASP.NET Core UI

## Introduction

Page toolbar system allows you to add components to the toolbar of any page. The page toolbar is the area right to the header of a page. A button ("Import users from excel") was added to the user management page below:

![page-toolbar-button](../../images/page-toolbar-button.png)

You can add any type of view component item to the page toolbar or modify existing items.

## How to Set Up

In this example, we will add an "Import users from excel" button and execute a JavaScript code for the user management page of the [Identity Module](../../modules/identity.md).

### Add a New Button to the User Management Page

Write the following code inside the `ConfigureServices` of your web module class:

````csharp
Configure<AbpPageToolbarOptions>(options =>
{
    options.Configure<Volo.Abp.Identity.Web.Pages.Identity.Users.IndexModel>(toolbar =>
    {
        toolbar.AddButton(
            LocalizableString.Create<MyProjectNameResource>("ImportFromExcel"),
            icon: "file-import",
            id: "ImportUsersFromExcel",
            type: AbpButtonType.Secondary
        );
    });
});
````

`AddButton` is a shortcut to simply add a button component. Note that you need to add the `ImportFromExcel` to your localization dictionary (json file) to localize the text.

When you run the application, you will see the button added next to the current button list. There are some other parameters of the `AddButton` method (for example, use `order` to set the order of the button component relative to the other components).

### Create a JavaScript File

Now, we can go to the client side to handle click event of the new button.

First, add a new JavaScript file to your solution. We added inside the `/Pages/Identity/Users` folder of the `.Web` project:

![user-action-extension-on-solution](../../images/user-action-extension-on-solution.png)

Here, the content of this JavaScript file:

````js
$(function () {

    $('#ImportUsersFromExcel').click(function (e) {
        e.preventDefault();
        alert('TODO: import users from excel');
    });

});
````

In the `click` event, you can do anything you need.

### Add the File to the User Management Page

Then you need to add this JavaScript file to the user management page. You can take the power of the [Bundling & Minification system](https://docs.abp.io/en/abp/latest/UI/AspNetCore/Bundling-Minification).

Write the following code inside the `ConfigureServices` of your module class:

````csharp
Configure<AbpBundlingOptions>(options =>
{
    options.ScriptBundles.Configure(
        typeof(Volo.Abp.Identity.Web.Pages.Identity.Users.IndexModel).FullName,
        bundleConfiguration =>
        {
            bundleConfiguration.AddFiles(
                "/Pages/Identity/Users/my-user-extensions.js"
            );
        });
});
````

This configuration adds `my-user-extensions.js` to the user management page of the Identity Module. `typeof(Volo.Abp.Identity.Web.Pages.Identity.Users.IndexModel).FullName` is the name of the bundle in the user management page. This is a common convention used for all the ABP Commercial modules.

## Advanced Use Cases

TODO