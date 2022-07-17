# About

A set of resources around grype


https://github.com/anchore/grype/issues/724#issuecomment-1139563814
https://github.com/anchore/grype#using-templates
https://github.com/anchore/grype/releases/tag/v0.42.0
DEV.to

Sprig Function Documentation : http://masterminds.github.io/sprig/
GO template https://pkg.go.dev/text/template#hdr-Functions

https://github.com/anchore/grype/issues/826

# Prerequsites

- `git`
- `brew` installed
- `python3` and `pip`

# Install steps


## :one: grype

```
brew tap anchore/grype
brew install grype
```

## :two: Reporting tools


```
python3 -m pip install termgraph
```

# :rocket: Getting started

## Aggregated report in the terminal

```shell
clear
export IMAGE=nginx:latest # optnc/api-transitaires
echo "☝️ About to analyze $IMAGE with grype ❕"
grype $IMAGE -o template -t tmpl/csv-vulnerability_id-severity-no-headers.tmpl > analysis.csv
cat analysis.csv
echo ""
echo "✅ grype analysis done."
echo "$(tail -n +2 analysis.csv)" > analysis.csv
echo "➕ Aggregating datas :"
awk -F, '{a[$1]+=$2;}END{for(i in a)print i", "a[i];}' analysis.csv > analysis-aggregated.csv
cat analysis-aggregated.csv
echo "📊 Charting analysis"
termgraph  analysis-aggregated.csv --title "🛡️  Grype report for [${IMAGE}] 🐳"
# Visit https://github.com/opt-nc/grype-tools/ for more tools around reporting and templates
```

## Html report

```
clear
export IMAGE=optnc/api-transitaires
echo "☝️ About to analyze $IMAGE with grype ❕"
grype $IMAGE -o template -t tmpl/html-table.tmpl > analysis.html
firefox analysis.html
```

## Jupyter NoteBook


# TODOS


## Enhanced html report
- html report -> add href to cves
- replace severity by a css badge
- replace state with a css badge
- replace type by css badge
- style name,installed columns a code style
- link packages to https://pkgs.org/about/

## toolchain

- Package as a Makefile
- Develop markdown template and implement pandoc toolchain for various exports
- JupyterBook report template based on raw csv export

## Ideas

- Concatenate multiple reports within a same csv for advanced JupterNotebook and other reporting tools (OpenSearch, ELK, PowerBI,...)
- Jupter NoteBook sur le json
- HTML sur le json