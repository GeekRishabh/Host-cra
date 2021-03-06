# create-react-app with a ExpressJs server on Heroku

A minimal example of using a ExpressJs backend (server for API, proxy, & routing) with a [React frontend](https://github.com/facebookincubator/create-react-app).

To deploy a frontend-only React app, use the static-site optimized  
▶️ [create-react-app]

⤵️ [Switching from create-react-app-buildpack](#switching-from-create-react-app-buildpack)?


## Design Points

A combo of two npm projects, the backend server and the frontend UI. So there are two `package.json` configs and thereforce two places to run `npm` commands:

  1. [`package.json`](package.json) for [Node server](server/) & [Heroku deploy](https://devcenter.heroku.com/categories/deployment)
      * `heroku-postbuild` script compiles the webpack bundle during deploy
      * `cacheDirectories` includes `react-ui/node_modules/` to optimize build time
  2. [`react-ui/package.json`](react-ui/package.json) for [React web UI](react-ui/)
      * generated by [create-react-app](https://github.com/facebookincubator/create-react-app)

Includes a minimal [Node Cluster](https://nodejs.org/docs/latest-v8.x/api/cluster.html) [implementation](server/index.js) to parallelize the single-threaded Node process across the available CPU cores.

## Demo

[Demo deployment](https://cra-node.herokuapp.com/): example API call from the React UI is [fetched with a relative URL](react-ui/src/App.js#L16) that is served by an Express handler in the Node server.


## Deploy to Heroku

```bash
git clone https://github.com/GeekRishabh/Host-cra.git
cd Host-cra/
heroku create
git push heroku master
```

This deployment will automatically:

  * detect [Node buildpack](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-nodejs)
  * build the app with
    * `npm install` for the Node server
    * `heroku-postbuild` for create-react-app
  * launch the web process with `npm start`
    * serves `../react-ui/build/` as static files
    * customize by adding API, proxy, or route handlers/redirectors

⚠️ Using npm 5’s new `package-lock.json`? We resolved a compatibility issue.

👓 More about [deploying to Heroku](https://devcenter.heroku.com/categories/deployment).
  

## Local Development

### Run the API Server

In a terminal:

```bash
# Initial setup
npm install

# Start the server
npm start
```


### Run the React UI

The React app is configured to proxy backend requests to the local Node server. (See [`"proxy"` config](react-ui/package.json))

In a separate terminal from the API server, start the UI:

```bash
# Always change directory, first
cd react-ui/

# Initial setup
npm install

# Start the server
npm start
```
