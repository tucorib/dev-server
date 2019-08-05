# dev-server

## Backups

Mount the backups folder:

```shell
sudo mount <user>@<host>:/backups /mnt/backups
```

## Gitlab

### Backups

#### Backups setup
By default, backups are done into the mounted folder /mnt/backups/gitlab.
This folder must exists with git user own:

```shell
sudo mkdir /mnt/backups/gitlab
sudo chown 998 /mnt/backups/gitlab
```

#### CRON backups
In order to have daily backups with CRON, run:

```shell
sudo crontab -e

# Add the following line to file:
0 2 * * * docker exec -it gitlab gitlab-rake gitlab:backup:create CRON=1
```
