# dictyBase server (http://www.dictybase.org)
## MVC web applications
- [Gene page](http://dictybase.org/gene/sadA)
- [Literature](http://dictybase.org/publication/pubmed/29121537)
- [Fast track curation](http://dictybase.org/tools/curation) 
## Backend application
- Cache manager: http://dictybase.org/cache/gene/sadA   
It will only responds to HTTP `DELETE` request, something like this from a
terminal
```
curl -X DELETE http://dictybase.org/cache/gene/sadA
```
## Administration
### Scenario
Any,all of few of the above applications are down or not responding...   
### Troubleshooting
- Visit the designated url of the application and check it response.
- Email `dictybase@northwester.edu` and ask to explain the issue.   
## Possible action
Restarting the unresponsive application works most of the cases.   
### How to restart applications
Login to dictybase production server via ssh and run the following shell
commands as necessary. Preferably, restart the one that is unresponsive.   
```
cd ~/dictyBase/Apps

cd dictygene/
script/fcgi_backends.pl restart staging.server
cd ~/dictyBase/Apps

cd curation/
script/fcgi_backends.pl restart staging.server
cd ~/dictyBase/Apps


cd cachemanager/
script/fcgi_backends.pl restart staging.server
cd ~/dictyBase/Apps


cd ontopub/
script/fcgi_backends.pl restart staging.server
cd ~/dictyBase/Apps
```

And for a blanket safety restart apache, it’s kind of a all in one solution
generally.
```
sudo /etc/init.d/apache2 reload
```
