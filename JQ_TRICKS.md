# ❔ About

Here you will find `jq` tricks to perform some reports around the `-o json` option... hence making this even easier
than the use of templates.

## List vulnerabilities as  `csv`

```
grype nginx:latest -o json | jq -r '.matches | .[] | .vulnerability | [.id, .severity, .namespace, .dataSource] | @csv'
```

Ouput sample : 

```csv
"CVE-2004-0971","Negligible","debian:distro:debian:11","https://security-tracker.debian.org/tracker/CVE-2004-0971"
"CVE-2004-0971","Negligible","debian:distro:debian:11","https://security-tracker.debian.org/tracker/CVE-2004-0971"
"CVE-2004-0971","Negligible","debian:distro:debian:11","https://security-tracker.debian.org/tracker/CVE-2004-0971"
"CVE-2004-0971","Negligible","debian:distro:debian:11","https://security-tracker.debian.org/tracker/CVE-2004-0971"
"CVE-2005-2541","Negligible","debian:distro:debian:11","https://security-tracker.debian.org/tracker/CVE-2005-2541"
```

## Group vulnerabilities by `severity`

Below the query to get the count of of each `severity`, see [dedicated issue](https://github.com/opt-nc/grype-contribs/issues/8) :

```
# TODO
# grype nginx:latest -o json | ... | @csv'
```

Output sample :

```csv
"Negligible",0
"Low",4
"High",10
```
