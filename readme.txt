=== Documents from Git ===
Contributors: Nils Nolde
Tags: markdown,github,bitbucket,gitlab,vcs
Requires at least: 5.0.0
Tested up to: 5.3.2
Requires PHP: 5.6
Stable tag: 1.0.0
License: GPLv3
License URI: https://github.com/gis-ops/wordpress-markdown-git/blob/master/LICENSE

A plugin to inject files directly into a post from popular Git platforms.

Currently supported file types: Markdown.

Currently supported platforms: Github, Bitbucket.

== Description ==

This WordPress Plugin lets you easily publish, collaborate on and version control your \[**Markdown, Jupyter notebook**\] documents directly from your favorite remote Git platform, **even if it's self-hosted**.

The advantages are:

- Write documents in your favorite editor and just push to your remote repository to update your blog instantly
- Use the power of version control: publish different versions of the document in different posts, i.e. from another branch or commit than latest `master`
- Easy to update by external users via pull requests, minimizes the chance of stale tutorials

The following document types are currently supported:

- Markdown
- Jupyter notebooks (**only Github supported**)

The following platforms are currently supported:

- Github
- Bitbucket
- Gitlab

## Usage

### Shortcodes

The plugin features a variety of shortcodes.


#### Publish documents

The document-specific shortcode follow a pattern of `[git-<platform>-<action>]`, where

- `<platform>` can be one of
    - `github`: if you use Github as your VCS platform
    - `bitbucket`: if you use Bitbucket as your VCS platform
    - `gitlab`: if you use Gitlab as your VCS platform
- `<action>` can be one of
    - `markdown`: Render your Markdown files hosted on your VCS platform in Github's rendering style
    - `jupyter`: Render your Jupyter notebook hosted on your VCS platfrom (**only Github supported**)
    - `checkout`: Renders a small badge-like box with a link to the document and the date of the last commit
    - `history`:  Renders a `<h2>` section with the last commit dates, messages and authors

#### Manipulate rendering style

Additionally, there's an enclosing shortcode `[git-add-css]` which adds a `<div id="git-add-css" class="<classes_attribute>"` to wrap its contents. That way you can manipulate the style freely with additional CSS classes. Follow these steps:

1. Add a CSS file to your theme's root folder, which contains some classes, e.g. `class1`, `class2`, `class3`
2. Enqueue the CSS file by adding `wp_enqueue_style ('git-markdown-style', get_template_directory_uri().'/my-style.css');` to the theme's `functions.php`
3. Add the enclosing `git-add-css` shortcode to your post with the custom CSS classes in the `classes` attribute, e.g.:

```
[git-add-css classes="class1 class2 class3"]
    [git-gitlab-checkout url=...]
    [git-gitlab-markdown url=...]
    [git-gitlab-history url=...]
[/git-add-css]
```

### Attributes

Each shortcode takes a few attributes, indicating if it's required for public or private repositories:

| Attribute | Action                   | Public repo                   | Private repo                  | Type    | Description                                                                                                   |
|-----------|--------------------------|-------------------------------|-------------------------------|---------|---------------------------------------------------------------------------------------------------------------|
| `url`       | all except `git-add-css` | :ballot_box_with_check:       | :ballot_box_with_check:       | string  | The browser URL of the document, e.g. https://github.com/gis-ops/wordpress-markdown-git/blob/master/README.md |
| `user`      | all except `git-add-css` | :negative_squared_cross_mark: | :ballot_box_with_check:       | string  | The **user name** (not email) of an authorized user                                                           |
| `token`     | all except `git-add-css` | :negative_squared_cross_mark: | :ballot_box_with_check:       | string  | The access token/app password for the authorized user                                                         |
| `limit`     | `history`                | :negative_squared_cross_mark: | :negative_squared_cross_mark: | integer | Limits the history of commits to this number                                                                  |
| `classes`   | `git-add-css`            | :ballot_box_with_check:       | :ballot_box_with_check:       | string  | The additional CSS classes to render the content with                                                         |

#### `Token` authorization

You **need to** authorize via `user` and `token` if you intend to publish from a private repository.

However, keep in mind that some platforms have stricter API limits for anonymous requests which are greatly extended if you provide your credentials. So even for public repos it could make sense.

How to generate the `token` depends on your platform:

- Github: Generate a Personal Access Token following [these instructions](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)
- Bitbucket: Generate a App Password following [these instructions](https://confluence.atlassian.com/bitbucket/app-passwords-828781300.html#Apppasswords-Createanapppassword)
- Gitlab: Generate a Personal Access Token following [these instructions](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#creating-a-personal-access-token)

This plugin needs only **Read access** to your repositories. Keep that in mind when creating an access token.

### Examples

We publish our own tutorials with this plugin: https://gis-ops.com/tutorials/.

#### Publish Markdown from Github

`[git-github-markdown user=nilsnolde token=hxsCL7LpnEp55FH9qK url="https://github.com/gis-ops/tutorials/blob/master/qgis/QGIS_SimplePlugin.md"]`

#### Display last commit and document URL from Bitbucket

`[git-bitbucket-checkout user=nilsnolde token=hxsCL7LpnEp55FH9qK url="https://bitbucket.org/nilsnolde/test-wp-plugin/src/master/README.md"]`

#### Display commit history from Gitlab

`git-gitlab-history limit=5 user=nilsnolde token=hxsCL7LpnEp55FH9qK url="https://gitlab.com/nilsnolde/esy-osm-pbf/-/blob/master/README.md"]`

#### Use additional CSS classes to style

The following example will put a dashed box around the whole post:

```
[git-add-css classes="md-dashedbox"]
    [git-github-checkout url="https://github.com/gis-ops/tutorials/blob/master/qgis/QGIS_SimplePlugin.md"]
    [git-github-markdown url="https://github.com/gis-ops/tutorials/blob/master/qgis/QGIS_SimplePlugin.md"]
    [git-github-history url="https://github.com/gis-ops/tutorials/blob/master/qgis/QGIS_SimplePlugin.md"]
[/git-add-css]
```

With the following CSS file contents enqueued to your theme:

```css
div.md_dashedbox {
    position: relative;
    font-size: 0.75em;
    border: 3px dashed;
    padding: 10px;
    margin-bottom:15px
}

div.md_dashedbox div.markdown-github {
    color:white;
    line-height: 20px;
    padding: 0px 5px;
    position: absolute;
    background-color: #345;
    top: -3px;
    left: -3px;
    text-transform:none;
    font-size:1em;
    font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
}
```

## Support

Add issues at <https://github.com/gis-ops/wordpress-markdown-git/issues>.

== Installation ==
1. Install WP Pusher (https://wppusher.com) via ZIP and activate
2. Install from Github via WP Pusher from gis-ops/wordpress-markdown-git
3. Activate and add shortcode to your posts.

Or directly from WordPress plugin repository.

Or install as ZIP from https://github.com/gis-ops/wordpress-markdown-git/archive/master.zip

== Changelog ==
= v1.0.0 =
* First version
