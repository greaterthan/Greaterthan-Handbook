# Development

## Comms
  * We use Slack for day to day comms
  * Our tickets live in Github
  * We have 2x weekly standsups and bi-weekly sprint planning.
  * We plan to add waffle.io integration.  

## Tickets
* We are learning the right scoping process for our tickets. As a start, please include _who_, _what_ and _why_ in tickets and issues, and as possible focus on user stories so that tickets include some user-facing result. 

## Branch structure, naming and other norms
* When a project has a `api` repo and a corresponding `ui` repo, if you are making changes in both, sync names between them. 
* We use `master` as the main trunk from which branches should be made and pull requests submitted to. 
* Use `feature/feature-name` and `bug/issue` naming for branches
* Feel free to submit `WIP` (work-in-progress) branches as pull requests if you desire feedback. Naming them with `WIP` will indicate they are not to be merged.   

## Code review
* For each ticket, use a pull request when it's ready to have someone else look at the code.
* This ensures we have two sets of eyes on each contribution so that multiple people are familiar with new code being added.
* As appropriate ask someone who didn't write the code to do a UI/useabilty review

## Tests
* Ideally a different person than the primary writer of a feature should write the corresponding test
