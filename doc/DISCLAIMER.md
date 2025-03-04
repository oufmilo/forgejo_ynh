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

