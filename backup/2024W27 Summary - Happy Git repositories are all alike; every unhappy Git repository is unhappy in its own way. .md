## This Week I Discovered
> _Happy Git repositories are all alike; every unhappy Git repository is unhappy in its own way._ —Linus Tolstoy

### Tools

Opensouce tools:
- [Do Something .new](https://whats.new/), whats.new provides shortcuts for some services, I love [gist.new](http://gist.new), [resume.new](http://resume.new) and [docs.new](http://docs.new).
- [chsrc](https://github.com/RubyMetric/chsrc), as said in the brief. I tried to change my cargo registry, but it can't recognise my configured sources because I created `.cargo/config` to keep them.
- [git-sizer](https://github.com/github/git-sizer), an easy-to-use sizer tool, and mentioned some useful tips for building **happy Git repositories.** 
- [git-bomb](https://kate.io/blog/git-bomb/), git-sizer mentioned this "git bomb".

Commercial tools:
- [18bit DNS](https://www.18bit.cn/index.html), 18bit DNS is a [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) service hosted in China, it may works well, but the user can't feel how important they are.
- [TradingView](https://www.tradingview.com/), data the market, interactive datasheet makes it easier to communicate.
- [TablePlus](https://tableplus.com/), an elegent db client for Mac.


### Projects
- [Neobrutalism components](https://www.neobrutalism.dev/), a collection of neobrutalism-styled Tailwind components.


## This Week I Learned
### GitHub Actions workflow in Gmeek project
Gmeek workflow file is [here](https://github.com/Meekdai/meekdai.github.io/blob/afa8456928cba3d75ec9406a44a505180f0aa65d/.github/workflows/Gmeek.yml)

#### Part 1, Events that trigger workflows used by Gmeek
1. [`workflow_dispatch`](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch): manual operation would trigger the flow.
2. [`issues`](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#issues): issue events would trigger the flow.
3. [`schedule`](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule): trigger the workflow at a scheduled time.

<details>

<summary>Some of the events would also be recorded in the GitHub Webhook payload. After triggering the flow, some of the flow [variables](https://docs.github.com/en/actions/learn-github-actions/variables) would be changed. </summary>

| Webhook event payload                                                                                                      | Activity types                                                                                                                                                                                                                                                                          | `GITHUB_SHA` after triggering                 | `GITHUB_REF` after triggering        |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- | ------------------------------------ |
| [workflow_dispatch](https://docs.github.com/en/webhooks-and-events/webhooks/webhook-events-and-payloads#workflow_dispatch) | Not applicable                                                                                                                                                                                                                                                                          | Last commit on the `GITHUB_REF` branch or tag | Branch or tag that received dispatch |
| Not applicable                                                                                                             | Not applicable                                                                                                                                                                                                                                                                          | Last commit on default branch                 | Default branch                       |
| [`issues`](https://docs.github.com/en/webhooks-and-events/webhooks/webhook-events-and-payloads#issues)                     | - `opened`  <br>- `edited`  <br>- `deleted`  <br>- `transferred`<br>- `pinned`  <br>- `unpinned`  <br>- `closed`  <br>- `reopened`  <br>- `assigned`  <br>- `unassigned` <br>- `labeled`  <br>- `unlabeled`  <br>- `locked`  <br>- `unlocked`  <br>- `milestoned`  <br>- `demilestoned` | Last commit on default branch                 | Default branch                       |

</details>

#### Part 2, Actions from [GitHub Marketplace](https://github.com/marketplace)
1. [`checkout@v4`](https://github.com/marketplace/actions/checkout), this action **on default** fetch the **single** commit on `$GITHUB_REF/$GITHUB_SHA` .
2. [`configure-pages@v4`](), this action **on default** enables Pages and extracts various metadata about the site.
3. [`setup-python@v5`](https://github.com/marketplace/actions/setup-python), this action setups Python environment including specific Python version, pack manager, and finally sets the path.
4. [`upload-pages-artifact@v3`](https://github.com/marketplace/actions/upload-github-pages-artifact), this action composite the static assets to be deployed on GitHub Pages using `tar` command.
5. [`deploy-pages@v4`](https://github.com/marketplace/actions/deploy-github-pages-site), this action deploys the artifact uploaded in the action above.

All the workflows that build the repository and publish it to GitHub Pages commonly use the actions above, and GitHub provides a [starter-workflow](https://github.com/actions/starter-workflows/tree/main/pages).

#### Part 3, Other jobs
1. Cloning the generator script and templates
	- the author uses `jq` command to parse `GMEEK_VERSION` and decide which version to clone.
2. Parsing configurations
	- the author uses [`json`](https://docs.python.org/3/library/json.html) package in the Python script to use other configurations, then generate HTMLs.
3. Generate the blogs
	- the author created a `GMEEK` class, it contains all the members and methods for creating the static assets
		- important members: `options`, `user`
		- important methods: 
			- `defaultConfig()`, this function reads configs from `config.json`.
			- `markdown2html(mdstr)`, calls github's markdown api to get html.
			- `renderHtml(template, blogBase, postListJson, htmlDir, icon)`, loads templates and render the final html file.
			- `createPostHtml(issue)`, opens `.md` file, gets `.html` output by calling `markdown2html()`, checks MathJax and alerts, configs the post info, render the final `.html` by calling `renderHtml()`.
4. Commit and push to the repository.

### JSON format and `jq` command
I've been wrong about understanding JSON format for a long time, almost every programming language supports JSON.

[JSON](https://www.json.org/json-en.html) is a data-interchange format, a JSON object is an unordered set of name/value pairs seperated by comma.

[`jq`](https://jqlang.github.io/) usage: [jq tutorial](https://jqlang.github.io/jq/tutorial/), the tutorial takes jq's GitHub repository as example to show how the command should be use.

## This Week I Built
### A Blog
[Blog site: immelon.top](https://immelon.top/), thanks to [Gmeek](https://github.com/Meekdai/Gmeek-template).

Gmeek uses GitHub Actions to render issues into HTMLs, then GitHub Pages render the repository on the \<username\>.github.io, comments on the issues would be rendered into comment widget of the blog by [utterance](https://github.com/utterance/utterances)

There are several other projects follows the road above(or similarily), they are different to some extent:
1. [Gitblog | Lightweight Blogging Solution](https://gitblog.io/), many more themes, provides analytics, needs to be paid for more than one blog under one account and API access(to analytics?).
2. [ yihong0618/gitblog: People Die, but Long Live GitHub](https://github.com/yihong0618/gitblog), opensource, but many complex configurations.
3. [GitHub - imuncle/gitblog](https://github.com/imuncle/gitblog), integrates configuration into one file, provides API access to the blog.

Which aspect of Gmeek attracts me most?
1. Easy and fast, two-step configuration based on [GitHub template repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository), I can even configure nothing to start writing!
2. [Primer](https://primer.style/) style UI, github's stable and pretty design system.

