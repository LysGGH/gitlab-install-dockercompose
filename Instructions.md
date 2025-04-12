# How To Install GitLab Using Docker Compose

---
## Prerequisites

1.	Docker Installed: Ensure Docker and Docker Compose are installed on your system.
- Install Docker.
- Install Docker Compose.

2.	System Requirements:
- Minimum 4 CPU cores and 4 GB of RAM for GitLab.

---
## Steps to Install GitLab Using Docker Compose

1.	Create a docker-compose.yml File:

- Navigate to a directory where you want to set up GitLab and create a docker-compose.yml file:
```bash
mkdir gitlab && cd gitlab
nano docker-compose.yml
```
- Add the following content:
```yaml
version: '3.8'

services:
  gitlab:
    image: gitlab/gitlab-ee:latest
    container_name: gitlab
    hostname: 'gitlab.example.com'
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - './data:/var/opt/gitlab'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.example.com'
        gitlab_rails['lfs_enabled'] = true
```

2.	Configure External URL:
- Replace gitlab.example.com in the hostname and external_url with your server’s domain or IP address.

3.	Start GitLab with Docker Compose:

Run the following command to start GitLab:
```bash
docker-compose up -d
```

4.	Check the Logs:

To ensure GitLab is starting correctly, check the logs:
```bash
docker-compose logs -f gitlab
```

5.	Access GitLab:
- Open a browser and go to http://<your-domain-or-ip>.
- If you haven’t configured DNS, access it via your server’s IP address.

6.	Set Up Admin Password:
- Run the command to reset the root password:
```bash
docker exec -it gitlab /bin/bash
gitlab-rails console
```
- In the console, enter:
```bash
user = User.find_by_username('root')
user.password = 'YourNewPassword'
user.password_confirmation = 'YourNewPassword'
user.save!
exit  # To exit the GitLab console
exit  # To exit the Docker container
```

7.	Persist Data:
- GitLab stores configuration, logs, and data in the config, logs, and data directories you mounted as volumes. Please make sure these are backed up if needed.

8.	Optional - SSL Configuration:
- For HTTPS, modify the external_url to use https and configure SSL certificates.

---
## Managing GitLab with Docker Compose
- Stop GitLab:
```bash
docker-compose down
```

- Restart GitLab:
```bash
docker-compose restart
```

Your GitLab instance is now set up and running using Docker Compose!
