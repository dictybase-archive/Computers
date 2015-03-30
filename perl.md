PERL ON DICTY
===

* [Perlbrew](#perlbrew)
* [Perl Packages](#packages)
* [Modware-Loader development](#modware)

<a name="perlbrew"/>
# Perlbrew

Absolutely avoid using Perl from the Mac OS X system. Instead, use `perlbrew` to manage Perl versions and libraries. 

Steps to install and use it (based on the [cpan site](http://search.cpan.org/~gugod/App-perlbrew-0.67/lib/App/perlbrew.pm))

```
	* curl -kL http://install.perlbrew.pl | bash
	* perlbrew init
	* perlbrew available
	* perlbrew install perl-5.19.11 (**it took a while**)
	* perlbrew list (check what is installed)
	* perlbrew switch perl-5.12.2 (Switch perl in the $PATH) + perl -v
    * perlbrew use perl-5.8.1 (Temporarily use another version only in current shell) + perl -v
    * perlbrew off (Turn it off completely. Useful when you messed up too deep. Or want to go back to the system Perl)
    * perlbrew switch perl-5.12.2
    
    Perl version installed: 
    - perl-5.18.4
  	- perl-5.19.11
  	- etc
```

* `cpanm`: Once `perlbrew` is installed, is absolutely essential to install `cpanm` running `perlbrew install-cpanm`. Use it to install libraries 

Now, to install `cpanm libraries` there exist two possibilities:

1. In your `home` available to any Perl project. Then just run `cpanm <package>`

2. Create specific libraries for every project. This is done with `local::lib` which is the package that allows the possibility of managing library packages for a particular project. But the way to install and use `local::lib` is through `cpanm`. **Follow these steps to make sure you are doing the right thing**:

* First, make sure that `cpanm` is installed on the system (if you do, `brew remove cpanm`)
* `perl -V` tells you which perl version and libraries you are using.
* Create the project folder (`ejemplo`) && cd
* Create the Perl library associated to that Perl project: 
	```
	perlbrew lib create ejemplo. 
	```
The message `lib 'perl-5.19.11@ejemplo' is created.` is prompted, which specify the Perl version under which you are installing the library.

* ***IN EVERY SHELL*** the library for that particular project MUST BE specified executing `perlbrew use perl-5.19.11@ejemplo`. Whatever Perl library you know install using `cpanm` will be available only in this shell and in the Perl scripts that you might run there. But first, make sure that you are truly using that Perl library by doing `perl -V` and paying attention to the following line:

	```
	@INC:
	/Users/djt469/.perlbrew/libs/perl-5.19.11@ejemplo/lib/perl5
	/Users/djt469/perl5/lib/perl5    
	```

* Testing: Install a package and test that it works:

```
cpanm Acme::Time::Baby
perl -MAcme::Time::Baby -E 'say babytime'
```

In conclusion: `perlbrew` helps you managing the libraries for every particular project and according to some particular perl version. It helps managing `local::lib` for you. Otherwise, you would need to do almost everything by hand and the libraries would have to be installed in a folder `lib` in your `project` directory. 
	
<a name="packages"/>
# Perl Packages
Use `cpanm`, but associated to every perl version that you are managing with `perlbrew`, as explained above. Anyway, it is better to install the packages locally in your home directory. 

Some useful perl packages that you can install using `cpanm <package>`

* `Devel::Cover`
* `PPI::HTML, Test::Differences, Parallel::Iterator, JSON::XS`required by `Devel::Cover` (check the manual)
* `XML::Simple` to handle xml files
* `Data::Dumper` to dump date (for example, date in the xml file)
* `DBIx::Class::Schema::Loader` 
* `Sqitch` is a database change management application. Use it when planning to do changes in the database to keep track
		- cpanm App::Sqitch DBD::Pg
		
* Perl Object Oriented related
	- `Moose`
	- `Moose::Manual`

<a name="modware"/>
# Modware-Loader development

`Modware-loader` is based on `MooseX::App:Cmd` to provide command line applications to import and export biological data from Chado database. `MooseX::App:Cmd` combines `MooseX::Getopt` and `App::Cmd`. 

## Tutorials & Documentation

* `App::Cmd::Tutorial` --> getting started with `App::Cmd`.

## Installations

* Install different versions of Perl. The lowest version I could install is perl-5.16.3
* Modware Installation:
    * Developers
        * Create library `perl-5.18.4@modwareDeveloper`
        * cpanm modules:
```
cpanm -n Devel::Loaded
cpanm -n DBI
cpanm -n DBD::Oracle DBD::Pg
cpanm git://github.com/dictyBase/Test-Chado.git
cpanm -n Test::File Test::Moose Test::RooT
```
    * Standalone
        - Create library `perl-5.18.4@modware-standalone`
```
cpanm -n Modware-Loader-v1.6.1.tar.gz
cpanm -n XML::LibXML
cpanm -n XML::LibXSLT
cpanm -n DBD::Oracle 
```
