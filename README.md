# Online Stores Theme

A custom theme for Online Stores.

## Branching Structure

Online Stores owns multiple websites, this repository houses each of those individual *themes* under this branching structure:

* `master` -> Base theme
* `theme/construction-gear` -> Construction Gear specific theme
* `theme/discount-safety-gear` -> Discount Safety Gear specific theme
* `theme/safety-girl` -> Safety Girl specific theme
* `theme/tea-store` -> Tea Store specific theme
* `theme/northline` -> Northline Express specific theme
* `theme/online-stores` -> Online Stores specific theme
* `theme/us-flag` -> United States Flag specific theme
* `theme/light-up` -> Light Up specific theme

Changes made to the core theme should be added to the `master` branch and merged into each individual theme. Anything that is specific to a stores theme, should be done in that theme's branch.

This branching structure might cause collisions when you merge downstream from master into the theme branches if you have made significant changes to those files. In that case, you will have to cherry pick those changes and merge them by hand.

Each theme branch contains a `production` and `sandbox` configuration. You can switch between then using `npm run sandbox` or `npm run production`. This will change stencil to read from that stores product listings, categories. . .

## Prebuilding Step

When you go to production, you'll need to follow this step to make sure you do not overwrite the custom configuration done through the theme's `customize` option in BigCommerce.

1. Log into the store's `/manage` portal.
2. Click on `Storefront > My Themes`.
3. In the `Current Theme` section, click the `Advanced` dropdown.
4. Click `Download Current Theme`.
5. Once the zip of the current theme is downloaded, extract it to a folder.
6. Compare the `config.json` file in the extracted folder to the `config.json` found in this repository.
7. Merge over any changes made

## Building Step

In order to build a theme for a website:

1. Switch to the `/theme/{website}` branch.
2. Merge `master` into your current branch to sync changes to the base theme.
3. Run `npm run build` to increment the version number and bundle the theme.

This will produce a `{Theme Name}-x.x.zip` file in the root of this repository. You can either:

1. Run `stencil push` to push that bundle into the **current environment** store theme list
   1. If you have too many themes, the cli will prompt you to delete a few
   2. The cli will prompt you if you want to apply the theme now
2. Upload the theme by hand to the store by:
   1. Log into the store's `/manage` portal
   2. Click on `Storefront > My Themes`
   3. Click on `Upload Theme`
      1. If this button is greyed out, you need to delete an older theme to add a new one.
   4. Once the theme is uploaded, click on the theme and hit `apply` (this option is also available in the hamburger menu on the theme's list page)

## Commands

* `npm run start` -> Runs stencil on your local machine.
* `npm run build` -> Increments your current version number and builds the bundle.
* `npm run production` -> Switches your environment to the production site.
* `npm run sandbox` -> Switches your environment to the sandbox site.
* `npm run dev` -> Alias for `npm run sandbox`.
* `npm run prod` -> Alias for `npm run production`.

## BigCommerce Cornerstone Information

### Stencil Utils
[Stencil-utils](https://github.com/bigcommerce/stencil-utils) is our supporting library for our events and remote interactions.

## JS API
When writing theme JavaScript (JS) there is an API in place for running JS on a per page basis. To properly write JS for your theme, the following page types are available to you:

* "pages/account/addresses"
* "pages/account/add-address"
* "pages/account/add-return"
* "pages/account/add-wishlist"
* "pages/account/recent-items"
* "pages/account/download-item"
* "pages/account/edit"
* "pages/account/return-saved"
* "pages/account/returns"
* "pages/account/payment-methods"
* "pages/auth/login"
* "pages/auth/account-created"
* "pages/auth/create-account"
* "pages/auth/new-password"
* "pages/blog"
* "pages/blog-post"
* "pages/brand"
* "pages/brands"
* "pages/cart"
* "pages/category"
* "pages/compare"
* "pages/errors"
* "pages/gift-certificate/purchase"
* "pages/gift-certificate/balance"
* "pages/gift-certificate/redeem"
* "global"
* "pages/home"
* "pages/order-complete"
* "pages/page"
* "pages/product"
* "pages/search"
* "pages/sitemap"
* "pages/subscribed"
* "page/account/wishlist-details"
* "pages/account/wishlists"

These page types will correspond to the pages within your theme. Each one of these page types map to an ES6 module that extends the base `PageManager` abstract class.

```javascript
    export default class Auth extends PageManager {
        constructor() {
            // Set up code goes here; attach to internals and use internals as you would 'this'
        }
    }
```

### JS Template Context Injection
Occasionally you may need to use dynamic data from the template context within your client-side theme application code.

Two helpers are provided to help achieve this.

The inject helper allows you to compose a JSON object with a subset of the template context to be sent to the browser.

```
{{inject "stringBasedKey" contextValue}}
```

To retrieve the parsable JSON object, just call `{{jsContext}}` after all of the `{{@inject}}` calls.

For example, to setup the product name in your client-side app, you can do the following if you're in the context of a product:

```html
{{inject "myProductName" product.title}}

<script>
// Note the lack of quotes around the jsContext handlebars helper, it becomes a string automatically.
var jsContext = JSON.parse({{jsContext}}); // jsContext would output "{\"myProductName\": \"Sample Product\"}" which can feed directly into your JavaScript

console.log(jsContext.myProductName); // Will output: Sample Product
</script>
```

You can compose your JSON object across multiple pages to create a different set of client-side data depending on the currently loaded template context.

The stencil theme makes the jsContext available on both the active page scoped and global PageManager objects as `this.context`.


## Static assets
Some static assets in the Stencil theme are handled with Grunt if required. This
means you have some dependencies on grunt and npm. To get started:

First make sure you have Grunt installed globally on your machine:

```
npm install -g grunt-cli
```

and run:

```
npm install
```

Note: package-lock.json file was generated by Node version 8 and npm version 6.4.1. Although the app supports Node versions 6 and 8 as well as multiple versions of npm, we should always use those versions when updating package-lock.json, unless it is decided to upgrade those, and in this case the readme should be updated as well. If using a different version for node OR npm, please delete the package-lock.json file prior to installing node packages and also prior to pushing to github.

If updating or adding a dependency, please double check that you are working on Node version 8 and npm version 6.4.1 and run ```npm update <package_name>```  or ```npm install <package_name>``` (avoid running npm install for updating a package). After updating the package, please make sure that the changes in the package-lock.json reflect only the updated/new package prior to pushing the changes to github.


#### License

(The MIT License)
Copyright (C) 2015-present BigCommerce Inc.
All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense,and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
