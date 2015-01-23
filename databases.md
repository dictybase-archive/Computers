DATABASES
=======

# Tools

## Oracle SQL Developer
It required to create an account to be able to download the app.
- Instant `Client for Mac OS X (Intel x86) Version 11.2.0.3.0 (64-bit)`:
	- Client Package Basic: All files required to run OCI, OCCI, and JDBC-OCI applications 
	- Download `instantclient-basic-macos.x64-11.2.0.3.0.zip` (62,342,264 bytes)
## Postgres93 
It comes from `postgres.app`. It is a PostgreSQL server running on the mac:

Modify the file `.bash_profile` with the following line:

	PATH="/Applications/Postgres93.app/Contents/MacOS/bin:$PATH”

and now I can run the commands for PostgrlSQL from the command line.

## Pgadmim (GUI)
PostgreSQL administration and management tools

## SchemaSpy
[SchemaSpy](http://schemaspy.sourceforge.net/) analyzes database metadata to reverse engineer dynamic Entity Relationship (ER) diagrams. I installed `schemaSpy_5.0.0.jar`. It also requires the drivers of the database. In this case, I also have available in the same directory: `postgresql-9.3-1101.jdbc3.jar`. Check `README` about how to run it in the local folder `/bin/schemaSpy5/` directory 

A tool to analyze database schemas. Steps:
- It requires `brew install Graphviz`
- Run it like this: 

    java -jar schemaSpy_5.0.0.jar -t pgsql -host localhost:5432 -db music_djm -s public -u djt469 -o foldername -dp postgresql-9.3-1101.jdbc3.jar

## Perl driver DBD::Oracle on Mac OS X

I need to first install the 64 bits instant client for Mac, which traditionally has been very problematic. After a lot of difficulties, I made it work. And these are the steps adapted from [here](http://blog.caseylucas.com/tag/oracle-sqlplus/) (the sh script is the essential part) and specially [here](http://blog.g14n.info/2013/07/how-to-install-dbdoracle.html). Since I combined both, I am going to rewrite the steps:

Folder: `$HOME:/opt/Oracle/packages/` where I [downloaded](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html):

```
ls opt/Oracle/packages/
instantclient-basic-macos.x64-11.2.0.3.0.zip   
instantclient-sdk-macos.x64-11.2.0.3.0.zip     
instantclient-sqlplus-macos.x64-11.2.0.3.0.zip
```

Next unzip them:

```
$ cd $HOME/opt/Oracle
$ unzip packages/basic-10.2.0.5.0-linux-x64.zip
$ unzip packages/sdk-10.2.0.5.0-linux-x64.zip
$ unzip packages/sqlplus-10.2.0.5.0-linux-x64.zip
```

Then, go to `$HOME` and create a `.oracle_profile` file with the environment variables 

```
more .oracle_profile
export ORACLE_BASE=$HOME/opt/Oracle
export ORACLE_HOME=$ORACLE_BASE/instantclient_11_2
export PATH=$ORACLE_HOME:$PATH
export TNS_ADMIN=$HOME/etc
export NLS_LANG=AMERICAN_AMERICA.WE8ISO8859P15
export LD_LIBRARY_PATH=$ORACLE_HOME
export DYLD_LIBRARY_PATH=$ORACLE_HOME
```

...which has to be source from `.bash_profile`. At this point, the test `sqlplus /nolog` should give errors. To solve the problem, I cd to the folder `/opt/Oracle/instantclient_11_2` and run the script `changeOracleLibs.sh` (it should be available in this github project, folder `/bin`).

After running the script, testing sqlplus should work:

```
$ sqlplus /nolog

SQLPlus: Release 11.2.0.3.0 Production on Fri Mar 21 13:49:34 2014

Copyright (c) 1982, 2012, Oracle.  All rights reserved.

SQL>

```

Finally, install the DBI module ``cpanm PERL::DBI``, which was installed WITH SUCCESS!!

The testing script ``connect2oracle.pl`` was tested to connect to the Oracle database at the VM on nubic with SUCCESS!

**As consequence of following this step, now it is possible to develop and execute Perl DBI scripts from a Mac OS X (64 bits).**

# Installing Chado on PostgreSQL 

I basically followed the steps explained in the [GMOD chado tutorial](http://gmod.org/wiki/Chado_Tutorial#Practice).

- `MAKER` (portable genome annotation pipeline) 

`brew install maker`, which required the additional modules:

```
- cpanm IO::Prompt
- cpanm IO::Prompt
- cpanm help
- cpanm forks::shared
- cpanm forks::shared
- cpanm forks::shared
- cpanm File::Which
- cpanm Bio::Perl
- cpanm PerlIO::gzip
- cpanm Bit::Vector
- cpanm Perl::Unsafe::Signals
- cpanm Inline
- cpanm DBI
- cpanm DBD::SQLite
- cpanm DBD::Oracle
- brew install gcc
```
But after all this LONG waiting, I finally got this output:
     
```
brew install maker
==> Downloading http://yandell.topaz.genetics.utah.edu/maker_downloads/static/maker-2.31.4.tgz 
Already downloaded: /Library/Caches/Homebrew/maker-2.31.4.tgz
==> yes "" |perl Build.PL
==> ./Build install
MISSING MAKER PREREQUISITES - CANNOT CONTINUE!!
Building MAKER
READ THIS: https://github.com/Homebrew/homebrew/wiki/troubleshooting
If reporting this issue please do so at (not Homebrew/homebrew):
  https://github.com/homebrew/homebrew-science/issues
```

Therefore, I downloaded **MAKER** from the [official website](http://www.yandell-lab.org/software/maker.html) and installed it on `~/local/maker/bin/`

- Download [CHADO](http://sourceforge.net/projects/gmod/files/gmod/) and follow installation instructions:

```
- Install all the required perl modules: `cpanm install Bundle::GMOD`

! Installing the dependencies failed: Module 'HTML::TreeBuilder' is not installed
! Bailing out the installation for Bundle-GMOD-1.0.
34 distributions installed

- Install CHADO. Basically, cd into chado-1.23/ and

perl Makefile.PL GMOD_ROOT=/usr/local/gmod CHADO_DB_NAME=david_chado
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
Makefile written.  Now you should do the following, in order:
1. make              (creates necessary build files)
2. sudo make install (creates $GMOD_ROOT and subdirectories)
3. make load_schema (loads SQL schema into database)
```

And the database `david_chado` was created, with the following [list of tables](chadoTables.md).

I also created the testing database `dicty_chado` adding the [schema available on github](https://github.com/dictyBase/Test-Chado/blob/develop/share/chado.pg).
	

### Other [GMOD PROJECTS](http://gmod.org/wiki/Downloads):

