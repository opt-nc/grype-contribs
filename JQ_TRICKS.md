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

Below the query to get the count of of each `severity :

### CSV output

```shell
-o json | jq -r '[.matches[].vulnerability | {severity}] | group_by(.severity) | [.[] | {severity: .[0].severity, count: . | length}]|(.[0] | keys_unsorted) as $keys | ([$keys] + map([.[ $keys[] ]])) [] | @csv'
```

Output sample :

```csv
"severity","count"
"Critical",8
"High",26
"Low",10
"Medium",24
"Negligible",82
"Unknown",4
```
### Tabular output
```shell
-o json | jq -r '[.matches[].vulnerability | {severity}] | group_by(.severity) | [.[] | {severity: .[0].severity, count: . | length}]|(.[0] | keys_unsorted) as $keys | ([$keys] + map([.[ $keys[] ]])) [] | @tsv '
```
Output sample :

```
severity	count
Critical	8
High	26
Low	10
Medium	24
Negligible	82
Unknown	4
```
