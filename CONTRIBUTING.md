# Contributing

Contributions that improve scientific accuracy, usability, or robustness are welcome.

## Evidence-backed corrections

If a claim in any command is inaccurate, overstated, or outdated:

1. Open an issue describing the claim, what the evidence actually shows, and your source
2. Cite a peer-reviewed primary source (systematic review or RCT preferred)
3. PRs that add or change empirical claims without a citation will not be merged

Source hierarchy (prefer top): systematic reviews / meta-analyses > RCTs > consensus position stands (ACSM, ISSN, AASM) > primary observational studies > narrative reviews. Vendor documentation is treated as product definition, not validated physiology.

## What belongs in these commands

- Tool calls and output templates — yes
- Heuristics presented as such — yes (labeled `# heuristic` inline)
- Scientifically well-supported protocols — yes
- Vendor product definitions presented as validated physiology — no
- Specific supplement doses or medical protocols — no

## Style

- Preserve command mechanics exactly: MCP tool names, file path logic, YAML output templates
- Change content, not scaffolding
- Add an inline `# [Strong/Moderate/Weak] — [short rationale]` note when adding any new empirical claim
- No fabricated citations — "no good evidence found" is a valid and useful result
