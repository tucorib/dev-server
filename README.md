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

#### Restore backup
In order to restore backup, execute:

```shell
# Copy backup from backup server to docker container
sudo docker cp /mnt/backups/gitlab/XYZ_gitlab_backup.tar gitlab:/var/opt/gitlab/backups/

# Set owner
docker exec -it gitlab chown git.git /var/opt/gitlab/backups/XYZ_gitlab_backup.tar

# Stop services
docker exec -it gitlab gitlab-ctl stop unicorn
docker exec -it gitlab gitlab-ctl stop sidekiq

# Verify
docker exec -it gitlab gitlab-ctl status

# Restore
docker exec -it gitlab gitlab-rake gitlab:backup:restore BACKUP=XYZ

# Restart services
docker exec -it gitlab gitlab-ctl start sidekiq
docker exec -it gitlab gitlab-ctl start unicorn

# Verify
docker exec -it gitlab gitlab-ctl status
```
