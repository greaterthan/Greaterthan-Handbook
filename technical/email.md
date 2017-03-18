# Email

## Stack
* Email sending leverages Rails' [action mailer](http://guides.rubyonrails.org/action_mailer_basics.html)
* Action mailer is integrated with [active job](http://edgeguides.rubyonrails.org/active_job_basics.html), which is a generic API for queueing jobs
* ActiveJob requires you to specify a backend. there are a few built in by default. We use one of them, called "delayed job"
* in `cobudget-api/config/application.rb:` `config.active_job.queue_adapter = :delayed_job`
* delayed_job is an external Gem, listed as a dependency in the Gemfile. 

## Development
When developing with emails locally, make sure mailcatcher is running. assuming
mailcatcher is installed (`gem install mailcatcher`), in a terminal window just
run `mailcatcher`, then visit `http://localhost:1080/` to see your mails coming
through. 

Note, for messages to get queued for actual sending, you need to append a
[delivery
call](http://edgeapi.rubyonrails.org/classes/ActionMailer/MessageDelivery.html)
(`deliver_now`, `deliver_later`) otherwise you are just creating a MailMessage
object, and it will seem to be failng silently because you're actually not
asking for it to be delivered. 

this is working for `deliver_now` emails. i am still not sure how deliver_later
is working, or whether it's working. i don't see them coming through
mailcatcher, and am not sure if it's because the delayed_job queue is not
running or something else. 

run `bundle exec rake jobs:work` to start a background process for delayed jobs.

additional info about delayed_job at
https://github.com/collectiveidea/delayed_job/wiki/Delayed-job-command-details


## Production 

## Sending
Production mails are being sent through sendgrid. we do not have a login for
the existing account yet. 


## Log reporting emails
`config/initializers/delayed_job_airbrake.rb` seems to be configured to work
with [airbrake](https://airbrake.io/) on production, but there are no airbrake
settings configured on heroku. 

