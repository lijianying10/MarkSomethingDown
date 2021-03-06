# GitHub Access Token

![octotree 被限速](https://raw.githubusercontent.com/moooofly/ImageCache/master/Pictures/Access%20Token%20problem%20when%20using%20octotree.png "octotree 被限速")

Octotree uses [GitHub API](https://developer.github.com/v3/) to retrieve repository metadata. By default, it makes **unauthenticated requests** to the GitHub API. However, there are two situations when requests must be authenticated:

- You access a private repository
- You exceed the [rate limit of unauthenticated requests](https://developer.github.com/v3/#rate-limiting)

When that happens, Octotree will ask for your [GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/). If you don't already have one, [create one](https://github.com/settings/tokens/new), then copy and paste it into the textbox. Note that the minimal scopes that should be granted are `public_repo` and `repo` (if you need access to private repositories).

![Cache/Pictures/new personal access token - 1](https://raw.githubusercontent.com/moooofly/ImageCache/master/Pictures/new%20personal%20access%20token%20-%201.png "Cache/Pictures/new personal access token - 1")

![Cache/Pictures/new personal access token - 2](https://raw.githubusercontent.com/moooofly/ImageCache/master/Pictures/new%20personal%20access%20token%20-%202.png "Cache/Pictures/new personal access token - 2")

----------


## [Rate Limiting](https://developer.github.com/v3/#rate-limiting)

The returned HTTP headers of any API request show your current rate limit status:

```
curl -i https://api.github.com/users/octocat
HTTP/1.1 200 OK
Date: Mon, 01 Jul 2013 17:27:06 GMT
Status: 200 OK
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 56
X-RateLimit-Reset: 1372700873
```

| Header Name | Description |
| -- | -- | 
| **X-RateLimit-Limit** | The maximum number of requests you're permitted to make per hour. |
| **X-RateLimit-Remaining** | The number of requests remaining in the current rate limit window. |
| **X-RateLimit-Reset** | The time at which the current rate limit window resets in UTC epoch seconds. |

If you exceed the rate limit, an error response returns:

```
HTTP/1.1 403 Forbidden
Date: Tue, 20 Aug 2013 14:50:41 GMT
Status: 403 Forbidden
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1377013266

{
   "message": "API rate limit exceeded for xxx.xxx.xxx.xxx. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)",
   "documentation_url": "https://developer.github.com/v3/#rate-limiting"
}
```

## GitHub API rate limit

```shell
Error: GitHub API rate limit exceeded for 220.166.254.65. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
Try again in 56 minutes 17 seconds, or create an personal access token:
  https://github.com/settings/tokens
and then set it as HOMEBREW_GITHUB_API_TOKEN.
```

解决办法：

- 注册、登录 https://github.com ；
- 访问 https://github.com/settings/tokens ；
- 在 `Personal settings -> Personal access tokens` 中点击 "Generate new token" 创建 token ；
- Make sure to copy your new personal access token now. You won't be able to see it again!
- Personal access tokens function like ordinary OAuth access tokens. They can be used instead of a password for Git over HTTPS, or can be used to authenticate to the API over Basic Authentication.
- 在 `~/.bashrc` 中添加 `HOMEBREW_GITHUB_API_TOKEN` 环境变量

```shell
if [ -f /usr/local/bin/brew ]; then
    export HOMEBREW_GITHUB_API_TOKEN=xxxxxxxxxx
fi
```


## [Creating a personal access token for the command line](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)

略

