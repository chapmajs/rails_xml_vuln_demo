Rails XML Vulnerability Demo
============================

This project was used along with a recent install of Metasploit to demonstrate the practical application of [CVE-2013-0156](http://cvedetails.com/cve/2013-0156/) against an unpatched Rails 3.2.10 application for the January 2013 [Tech Valley Ruby Brigade](http://www.techvalleyrb.org/). It is broken into three main commits, detailed below. For the vulnerable application server, you require git, a recent RVM and the ability to build MRI/Rails/SQLite3, along with a compatible JRE and the ability to build JRuby if you wish to run against JRuby

MRI 1.9.3 / Rails 3.2.10
------------------------

A `git checkout 79e452a8` will get you a brand-new empty Rails project ready to run. After setting up your RVM environment, do a `bundle install` and `rails s` to bring up the project in a ready-to-be-exploited state.

JRuby 1.6.6 / Rails 3.2.10
--------------------------

A `git checkout 5eb825c2` will switch the Ruby to JRuby 1.6.6. The Rails version remains the same, but the SQLite3 adapter has to be changed to provide JRuby with a JDBC connection. Do a `bundle install` and `rails s` again to bring the application up. While Rails 3.2.10 is still vulnerable, you will find that the default shellcode included with Metasploit will get injected, but will not properly drop shell for you. **THIS IS NOT A SECURE OPTION TO UPGRADING RAILS!** At best, running with a vulnerable version of Rails with XML params enabled under JRuby is security through obscurity...which isn't security.

MRI 1.9.3 / Rails 3.2.11
------------------------

A `git checkout master` should bring you up to the final commit, which switches back to MRI 1.9.3. A `bundle install` will bring the project up to Rails 3.2.11. Running the Rails server and attempting to run the Metasploit exploit against it should result in a `Hash::DisallowedType` error for the YAML contained within Metasploit's XML params.