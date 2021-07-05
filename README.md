# Experiment with Jekyll Auth

Experiment with the [jekyll-auth module](https://github.com/benbalter/jekyll-auth).

The software in this project is highly experimental. Its only purpose is for me to learn about integrating authentication into a Jekyll site.

## Note

To save costs for the environment I have removed the corresponding heroku instance. In order to reactivate it, please follow the steps in the section [Reactivate heroku instance](#reactivate-heroku-instance) below.

## Steps required to make the Jekyll page work on heroku

* ✅ Create a minimal Jekyll site
* ✅ Create a free [heroku](https://www.heroku.com) account
* ✅ Follow the [Getting Started](https://github.com/benbalter/jekyll-auth/blob/master/docs/getting-started.md) instructions for the [jekyll-auth module](https://github.com/benbalter/jekyll-auth)
* ✅ Fix: Cannot open https://experiment-with-jekyll.herokuapp.com/ although I am member of the organisation. See next section.

## Fix access denied error

Please refer to my [post to the maintainer of jekyll-auth](https://github.com/benbalter/jekyll-auth/issues/141#issuecomment-870213536):

[...] After following the [Getting Started](https://github.com/benbalter/jekyll-auth/blob/master/docs/getting-started.md) guide, I ended up with my sample Jekyll page showing the error 403 access denied cat.

Please consider adding the following steps to the documentation. They fixed the problem on my side:

1. Add a team to your organization containing the members who should have access and configure the GITHUB_TEAM_ID for heroku
2. Grant OAuth App access to your Jekyll App in your organization

## Detailed step descriptions

### 1. Add a team to your organization containing the members who should have access and configure the GITHUB_TEAM_ID for heroku

* Create a team within your organisation
* Create a REST API token for your lokal heroku / jekyll installation
* Save the REST API token by adding the line `GITHUB_TOKEN=<your token>` to `.env` (never commit that!)
* Find out your numeric team id by `jekyll-auth team_id --org <your org name> --team <your team name>`
* Tell heroku your team id by `heroku config:set GITHUB_TEAM_ID=<your numeric team id>`
* You can delete the REST API token added in step 2 from your GitHub account

### 2. Grant OAuth App access to your Jekyll App in your organization

* After the team id was provided to the application, it showed an **error 500**.
* Checking the logs on heroku revealed the message <br>`2021-06-29T03:44:45.449035+00:00 app[web.1]: 2021-06-29 03:44:45 - Octokit::Forbidden - GET https://api.github.com/teams/4923947/members/wonderbird: 403 - Although you appear to have the correct authorization credentials, the `boos-systems` organization has enabled OAuth App access restrictions, meaning that data access to third-parties is limited. For more information on these restrictions, including how to enable this app, visit https://docs.github.com/articles/restricting-access-to-your-organization-s-data/ // See: https://docs.github.com/rest:`
* [Approving OAuth Apps for your organization](https://docs.github.com/en/organizations/restricting-access-to-your-organizations-data/approving-oauth-apps-for-your-organization) made the page work. You have to select your heroku application and grant access.

## Reactivate Heroku instance

1. Create a free Heroku account
1. Create a GitHub organization and a team containing allowed users
1. Update variables which might be saved in your `.env` file (not checked into version control)
1. Follow the [instructions to set up hosting with heroku manually](https://github.com/benbalter/jekyll-auth/blob/master/docs/getting-started.md#manually)
1. Follow the steps described in [Fix access denied error](#fix-access-denied-error) above
