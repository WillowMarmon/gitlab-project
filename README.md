# Setup an On Prem Dev-Ops server from scratch using git-lab

✅ [Prerequisites]()

💿 [Installation]()

⚙️ [Configuration]()

🌐 [Post-Install]()

## Description

This project consists of the installation and setup of my actual on-prem Dev-Ops
server that I use for open-source project development. It involves the installation,
configuration, and deployment of GitLab using Docker and Docker-Compose.

### Technologies Used

- Docker
- Docker-Compose

### Languages Used

- YAML
-

## Prerequisites

### 1. A server

You can use just about anything for this, one common choice is to use an old
office tower for the purpose of a home server, but in this example we'll be using
my personal server which is a combination of purpose built server hardware and
some more consumer grade elements.

Mine is running nix-os (a Linux distribution), but yours can be running Macos, Linux,
or even openBSD. So long as git-lab is available there's a good chance it'll work.

### 2. Networking

This project is not really about networking, docker networking, or what kinds of
networking you're going to need. If you need more information about configuring your
home-lab network I'd recommend [serve the home]() they have great resources, and
if you're looking for docker networking I'd recommend [this article]() by SOMEGUY.

### 3. An understanding of your operating system and included tools

While I will be walking you through the docker-compose setup, a baseline of
knowledge about your tools is expected. For example, I'll be using a text editor
called vim, but other options like nano, emacs, or notepad would work just as
well just so long as you understand the tools yourself.

## The Installation

### 1. Install Docker and Docker-Compose

On debian-based linux, you can install Docker and Docker-Compose with the
following command:
`$ sudo apt install docker docker-compose`

### 2. create /srvs/stacks git-lab stack

Next we're going to create the directory for our docker-compose stacks to live in.
This is often placed in either /opt/stacks/ or /srvs/stacks/. From here on we'll
be refering to this location as ...stacks/ as there are no specific requirements.
In any example code however this will be listed as /srvs/stacks.

Next within .../stacks, create a dir for the docker-compose stack. Our file tree
should now look like this:

- /
  - srvs
    - stacks
      - git-lab

### 3. Create The docker-compose.yaml File

Next we're going to write our docker-compose.yaml. Go ahead and open
`/srvs/stacks/git-lab/docker-compose.yaml` in your favorite text editor
and add the following:

```yaml
version: "3.6"
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    container_name: git-lab
    restart: unless-stopped
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
    ports:
      - "80:80"
      - "2222:22"
    volumes:
      - "/srvs/stacks/git-lab/config:/etc/gitlab"
      - "/srvs/storage/volumes/git-lab/_data:/var/opt/gitlab"
      - "/srvs/stacks/git-lab/logs:/var/log/gitlab"
    shm_size: "256m"
```

### 4. Start the git-lab stack

In the same directory as docker-compose.yaml, enter the following command to
start the container:
`# docker-compose up -d`
The `-d` is to start it in the background, or "disconnected."

## Configuration

For configuration we have two options. You can either write the configuration
options within the container in /etc/gitlab/gitlab.rb, or you can write them
within the `GITLAB_OMNIBUS_CONFIG` inside the docker-compose file.

### Write the Config Inside the Container

First we get a shell inside the container with:
`# docker exec -it git-lab /bin/bash`
and then edit the config file using nano or vim:
`# vi /etc/gitlab/gitlab.rb`

Now set the `external_url`, smtp settings, and https settings to your preference.

### Write the Config In docker-compose.yaml

My prefered method is to configure containers within docker-compose.yaml, so
lets go through that. First open `docker-compose.yaml`

Now we're going to write the flags to send to gitlab.rb inside our GITLAB_OMNIBUS_CONFIG
variable as follows:

```yaml
GITLAB_OMNIBUS_CONFIG: |
  external_url 'http://gitlab.willowmarmon.com';
  gitlab_rails['smtp_enable'] = false;
```

There are many other settings you can configure in GITLAB_OMNIBUS_CONFIG, and
you can see them all [here](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template)

## Post Installation Configuration

### 1. Sign into your instance as root

Git-lab helpfully sets up a user account for us with the username of root. The password
is stored in `/srvs/stacks/git-lab/config/initial_root_password` for the first 24
hours after the instance is created after which that file is removed.

Go ahead and navigate to the FQDN we specified earlier and sign in with those credentials.

### 2. Change your password

The first thing to do after signing in is to change your password. Select your gravatar
in the upper right corner and select `Edit Profile`.

[img of Edit Profile]()

Next we go to password on the right, and enter the current password followed by our
new password.

Once the change is complete it will ask you to sign back into your gitlab.

### 3. Adjust Profile Settings

Git-lab provides us with some sane default settings, but once you start using the
software they usually aren't enough. Go ahead and head back to `Edit Profile`

[img of Edit Profile]()

Go ahead and set your name and email (even though we haven't set up email on the
server side, it still uses it for avatar detection using [gravatar](https://gravatar.com))

### 4. Change Username

Head over to **Account > Change Username** and replace `root` with your desired
username.

[img of change username]()

### 5. Restrict or Disable Public Sign-Up

By default anyone can request to sign up for an account but must be manually
approved by an administrator.

To change that, navigate to the admin pannel with the button in the botom right.
Then go to `Settings > General` and select Sign-up restrictions.

[img of sign-up restictions]()

Now we can diable Sign-up all together, or restrict it with a confirmation email
and/or email domains (This last option is very useful for orgs who have all their
users on one email domain).

### Adding SSH Keys to Your Account

Head over to **SSH Keys** and click add new key.

[img of SSH Keys Add]()

Then put the public key in the key field, give it a title, and set up it's
authentication type and expiration date.

Then click add key.

###
