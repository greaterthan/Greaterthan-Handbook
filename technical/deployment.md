# Deploying

## Deployment pipeline - Frontend

* tools: github pages, gulp, travis. 

The app frontend is developed in the [`cobudget-ui`](https://github.com/cobudget/cobudget-ui) repo and is hosted using [github pages](https://pages.github.com/). This is done by building the app locally into a `build` directory, and then pushing that `build` directory to a separate, dedicated production (or staging) repo ([cobudget.co](https://github.com/cobudget/cobudget.co) and [staging.cobudget.co](https://github.com/cobudget/staging.cobudget.co), respectively). 

* package.json scripts and configuration...
* CNAME changes...
* [gulp-build-branch](https://www.npmjs.com/package/gulp-build-branch)...


### CI
* where
* how

### Deploy Process
* run tests locally (add detail...)
* push to staging: `npm run deploy`
* push to production: `npm run stage`


## Deployment pipeline - Backend

### Deploy Process

## Syncing backend and frontend
