# TabulaRasa Theme

A free, minimal boilerplate theme for OctoberCMS using Tailwindcss.

I created this theme based on my personal needs, I hope somebody else will find it useful, too :-)
There are other TailwindCss themes on October, however they failed me in several respects:

- some require adding packages.json and node_modules in the website root (this breaks beautiful modularity of OctoberCMS)
- some add a lot of bloatware: additional JS and CSS files, extra dependencies etc. I needed a clean TailwindCSS (+ optional, easily removable VueJS for more complex projects)
- sometimes the October AJAX framework may be useful - therefore in `partials/site/scripts.htm` jquery is loaded, and additional framework scripts are included. Comment out the whole file if you dont intend to use it.

## Features

- Minimal setup, no bloated extras. Just one layout and one basic page
- Easy to customise colors - all colors are defined in `tailwind.config.js`
- Just one additional css file to restore minimal formatting - see below for details
- Uses laravel-mix
- Hot-reload for easy theme and site development
- Commented-out lines allow quickly adding vue to your app

### Theme Options

This theme has no options - developers who need them can quickly add them as explained in OctoberCMS docs: https://octobercms.com/docs/themes/development#customization

### Theme Colors

The theme uses a limited color selection. In the file `tailwind.config.js` you can find four sets of colours: primary, secondary, tertiary and grey, each of them with five variations: lightest, light, default, dark, and darkest.

There is an advantage to this setup - you can quickly change colors in the whole app, just by changing the primary & secondary colors.

If you prefer to use standard TailwindCSS colours, remove the whole 'colors' section from the above file.

### Theme Setup

You must first install the theme dependencies. In the theme folder, execute:

```
npm install
```

### Theme Customisation and Development

In webpack.mix.js replace site name in the following line:

```
proxy: 'http://127.0.0.1:8000/',
```

If you use built-in php/laravel server (php artisan serve), the default value should work for you.

Replace in webpack.mix.js your theme name if modified to load local fonts correctly

```
mix
  .setPublicPath('./')
  .setResourceRoot('/themes/{YOUR_THEME_FOLDER_NAME}')
```

When you run the command below:

```
npm run watch
```

you can open your site with hot-reload at http://localhost:3000

### Optional CSS: tailwindcss-typography

A plugin that provides a set of prose classes you can use to add beautiful typographic defaults to any vanilla HTML you don't control (like HTML rendered from Markdown, or pulled from a CMS).

I found it useful to restore basic formatting options after normalize.css removes all formatting.
Now you can use the prose classes to add sensible typography styles to any vanilla HTML:

```
<article class="prose lg:prose-xl">
  <h1>Garlic bread with cheese: What the science tells us</h1>
  <p>
    For years parents have espoused the health benefits of eating garlic bread with cheese to their
    children, with the food earning such an iconic status in our culture that kids will often dress
    up as warm, cheesy loaf for Halloween.
  </p>
  <p>
    But a recent study shows that the celebrated appetizer may be linked to a series of rabies cases
    springing up around the country.
  </p>
  <!-- ... -->
</article>
```

Tailwindcss-typography doc : https://github.com/tailwindlabs/tailwindcss-typography

### Production build

Use 'npm run prod' to compile your assets with purge and minimize options.

```
npm run prod
```

When you deploy your site, you do not need to copy the node_modules folder! It is a huge folder, over 100Mb, and it is only needed in the development stage. It is utterly useless in the production environment (if you do need that folder in the production environment, you know what you're doing and you can ignore this comment)

### Activating vue.js

The theme is ready to support vue - all you have to do is:

- uncomment the contents of assets/src/js/app.js
- add id="app" to an outer div in the layout - there is a comment in the layout file to assist with that
  When you run npm install in the setup phase, all support libraries for using vue will be downloaded and ready to use, but they will not be loaded unless you activate vue in app.js

### Activating Alpine.js + Spruce.js + Axios

The theme is alternativel ready to support Alpine.js as a Vue replacement. All you have to do is:

- in the layouts/default uncomment {% partial 'site/scripts_alpine' %} (replace # with %)

Note: make sure you do not activate both Vue.js and Alpine.js at the same time. Or, if you do, make sure that Vue does not take over the whole DOM - use it locally, otherwise it will conflict with Alpine tags.

### Theme License

MIT License - check out [LICENSE.md](LICENSE.md) file for MIT license details

### Changelog

#### 2.0.0 - 2020-11-20

Important / potentially breaking changes!

- TailwindCSS upgraded to `postcss7-compat@^2.0.1` for compatibility reason with Laravel-mix dependencies (postcss-loader not compatible with postcss8), check https://tailwindcss.com/docs/installation#post-css-7-compatibility-build
- `Typography` TailwindCSS plugin removed temporarely to satisfy TailwindCSS 2.0.1-compat dependencies.
- Possibility to add fonts localy via npm `npm install fontsource-{FONT_NAME})`, check https://github.com/fontsource/fontsource for details, the installed font can be imported in `themes/oc-tabularasa-theme/assets/src/css/app.css`.
  Check https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/ to find why self-hosting fonts is significantly faste than loading a typeface from Google Fonts or other hosted font service.

```
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
@import "extra.css";

/* Add you fonts below, example */
@import "~fontsource-poppins/400.css";

```

#### 1.0.2 - 2020-11-11

Important / potentially breaking changes!

- Added `Typography` TailwindCSS plugin to requirements. This replaces htmlcontent.css (removed). Use `prose` class instead of `htmlcontent`.
- Removed postcss-\* from weback.mix.js
- Added standard "gray-100..900" colors. This will replace now deprecated grey-lightest..darkest - it does not make much sense to use custom version of grey, as grey is pretty much the same whether custom or standard.
- Modified default layout: id=app now removed, if you need it - add it. Global vue instance was clashing with local Alpine.js apps.
- Added partials/site/scripts_alpine.htm for those folks who - like me - prefer to switch from Vue to Alpine. Disabled by default. That partial loads: Alpine.js (2.x), Spruce (2.x), and Axios.
- Updated jquery to 3.5.1 in partials/site/scripts
