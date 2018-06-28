### To graceful poweroff
  $ `sudo systemctl poweroff`

### Install git
  $ `sudo yum install -y git`

### To clone the repo
  Go to git under your repo and copy the ssh link
  paste the link in the terminal by running this command: $ `git clone ssh link`

### SSH key-gen
  $ `ssg-keygen -t rsa -b 4096 -C "student44"`
  $ `sudo yum update -y git vim nano tmux tree htop epel-release`

  $ `curl -O ssh link`
### Libcurl fix
  $ `sudo yum install libcurl`
  $ `sudo ln -s /usr/lib64/libcurl.so.4 /usr

### Firewall
  $ 'sudo firewall-cmd --add-port=8008/tcp --permanent'
  $ 'sudo systemctl restart firewall' or 'sudo firewall-cmd --reload'

### View numbers for large config file
  $ escape :set num

### To check kafha
  $ 'echo ruok | nc 172.16.100.100 2181'

esc:%s/bro/suricata/g  (this command is find and replace)
