# Multigenome server (http://genomes.dictybase.org)
## MVC web applications
- [Gene page](http://genomes.dictybase.org/purpureum/gene/DPU_G0075240)
- [Literature](http://genomes.dictybase.org/publication/33)
- [Blast tool](http://genomes.dictybase.org/tools/blast)
- [Id redirector](http://genomes.dictybase.org/id/DDB_G0293536) : This
  technically sends a HTTP 302(redirect) and does not render any page of its
  own.   
## Administration
### Scenario
Any,all of few of the above applications are down or not responding...   
### Troubleshooting
- Visit the designated url of the application and check it response.
- Email `dictybase@northwester.edu` and ask to explain the issue.   
## Possible action
Restarting the unresponsive application works most of the cases.   
### How to restart applications
Login to multigenome production server via ssh and run the following shell
commands as necessary. Preferably, restart the one that is unresponsive.   
```
sudo svc -t /service/Genomepage
sudo svc -t /service/resolver
sudo svc -t /service/dictytools 
sudo svc -t /service/ontopub-multi
```
These applications are managed by
[daemontools](http://cr.yp.to/daemontools.html) and here is an easy
[guide](https://isotope11.com/blog/manage-your-services-with-daemontools)
explaining various commands. The above commands (-t) sends a `TERM` signal
and restarts the application. If that does not work try the `-d` and `-u`
version. The `HUP` (-h) works but for some unknown reason it generates bunch of
zombies which has to be killed manually. 


And for a blanket safety restart apache, it’s kind of a all in one solution
generally.
```
sudo service httpd restart
```

*Note:* Restarting the VM also restarts all the applications.

## Application logs
The are located inside the deployed application folder.
```
cd /home/dictybase/webapps/{application_name}/log/
less production.log
```

So for `dictytools` application it will be
`/home/dictybase/webapps/dictytools/log/production.log`

### Extra attention
It’s the `dictytools` application(called blast application) that is now a days
going down on a weekly basis. The backend host is `dicty-blast` and which has
to be reached by dual ssh through dictybase production server. A ping monitor
has been installed for this backend.
