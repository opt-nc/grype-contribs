# ❔ About

Here you will find `jq` tricks to perform some reports around the `-o json` option... hence making this even easier
than the use of templates.

## List vulnerabilities as  `csv`

```
 -o json | jq -r '.matches | .[] | .vulnerability | [.id, .severity, .namespace, .dataSource] | @csv'
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
# -o json | jq -r '[.matches[].vulnerability | {severity}] | group_by(.severity) | [.[] | {severity: .[0].severity, count: . | length}]| to_entries as $row |  ( ( map(keys_unsorted ) | add | unique ) as $cols | ( [$cols] | flatten) ,  ( $row | .[] as $onerow | $onerow |( [ ( $cols |   map ($onerow.value[.] as $v | $v )  ) ]| flatten ) ) ) | @csv '
```

Output sample :

```csv
"count","severity"
8,"Critical"
26,"High"
10,"Low"
24,"Medium"
82,"Negligible"
4,"Unknown"
```
