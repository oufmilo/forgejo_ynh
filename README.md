<!--
N.B.: This README was automatically generated by https://github.com/YunoHost/apps/tree/master/tools/README-generator
It shall NOT be edited by hand.
-->

# Forgejo for YunoHost

[![Integration level](https://dash.yunohost.org/integration/forgejo.svg)](https://dash.yunohost.org/appci/app/forgejo) ![Working status](https://ci-apps.yunohost.org/ci/badges/forgejo.status.svg) ![Maintenance status](https://ci-apps.yunohost.org/ci/badges/forgejo.maintain.svg)  
[![Install Forgejo with YunoHost](https://install-app.yunohost.org/install-with-yunohost.svg)](https://install-app.yunohost.org/?app=forgejo)

*[Lire ce readme en français.](./README_fr.md)*

> *This package allows you to install Forgejo quickly and simply on a YunoHost server.
If you don't have YunoHost, please consult [the guide](https://yunohost.org/#/install) to learn how to install it.*

## Overview

Forgejo is a Gitea fork (which is a Gogs fork). A self-hosted Git service written in Go. Alternative to GitHub / Gitlab

### Features

- User dashboard, user profile and activity timeline.
- User, organization and repository management.
- Repository and organization webhooks, including Slack, Discord and Dingtalk.
- Repository Git hooks, deploy keys and Git LFS.
- Repository issues, pull requests, wiki, protected branches and collaboration.
- Migrate and mirror repositories with wiki from other code hosts.
- Web editor for quick editing repository files and wiki.
- Jupyter Notebook and PDF rendering.
- Authentication via SMTP, LDAP.
- Customize HTML templates, static files and many others.


**Shipped version:** 1.18.0-1~ynh1

## Screenshots

![Screenshot of Forgejo](./doc/screenshots/screenshot.png)

## Disclaimers / important information

## Additional informations

### Notes on SSH usage

If you want to use Forgejo with SSH and be able to pull/push with your SSH key, your SSH daemon must be properly configured to use private/public keys. Here is a sample configuration `/etc/ssh/sshd_config` that works with Forgejo:

```bash
PubkeyAuthentication yes
ChallengeResponseAuthentication no
PasswordAuthentication no
```

You must also add your public key to your Forgejo profile.

When using SSH on any port other than 22, you need to add these lines to your SSH configuration `~/.ssh/config`:

```bash
Host domain.tld
    port 2222 # change this with the port you use
```

## Private Mode

Actually it's possible to access to the Git repositories by the `git` command over HTTP also in private mode installation. It's important to know that in this mode the repository could be ALSO getted if you don't set the repository as private in the repos settings.

### Upgrade

From command line:

`yunohost app upgrade forgejo`

### Backup

This application now uses the core-only feature of the backup. To keep the integrity of the data and to have a better guarantee of the restoration it is recommended to proceed as follows:

- Stop Forgejo service with this command:

`systemctl stop forgejo.service`

- Launch Forgejo backup with this command:

`yunohost backup create --app forgejo`

- Backup your data with your specific strategy (could be with rsync, borg backup or just cp). The data is generally stored in `/home/yunohost.app/forgejo`.
- Restart Forgejo service with theses command:

`systemctl start forgejo.service`

### Remove

Due of the backup core only feature the data directory in `/home/yunohost.app/forgejo` **is not removed**. It must be manually deleted to purge user data from the app.

### LFS setup
To use a repository with an `LFS` setup, you need to activate it on `/opt/forgejo/custom/conf/app.ini`

```ini
[server]
LFS_START_SERVER = true
LFS_HTTP_AUTH_EXPIRY = 20m
```
By default, NGINX is configured with a maximum value for uploading files at 200 MB. It's possible to change this value on `/etc/nginx/conf.d/my.domain.tld.d/forgejo.conf`.
```
client_max_body_size 200M;
```
Don't forget to restart Forgejo `sudo systemctl restart forgejo.service`.

> These settings are restored to the default configuration when updating Forgejo. Remember to restore your configuration after all updates.

### Git command access with HTTPS

If you want to use the Git command (like `git clone`, `git pull`, `git push`), you need to set this app as **public**.


## Documentation and resources

* Official app website: <https://forgejo.org>
* Official admin documentation: <https://docs.gitea.io/>
* Upstream app code repository: <https://codeberg.org/forgejo/forgejo>
* YunoHost documentation for this app: <https://yunohost.org/app_forgejo>
* Report a bug: <https://github.com/YunoHost-Apps/forgejo_ynh/issues>

## Developer info

Please send your pull request to the [testing branch](https://github.com/YunoHost-Apps/forgejo_ynh/tree/testing).

To try the testing branch, please proceed like that.

``` bash
sudo yunohost app install https://github.com/YunoHost-Apps/forgejo_ynh/tree/testing --debug
or
sudo yunohost app upgrade forgejo -u https://github.com/YunoHost-Apps/forgejo_ynh/tree/testing --debug
```

**More info regarding app packaging:** <https://yunohost.org/packaging_apps>
