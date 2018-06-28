point sensor to laptop repo
`sudo vi /etc/yum.repos.d/perched.repo`

```

[base-local]
name=base-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/base

[copr-rocknsm-2.1-local]
name=copr-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/copr-rocknsm-2.1

[epel-local]
name=epel-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/epel

[noarch-local]
name=noarch-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/noarch

[updates-local]
name=updates-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/updates

[elasticsearch-6.x-local]
name=elasticsearch-6.x-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/elasticsearch-6.x

[extras-local]
name=extras-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/extras

[rpmforge-local]
name=rpmforge-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/rpmforge

[rocknsm_2_1-local]
name=rocknsm_2_1-local
enabled=1
gpgcheck=0
baseurl=http://192.168.2.208:8008/rocknsm_2_1

```
