# PlanScore Front-Facing Website

Totally separate from the back-end code, this is the public-facing website. This is run from thre `gh-pages` branch of the `PlanScore/PlanScore` repository.

## Site URL

https://planscore.github.io/PlanScore/home/



## Data CSVs

The datasets which power the site, are under the `data/` folder.

Data dictionary for bias CSVs: https://docs.google.com/spreadsheets/d/1eMwwL8eaxD3aAyiy410yt79Cik2GV31WrXWyzFsiSTs/edit#gid=1340292617


## Development and Code Layout

Babel, SASS/SCSS, Webpack.

Upon initial setup on your system, run `nvm use` and `yarn install` to set up build tools.

Each page is its own folder, so this may be served as static files without special webserver configuration.

Your edits would be made to each folder's **index.src.html** **index.scss** **index.js6**

Running `npm run build` will compile the browser-consumable files **index.html** **index.css** **index.js** within each folder. Note that these outputs **are** included in version control, so they may be hosted via Github Pages without us needing to work in additional tooling.

`npm run serve` will run a HTTP server, as well as watching and rebuilding (below).


### The State Pages Are Special

**Do not edit the `index.*` files for the 50 states.** There are 50 directories and thus pages for Alabama, Alaska, ... etc. But we do not maintain 50 separate **index.js6** **index.scss** **index.src.html** files!

Instead, you'll want to make edits to the `_statetemplate` files (**state_template.js6**, **state_template.scss**, **state_template.src.html**). Then you would `npm run states` to copy these template files into the 50 target folders.

If you are running *webpack-dev-server*, it will automagically detect the files having changed, and will trigger a rebuild and reload as usual. Note that this takes a moment; be patient.

The usual `npm run build` step is required for state pages if you expect to depploy to the site, same as with other pages.

While working within the `_statetemplate/` content, some notes of interest:
* **state_template.src.html** -- You may insert the phrase **STATE_NAME** into the HTML. When the template is copied into the state folder, this will be replaced with the state's Properly Capitalized Name.
* All states will receive exactly the same programming, stylesheet, and HTML template (except for the STATE_NAME tag), so they should "detect" their state based on the URL string, if they will need to filter data or otherwise configure custom behavior.


### Shared Content / Partial Views

Some content is shared between pages, such as the sitewide navbar, the footer, and some of the HTML HEAD content. These are called "partial views".

To use them in the HTML templates, simply use the following tags. They will be substituted when `npm run build` is done, or when the webpack-dev-server is restarted.
```
<!--[include_headtags]-->
<!--[include_navbar]-->
<!--[include_footer]-->
```

Note that *webpack-dev-server* will not reload these when they are modified. If you are making changes to these common elements, you will need to stop and restart the *webpack-dev-server*.

If you need to define some new partial views, the following will be useful:
* These are defined in the `htmlpartials/` folder. Each one defines a JavaScript variable containing a block of HTML, e.g. the navbar.
* The `webpack.config.js` defines **HTML_PARTIALS** by loading those files.
* The `webpack.config.js` then uses **StringReplacePlugin** to perform the above-mentioned substitutions, e.g. `<!--[include_footer]-->` as the `.src.html` templates are processed.


### Deployment

This is run from the `gh-pages` branch of the `PlanScore/PlanScore` repository.

Be sure you are using the `gh-pages` branch.

After running the `npm run build`, commit the new built files into a new commit, and then push. This will cause the GH Pages site to be updated. (though it can take a few minutes)
