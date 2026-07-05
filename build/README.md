# PDF build

The distributable PDFs are **not committed** here — they are distributed exclusively via [Gumroad](https://0xshugo.gumroad.com/l/agent-security-checklist).

To regenerate them from the READMEs:

```sh
pandoc -f gfm -t html5 --standalone --embed-resources \
  --css=build/checklist-pdf.css \
  --metadata title="AI Agent Security Self-Assessment Checklist" \
  README.md -o /tmp/checklist-en.html

pandoc -f gfm -t html5 --standalone --embed-resources \
  --css=build/checklist-pdf.css \
  --metadata title="AIエージェントセキュリティ・セルフアセスメントチェックリスト" \
  --metadata lang=ja \
  README.ja.md -o /tmp/checklist-ja.html

"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf=AI-Agent-Security-Checklist-EN.pdf /tmp/checklist-en.html
# (same for JA)
```
