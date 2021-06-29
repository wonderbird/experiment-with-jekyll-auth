# Experiment with Jekyll Auth

Experiment with the [jekyll-auth module](https://github.com/benbalter/jekyll-auth).

The software in this project is highly experimental. Its only purpose is for me to learn about integrating authentication into a Jekyll site.

## Steps required to make the Jekyll page work on heroku

* ✅ Create a minimal Jekyll site
* ✅ Create a free Heroku account
* ✅ Follow the [Getting Started](https://github.com/benbalter/jekyll-auth/blob/master/docs/getting-started.md) instructions for the [jekyll-auth module](https://github.com/benbalter/jekyll-auth)
* ✅ Fix: Cannot open https://experiment-with-jekyll.herokuapp.com/ although I am member of the organisation. Error 403 access denied.
  * In any case you need to provide heroku with a **team id**. I thought configuring an organization is sufficient. To do so,
    * Create a team within your organisation
    * Create a REST API token for your lokal heroku / jekyll installation
    * Save the REST API token by adding the line `GITHUB_TOKEN=<your token>` to `.env`
    * Find out your numeric team id by `jekyll-auth team_id --org boos-systems --team researchers`
    * Tell heroku your team id by `heroku config:set GITHUB_TEAM_ID=<your numeric team id>`

  * You need to grant **OAuth App access** to your app
    * After the team id was provided to the application, it showed an **error 500**.
    * Checking the logs on heroku revealed the message <br>`2021-06-29T03:44:45.449035+00:00 app[web.1]: 2021-06-29 03:44:45 - Octokit::Forbidden - GET https://api.github.com/teams/4923947/members/wonderbird: 403 - Although you appear to have the correct authorization credentials, the `boos-systems` organization has enabled OAuth App access restrictions, meaning that data access to third-parties is limited. For more information on these restrictions, including how to enable this app, visit https://docs.github.com/articles/restricting-access-to-your-organization-s-data/ // See: https://docs.github.com/rest:`
    * [Approving OAuth Apps for your organization](https://docs.github.com/en/organizations/restricting-access-to-your-organizations-data/approving-oauth-apps-for-your-organization) made the page work. You have to select your heroku application and grant access.