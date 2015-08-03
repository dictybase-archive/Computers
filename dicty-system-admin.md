dicty-SYSTEM ADMINISTRATION
===

- [Servers](#servers)
- [Databases](#db)
- [Maintenance](#maintenance)
    + [Updates](#updates)
- [Most Frequent Problems](#mfp)
    + [Error: Software caused connection abort](#connectionabort)
    + [Error: deleting cache in Curation Central](#deletingcache)

---

##### Current system architecture 

<img src="https://github.com/dictyBase/presentation/blob/feature/archive/talks/images/dictyBase_architecture/after_mg_release.png" width="600">

---

<a name="servers"/>
# Servers

## dictyBase server (http://www.dictybase.org)

#### MVC web applications

- Gene page: http://dictybase.org/gene/gene_id or name
- Gene page cache: http://dictybase.org/gene/cache/gene_id or name
- Literature: http://dictybase.org/publication/id 
- Ontology: http://dictybase.org/gene/id/go/goid
- Fast track curation: http://dictybase.org/tools/curation 

#### Legacy CGI scripts

- Search tool
- Stock center tool
    - Old curation tool
    - Old converter tools
- Phenotype curation tool 
- EST/contig/chromosome pages 
- Colleague pages 

#### Stock center autofill backend
Go to project folder

```
$_> cd $HOME/dictyBase/Apps/stockcenter
```

Delete the existing pid(only in case of server restart)

```
$_> rm tmp/pids/unicorn.pid
```

Restart the app. Make sure you do not run it from a screen session, it tends to mess around with some env variables and the app refuses to restart
```
$_> RAILS_ENV=production script/unicorn_up
```



## Multigenome server (http://genomes.dictybase.org)

#### MVC web applications

Gene page: http://genomes.dictybase.org/gene/common_name/gene_id
Literature: http://genomes.dictybase.org/publication/id 
Blast tool: http://genomes.dictybase.org/tools/blast 
Id redirector: http://genomes.dictybase.org/id/any_dicty_id 

# Databases

## Oracle
- dictyDB
- MultigenomeDB

## PostgreSQL


# Restart server/apps

#### Servers
- Last resort: restart apache

* `testdb`:

#### Apps & CGI

On `~/dictyBase/Apps`:

```shell
cd dictygene/
script/fcgi_backends.pl restart staging.server
```

```shell
cd curation/
script/fcgi_backends.pl restart staging.server
```

```
cd Apps/dictygene/
script/fcgi_backends.pl restart production.server
tail -f production.log

etc
```

* Cron jobs

```
sudo /etc/init.d/cron status
sudo /etc/init.d/cron start
```


# Updates

### Update the systems:

```shell
sudo yum upgrade
sudo shutdown -r now
```

##### Issue updating packages on grows

A likely problem might appear while installing the new packages due to some sort of problem with the gpg-keys. In such case, it needs to be downloaded from [here](https://www.atomicorp.com/RPM-GPG-KEY.atomicorp.txt) and save them in the folder `/etc/pki/rpm-gpg/`. 
The files with ***the info*** about the repos are in `/etc/yum.respos.d/`, where the file `atomic.repo` had to be edit with this info, as follows...:

```shell
gpgkey = file:///etc/pki/rpm-gpg/RPM-GPG-KEY.art.txt
    file:///etc/pki/rpm-gpg/RPM-GPG-KEY.atomicorp.txt <---- add this line two times in the folder
```

<a name="data"/>
# Data & Scripts location in dictyBase

### Old scripts and libraries

Both prod and test:

```shell
/dicty/lib/dicty/S
/dicty/patches
```

<a name="mfp"/>
# Most Frequent Problems

<a name="connectionabort"/>
### Error: Software caused connection abort

```
[Fri Mar 06 08:27:37 2015] [error] Software caused connection abort at /usr/local/dicty/www_dictybase/db/cgi-bin/search/results.pl line 55.\n
[Fri Mar 06 08:27:40 2015] [error] Software caused connection abort at /usr/local/dicty/www_dictybase/db/cgi-bin/search/search.pl line 66.\n
```

###### Analysis (by Sidd):

Okay by looking at the log up and down around the error you reported, it
looks like a timeout, which is very much possible to happen in our
low resource production server. It could be perl cgi script waiting for a
result output timeout because the resources are busy(cpu/memory/swap
space). The other one could be a rogue bot making tons of HTTP calls to
our resources and hoging the connections.
So, not having a apache restart in this case was a good thing because
there was not database disconnection reported in the log. Otherwise, it
might have aborted bunch of running writable transaction(means only
having half of curation if it is running and getting kicked out in middle) that might cause data corruption which is very hard to detect and solve.

---
<a name="deletingcache"/>
### Error deleting cache

The option `Gene page cache` on `[Curator Central](http://dictybase.org/db/cgi-bin/dictyBase/curatorLogin)` allows to `delete cache`, and therefore, being able to visualize the new changes on the gene page.

***Problem***. `Delete chache` outputs the following error (embedded in html code):

```
Server error!

The server encountered an internal error and was unable to complete your request. Either the server is overloaded or there was an error in a CGI script.

Error 500
```

###### Server `/var/log/apache2/error.log` information:

```shell
[Wed Apr 08 11:21:13 2015] [error] [client IP] (111)Connection refused: FastCGI: failed to connect to server "/tmp/cachemanager.fcgi": connect() failed, referer: http://dictybase.org/db/cgi-bin/dictyBase/curatorLogin?user=CGM_CHADO
[Wed Apr 08 11:21:13 2015] [error] [client IP] FastCGI: incomplete headers (0 bytes) received from server "/tmp/cachemanager.fcgi", referer: http://dictybase.org/db/cgi-bin/dictyBase/curatorLogin?user=CGM_CHADO
```
<<<<<<< Updated upstream
tail -n 6000 error.log |  less
```
=======

###### Apps/curation/log/development.log information

```shell
Wed Apr  8 09:46:49 2015 debug Curation::Utils:291 [6249]: 500 Internal Server Error
Wed Apr  8 09:46:49 2015 debug Mojolicious::Plugin::RequestTimer:34 [6249]: 200 OK (1.384215s, 0.722/s).
Wed Apr  8 09:46:51 2015 debug Mojolicious:187 [6248]: GET /curation/reference/14193 (Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:37.0) Gecko/20100101 Firefox/37.0).
Wed Apr  8 09:46:51 2015 debug Mojolicious::Routes:406 [6248]: Dispatching Curation::Controller::Usersession->validate.
Wed Apr  8 09:46:51 2015 debug Mojolicious::Routes:406 [6248]: Dispatching Curation::Controller::Reference->show.
Wed Apr  8 09:46:51 2015 debug Mojolicious::Plugin::RequestTimer:34 [6248]: 200 OK (0.327978s, 3.049/s).
```

###### Diagnosis

It's a `500 Internal Server Error` probably as consequence of the power outrage that we experienced recently. 

###### Solucion

Restart the cache app:

```shell
cd dictyBase/Apps/cachemanager/
script/fcgi_backends.pl restart production.server
```

As a result, `Delete cache` now works!
>>>>>>> Stashed changes
