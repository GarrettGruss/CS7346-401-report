# This section uses combined mermaid and drawio diagrams

## Mermaid Diagrams

- Install the mermaid CLI: `docker pull minlag/mermaid-cli`
- Render the files: `docker run --rm -u `id -u`:`id -g` -v .:/data minlag/mermaid-cli -i use_cases.md -o use_cases.png -s 3 -w 1920 -H 1080`