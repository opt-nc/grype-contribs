# ❔ About

The aim of this repo is to summarize some resources around [Grype](https://github.com/anchore/grype)
to take the best ouf this great tool.

[![asciicast](https://asciinema.org/a/RoLhz0Ehe0sp74wA1NNipB0PH.svg)](https://asciinema.org/a/RoLhz0Ehe0sp74wA1NNipB0PH)

# 🧰 Prerequisites

For an optimal usage of these resources, you'll need :

- `git`
- [`brew`](https://brew.sh/) installed
- `python3` and `pip`

##  Install tools

```
brew tap anchore/grype
brew install grype
```

We'll use [`termgraph`](https://github.com/mkaz/termgraph),  _"A command-line tool that draws basic graphs in the terminal,"_ :

```
python3 -m pip install termgraph
```

Finally clone this repo : 

```
gh repo clone opt-nc/grype-contribs
cd grype-contribs
```

👉 You are ready.


## 📜 Templating

Since [`v0.42.0`](https://github.com/anchore/grype/releases/tag/v0.42.0), and
its issue [`#724`](https://github.com/anchore/grype/issues/724#issuecomment-1139563814)
it is possible to transform analysis report with [templates](https://github.com/anchore/grype#using-templates).

This feature makes it possible to build nicely useable and highly customizable reports.


### 📊  Aggregated report in the terminal (`termgraph`)

```shell
clear
 # Put your image here
export IMAGE=nginx:latest
echo "☝️ About to analyze $IMAGE with grype ❕"
grype $IMAGE -o template -t tmpl/csv-vulnerability_id-severity-no-headers.tmpl > work/analysis.csv
cat work/analysis.csv
echo ""
echo "✅ grype analysis done."
echo "$(tail -n +2 work/analysis.csv)" > work/analysis.csv
echo "➕ Aggregating datas :"
awk -F, '{a[$1]+=$2;}END{for(i in a)print i", "a[i];}' work/analysis.csv > work/analysis-aggregated.csv
cat work/analysis-aggregated.csv
echo "📊 Charting analysis"
termgraph  work/analysis-aggregated.csv --title "🛡️  Grype report for [${IMAGE}] 🐳"
# Visit https://github.com/opt-nc/grype-tools/ for more tools around reporting and templates
```

### 🔗 Html report

```
clear
export IMAGE=nginx:latest
echo "☝️ About to analyze $IMAGE with grype ❕"
grype $IMAGE -o template -t tmpl/html-table.tmpl > work/analysis.html
firefox work/analysis.html
```

### Jupyter NoteBook

- `TODO`

# TODOS


## Enhanced html report

- html report -> add href to cves
- replace severity by a css badge
- replace state with a css badge
- replace type by css badge
- style name,installed columns a code style
- link packages to https://pkgs.org/about/


## 💡 Ideas

- Concatenate multiple reports within a same csv for advanced JupterNotebook and other reporting tools (OpenSearch, ELK, PowerBI,...)
- Jupter NoteBooks on `json`
- Nicer HTML reports
- Package as a Makefile
- Develop markdown template and implement pandoc toolchain for various exports
- JupyterBook report template based on raw csv export
