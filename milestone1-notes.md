# FitFindr — Milestone 1 Notes

## Listing fields

- `price`: The item's price, stored as a number.
- `colors`: A list of colors associated with the item.
- `brand`: The brand name, or `null` if it is not provided.

Other fields observed include `platform`, `style_tags`, and `notes`.

## Starter project check

- Python environment and dependencies passed the environment check.
- The starter application runs.
- The `ask` command reports that the planning loop in `agent.py` has not been implemented yet, as expected at this stage.

## Search examples

- Vintage graphic tee under $30
- 90s track jacket in size M
- Denim jacket under $50
- Designer ballgown size XXS under $5 (empty-search test)

PowerShell searches use single quotes so dollar amounts are preserved literally.