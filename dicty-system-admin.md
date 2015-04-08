dicty-SYSTEM ADMINISTRATION
===

- [Servers](#servers)
- [Databases](#db)
- [Maintenance](#maintenance)
    + [Updates](#updates)
- [Most Frequent Problems](#mfp)

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
    /usr/local/dicty/lib/dicty/S
    /usr/local/dicty/patches


<a name="mfp"/>
# Most Frequent Problems
   
### Error: Software caused connection abort

```
[Fri Mar 06 08:27:37 2015] [error] Software caused connection abort at /usr/local/dicty/www_dictybase/db/cgi-bin/search/results.pl line 55.\n
[Fri Mar 06 08:27:40 2015] [error] Software caused connection abort at /usr/local/dicty/www_dictybase/db/cgi-bin/search/search.pl line 66.\n
```

### Analysis (by Sidd):

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


### Useful Commands:

```
tail -n 6000 error.log |  less
```