# Deploying

## Frontend Deployment Pipeline

The app frontend is developed in the [`cobudget-ui`](https://github.com/cobudget/cobudget-ui) repo and is hosted using [github pages](https://pages.github.com/). This is done by building the app locally into a `build` directory, and then pushing that `build` directory to a separate, dedicated production (or staging) repo ([cobudget.co](https://github.com/cobudget/cobudget.co) and [staging.cobudget.co](https://github.com/cobudget/staging.cobudget.co), respectively). 

`package.json` has scripts for `stage` and `deploy` which uses a tool called [gulp-build-branch](https://www.npmjs.com/package/gulp-build-branch) to publish the `build` directory to a specific remote (staging or production). it creates a new repo _inside_ the `build` dir, and then deploys that to the `gh-pages` branch of the staging/remote repo. 

* note: i'm not totally clear how `npm run deploy-push` knows to only push the contents of the `build` directory, since it appears to be running at the primary repo level. 
* There is a > 2 year old travis instance for the cobudget-ui repo, so I guess this has not been getting used. 

### Deploy Process (draft)
1. submit a pull request to `master` 
1. have someone else merge it
1. pull down `master` locally: `git pull origin master`
1. make sure remotes are set: `npm run set-remote-stage` or `npm run set-remote` as appropriate
1. to push to [staging](https://github.com/cobudget/staging.cobudget.co/tree/gh-pages): `npm run stage`
1. to push to [production](https://github.com/cobudget/cobudget.co/tree/gh-pages): `npm run deploy`
1. reset `$NODE_ENV=development` after deploy so that the repo looks for the right backend URL (localhost). 

### Rollbacks
1. [reset head](https://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git) on production: `git reset HEAD~1`
2. fix the bug locally 
3. Push the fixes
4. run `npm run deploy` again, which will push the new changes and update HEAD past the broken commit to the new one.  

## Backend Deployment Pipeline
* The app backend is developed in the [cobudget-api](https://github.com/cobudget/cobudget-api) repo and is hosted on heroku. 
* Heroku config...
* There is currently no staging server for the api.  
* There is an active travis instance for the cobudget-api repo. 

### Database and backups
* Database is Postgres
* Database is scheduled to be backed up daily. You can check backup schedules with `heroku pg:backups:schedules`. 
* View the latest backups by following the link to the database from the heroku dashboard, or using the CLI. 
* More on downloading and restoring from backup [here](https://devcenter.heroku.com/articles/heroku-postgres-backups). 

### Deploy Process (draft)
1. make sure tests pass locally: run `rspec`
* submit a pull request to `master`
* ensure travis tests have passed
* have someone else merge it
* `git push heroku master`
* `heroku run rake db:migrate`

### Rollbacks
Heroku has a [`rollback`](https://devcenter.heroku.com/articles/releases) command. Rolling back on Heroku will only 'reactivate' a previous commit, it won't actually change the repository on Heroku. So if you were to then pull and push your Heroku remote (which should usually do nothing), you'll actually redeploy the commit that you rolled back.). Rollback also does not update the database or rollback migrations, so this has to be done manually. 

* roll back to the previous commit: `heroku rollback`
* roll back to the previous DB version if necessary: `heroku run rake db:rollback STEP=1` (or however many steps are appropriate). 

## Timing: backend and frontend
* Use an appropriate ordering between frontend and backend when deploying changes to both repos. Usually, deploy backend first. 
