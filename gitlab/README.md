[GitLab 공식](https://docs.gitlab.com/ee/install/docker.html#install-gitlab-using-docker-compose)


[Pre-configure Docker container](https://docs.gitlab.com/ee/install/docker.html#pre-configure-docker-container)

1. Install Docker Compose.
2. Create a docker-compose.yml file:
```
version: '3.6'
services:
  web:
    image: 'gitlab/gitlab-ee:latest'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
        # Add any other gitlab.rb configuration here, each on its own line
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    shm_size: '256m'
```
3. Make sure you are in the same directory as docker-compose.yml and start GitLab:
'''
docker compose up -d

> Read the [Pre-configure Docker container](https://docs.gitlab.com/ee/install/docker.html#pre-configure-docker-container) section to see how the GITLAB_OMNIBUS_CONFIG variable works.

Below is another docker-compose.yml example with GitLab running on a custom HTTP and SSH port. Notice how the GITLAB_OMNIBUS_CONFIG variables match the ports section:
```
version: '3.6'
services:
  web:
    image: 'gitlab/gitlab-ee:latest'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.example.com:8929'
        gitlab_rails['gitlab_shell_ssh_port'] = 2224
    ports:
      - '8929:8929'
      - '2224:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    shm_size: '256m'
```