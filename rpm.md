## rpm
---

### Query

    -q    query an installed package
    -qp   query a local package

    rpm -q --changelog package               # review the package changelog
    rpm -q --whatprovides /path/to/file      # what package provides this file
    rpm -qf /path/to/file                    # what package provides this file
    rpm -q --filesbypkg package              # list files of installed package
    rpm -q --list package
    rpm -qc --list package                   # list the config files of package
    rpm -qd --list package                   # list the documentation files of package
    rpm -q --provides package                # list what this package provides
    rpm -q --requires package                # list what this package requires
    rpm -q --scripts package                 # list scripts in package

### Keys

#### Import

    rpm --import LOCAL-RPM-GPG-KEY
    rpm --import http[s]://host.domain/RPM-GPG-KEY

#### Delete 

    rpm --erase gpg-pubkey-VERSION-RELEASE

#### List

#!/bin/bash

    echo "INSTALLTIME             NAME-VERSION-RELEASE         SUMMARY"
    echo "-----------             --------------------         -------"

    rpm -q gpg-pubkey --qf '%{INSTALLTIME\t%{NAME}-%{VERSION}-%{RELEASE}\t%{SUMMARY}\n' |
    while read epoch key remainder; do
      printf "%s %s %s\t%s\t" $(date -u --date="@$epoch" +'%F %T %z') $key
      echo $remainder
    done

### Verify

    rpm -Va                                  # verify all installed packages
    rpm -Vf /path/to/file                    # verify package that provided file
    rpm -Vp package                          # verify installed files against original package
    rpm -Vg group                            # verify packages belonging to group
    rpm -K | --checksig  package             # verify package before install

### rpmbuild

    rpm --eval 'macro'                       # evaluate an rpm macro

### sites

    http://ftp.rpm.org/max-rpm/
    https://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/
    https://rpm-software-management.github.io/rpm/manual/macros.html

