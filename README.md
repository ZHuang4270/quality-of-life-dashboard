# Quality of Life Dashboard v3

A dashboard for community data and health. Forked from Tobin Bradley's work on Mecklenburg QoL dashboard. See their [demo site](http://mcmap.org/qol-dev).

The original Mecklenburg repository with old versions of the Dashboard is [here](https://github.com/tobinbradley/Mecklenburg-County-Quality-of-Life-Dashboard).

## Related Projects

*   [quality-of-life-embed](https://github.com/tobinbradley/quality-of-life-embed)
*   [quality-of-life-report](https://github.com/tobinbradley/quality-of-life-report)
*   [quality-of-life-data](https://github.com/DataWorks-NC/durham-quality-of-life-data)

## Get Started

This project requires [NodeJS](https://nodejs.org).

``` bash
git clone https://github.com/DataWorks-NC/quality-of-life-dashboard.git dashboard
cd dashboard
git clone https://github.com/DataWorks-NC/durham-quality-of-life-data data
npm install
```

After running npm install, and after each fresh time re-running npm install, you may need to make a one-line correction to fix a bug in the Mapbox GL library which `npm` installs by default. From root of the repo, run

`sed -i '' "s_require('rest');_require('rest/browser.js');_" node_modules/mapbox/lib/client.js`.

You'll then need to populate `private.js` in the `data/config` directory, following the instructions in https://github.com/DataWorks-NC/durham-quality-of-life-data/blob/master/README.md.

Then run

```bash`
npm run build
npm start
```

You should now be able to access the dashboard locally at http://127.0.0.1:3000/.

To build the site for production, see Deployment, below, and the Tehnical Infrastructure Manual.

## Customizing the Dashboard

Most Dashboard customization can be accomplished by creating your own data repository [following the directions here](https://github.com/tobinbradley/mecklenburg-quality-of-life-data). The data repository includes Dashboard meta (title, author, keywords, etc.), map style and configuration settings, data, etc.

The Dashboard is built using [Vue.js](http://vuejs.org/), [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/), and [Material Design Lite](https://getmdl.io/). The business end of things consists of independent Vue.js components in `app/js/components`. You can very easily create or disable components as needed. Each component has a shared state between all the components, and some components have a private state for things only needed by that component.

The `app/js/search.vue` component searches for the geography id, zip codes, and addresses. Zip code and address searches use HTTP API's from our [Dirt Simple PostGIS HTTP API](https://github.com/tobinbradley/dirt-simple-postgis-http-api) project with Mecklenburg data and won't work for other areas (only the geography ID search will work). Setting up new searches is fairly straight-forward and you can customize it to meet your needs.

To disable the non-geography searches, comment out everything but `searchNeighborhood` in the `search` method of `app/js/search.vue`.

``` javascript
search: function() {
    let query = this.privateState.query.trim();

    this.searchNeighborhood(query);
    //this.searchAddress(query);
    //this.searchZipcode(query);
},.
```

## Deployment

Further deployment instructions in the DataWorks NC Technical Infrastructure Manual.

This project includes a python script, `.circleci/deploy.py` for pushing the website to Azure using Blob storage for storage.
In order for this command to succeed, you'll need to have environment variables `AZURE_STORAGE_CONNECTION_STRING` and `AZURE_DESTINATION_BLOB` set to the connection string for your Azure storage container and the destination blob name, respectively. We recommend using `direnv` (https://direnv.net/) to track
these environment variables, as described [here](https://www.taos.com/using-multiple-accounts-aws-cli-direnv/).

To deploy, run `python .circleci/deploy.py` (make sure that `python-dotenv` is installed locally using `pip install python-dotenv`, and that the Microsoft Azure CLI is also installed). This will deploy everything in the `public` directory to the blob name set in `AZURE_DESTINATION_BLOB` This Azure hosting strategy follows [these instructions by Hao Luo](https://blog.lifeishao.com/2017/05/24/serving-your-static-sites-with-azure-blob-and-cdn).
