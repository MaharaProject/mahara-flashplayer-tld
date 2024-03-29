This repository is not maintained any more and only made available for historical purposes.

# tld

List of secondary top level domains (TLD) for subdomain handling in Flowplayer license key
validation.

Flowplayer licenses validate against the main domain name and all its subdomains. Subdomains
therefore must be stripped from the domain name when

- generating a license key
- validating a license key

To avoid treating treating for example ```co.uk``` as subdomain of the ```uk``` the given domain
name must be checked against a list of secondary TLDs.

This list must be maintained and synchronized *manually* as there is no static canonical reference
of the ever growing number of secondary TLDs.

## Filename layout

This repo will contain the following files:

```
secondary
secondary-3.2.16
secondary-5.4.3
```
etc.

- ```tld/secondary``` is the reference for all current Flowplayer builders
- ```tld/secondary-<version>``` is the reference applicable to Flowplayer ```<version>```.

## Building

### Players

At release time the commercial and unlimited players are built using the ```secondary-<version>```
file. During development the latest ```secondary``` (unversioned) can be used for example by issuing
```make VERSION=5.8-dev all```.

Makefile example which reads ```secondary-<version>``` if present or the latest version of
```secondary``` thus ensuring that the latest list is used in case copying it to
```secondary-<releaseversion>``` was forgotten before triggering the release builds:

```make
TLD_DIR = ../tld
TLD=$(shell tld=secondary-$(VERSION) && cd $(TLD_DIR) && \
    test -r $$tld || tld=secondary && \
    uniq $$tld | sort | awk '{ ORS = "," } { print }' | sed 's/,$$//')
```
The above will store all unique secondary TLDs in a sorted comma separated string which can be
inserted where required like so:
```make
key:
        @ sed 's/@TLD/$(TLD)/' somewhere/keycheck >> $(DIST)/key
```

### Account/Admin

Because Flowplayer HTML5 and Flowplayer Flash releases do not happen at the same time, account and
admin builders must parse the latest ```secondary-<version>``` list for each player, for example
```secondary-5.5``` for Flowplayer HTML5 version 5.5 and ```secondary-3.2.17``` for Flowplayer Flash
3.2.17.

For account/adming updates therefore the ```secondary-<releaseversion>``` **must** be present.

## TODO

Historic versions to be added manually as reference for support issues etc.
