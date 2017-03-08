# Deploying

## Frontend Deployment Pipeline

* tools: github pages, gulp, travis. 

The app frontend is developed in the [`cobudget-ui`](https://github.com/cobudget/cobudget-ui) repo and is hosted using [github pages](https://pages.github.com/). This is done by building the app locally into a `build` directory, and then pushing that `build` directory to a separate, dedicated production (or staging) repo ([cobudget.co](https://github.com/cobudget/cobudget.co) and [staging.cobudget.co](https://github.com/cobudget/staging.cobudget.co), respectively). 

* package.json has scripts for `stage` and `deploy` which uses a tool called [gulp-build-branch](https://www.npmjs.com/package/gulp-build-branch) to publish the `build` directory to a specific remote (staging or production). it creates a new repo _inside_ the `build` dir, and then deploys that to the `gh-pages` branch of the staging/remote repo. 
  * note: i'm not totally clear how `npm run deploy-push` knows to only push the contents of the `build` directory, since it appears to be running at the primary repo level. 
  
  There is a > 2 year old travis instance for the cobudget-ui repo, so I guess this has not been getting used. 

### Deploy Process (draft)
1. make sure tests pass locally (add detail...)
* submit a pull request to `master` (should trigger travis to run tests - need detail on tests that get run...)
* have someone else merge it
* push to staging: `npm run stage`
* push to production: `npm run deploy`

## Backend Deployment Pipeline

* The app backend is developed in the [cobudget-api](https://github.com/cobudget/cobudget-api) repo and is hosted on heroku. 
* Heroku config...
* There is currently no staging server for the api.  
* There is an active travis instance for the cobudget-api repo. 

### Deploy Process (draft)
1. make sure tests pass locally (add detail...)
* submit a pull request to `master` (should trigger travis to run tests)
* have someone else merge it
* `git push heroku-staging master`
* `git push heroku-production master`


## Syncing backend and frontend
