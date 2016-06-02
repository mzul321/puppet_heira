# puppet_heira
Everything about Puppet and heira implementation

#What is Puppet?
Puppet is an open-source configuration management tool. It runs on many Unix-like systems as well as on Microsoft Windows, and includes its own declarative language to describe system configuration.

#Who is the creator?
Puppet is produced by Puppet Labs, founded by Luke Kanies in 2005. It is written in Ruby and released as free software under the GNU General Public License (GPL) until version 2.7.0 and the Apache License 2.0 after that.[2]

#What is Heira?
A simple pluggable Hierarchical database generally used in Puppet for retrieving infrastructure information from centra repository.

#Why Heira?
Hierarchical data is a good fit for the representation of infrastructure information. Consider the example of a typical company with 2 datacenters and on-site development, staging etc.

All machines need:

ntp servers
sysadmin contacts
By thinking about the data in a hierarchical manner you can resolve these to the most correct answer easily:

     /------------- DC1 -------------\             /------------- DC2 -------------\
    | ntpserver: ntp1.dc1.example.com |           | ntpserver: ntp1.dc2.example.com |
    | sysadmin: dc1noc@example.com    |           |                                 |
    | classes: users::dc1             |           | classes: users::dc2             |
     \-------------------------------/             \-------------------------------/
                                \                      /
                                  \                  /
                           /------------- COMMON -------------\
                          | ntpserver: 1.pool.ntp.org          |
                          | sysadmin: sysadmin@%{domain}       |
                          | classes: users::common             |
                           \----------------------------------/
In this simple example machines in DC1 and DC2 have their own NTP servers, additionaly DC1 has its own sysadmin contact – perhaps because its a remote DR site – while DC2 and all the other environments would revert to the common contact that would have the machines domain fact expanded into the result.

The classes variable can be searched using the array method which would build up a list of classes to include on a node based on the hierarchy. Machines in DC1 would have the classes users::common and users::dc1.

The other environment like development and staging would all use the public NTP infrastructure.

This is the data model that extlookup() have promoted in Puppet, Hiera has taken this data model and extracted it into a standalone project that is pluggable and have a few refinements over extlookup.
