# FitFindr

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, every
> command, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> python app.py listings --full -n 6      # read the data (Milestone 1)
> python app.py fields                    # what you can filter on
> python app.py ask 'vintage graphic tee under $30'
> ```
>
> All three tools are stubs, so that last command will do nothing useful yet.
> That's the starting position.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     This is your submission. Fill each section in as you finish the milestone
     it belongs to — don't leave it all to the end.

     Unit 3 asks for the first five sections. Unit 4 adds the five below them.
     Leave the unit 4 sections alone until then; they're here so you know
     what's coming.

     Everything is pasted as TEXT. No screenshots, no images, no video links.
     A typed block of output gets full credit; a picture of the same output
     gets none.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 3 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Three or four sentences: what a user asks for, and what they get back. -->

FitFindr is an AI-powered outfit recommendation tool that helps users discover clothing listings and build outfits around items they find. It searches clothing listings using a description, optional size, and maximum price. When a matching item is found, FitFindr generates an outfit suggestion based on the selected item and the user's wardrobe, then creates a fit-card caption describing the item and its style.  The application uses three tools: `search_listings`, `suggest_outfit`, and `create_fit_card`. The agent stores its progress in a session dictionary so that search results, the selected item, outfit suggestions, and fit cards can be inspected and tested.

## Planning Loop

The planning loop is implemented in `agent.py::run_agent`.

**Branch rule:** If the search returns no matching listings, the agent stores a helpful error message and stops before generating an outfit or fit card. Otherwise, it selects the first matching listing and continues through outfit generation and fit-card creation.

The agent stores each tool's result in the session dictionary before using that result in the next step. This makes the workflow easier to inspect and test.

When a search returns no matches, the agent suggests trying a different size, increasing the maximum price, or using different or broader search terms.

## How I Used AI

I used AI assistance to help implement, troubleshoot, and test the FitFindr tools and planning loop. I reviewed the generated code and ran commands in my local Python environment to check its behavior.

**Moment 1 — Listing search and filtering:** I asked for help implementing `search_listings` so it could find relevant clothing listings and apply size and price filters. I tested a query for a graphic tee under $30 and an intentionally impossible search. I checked that the first query returned matching listings and the impossible query returned an empty list. I also refined the size matching logic to avoid treating similar size strings, such as `S` and `US 9`, as equivalent.

**Moment 2 — Agent branching and session state:** I asked for help connecting the three tools in `agent.py::run_agent` and handling searches with no results. The initial placeholder stopped both test queries before the tools ran. I replaced it with query parsing, session-state updates, and a branch that returns a helpful message when the search results are empty. I then ran the agent again to verify that an unsuccessful search left `session["fit_card"]` as `None` and explained what the user could try next.

I used terminal tests to check the behavior of the implementation and committed and pushed my changes to my GitHub fork.




---

## Tool Inventory

<!-- Four lines per tool. This is worth 2 points and it's the single most
     common place students lose them.

     "Returns a list" earns NOTHING. The description has to say what is IN
     the list.

     The empty case isn't optional either — it's the thing your loop branches
     on, and if you don't decide it here you'll discover it as a crash in
     Milestone 5. -->

      

 1. `search_listings`

- **What it does:** Searches thrift listings for items matching a description, with optional size and maximum-price filters, and ranks matches by keyword overlap.
- **Inputs:** `description` (`str`, required); `size` (`str | None`, optional); `max_price` (`float | None`, optional).
- **Return value:** A list of up to `config.SEARCH_RESULT_LIMIT` listing dictionaries, ordered by relevance. Each dictionary contains `id`, `title`, `description`, `category`, `style_tags` (list), `size`, `condition`, `price` (float), `colors` (list), `brand` (`str | None`), and `platform`.
- **Empty-case behavior:** Returns an empty list (`[]`) when no listings match. Size matching must be case-insensitive and avoid incorrect partial matches, such as matching `"s"` to `"us 9"` or `"l"` to `"xl"`.

 2. `suggest_outfit`

- **What it does:** Uses the language model to suggest one or two outfits featuring a selected thrift item and, when available, items from the user's wardrobe.
- **Inputs:** `new_item` (`dict`, required; a listing dictionary); `wardrobe` (`dict`, required; contains an `items` key with a list of wardrobe items).
- **Return value:** A non-empty string (`str`) containing outfit suggestions, ideally naming specific pieces from the user's wardrobe when available.
- **Empty-case behavior:** If `wardrobe["items"]` is empty, returns general styling advice for the selected item instead of raising an error or returning an empty string.

 3. `create_fit_card`

- **What it does:** Uses the language model to create a social-media caption describing the thrift find and its outfit styling.
- **Inputs:** `outfit` (`str`, required; the outfit suggestion); `new_item` (`dict`, required; the listing dictionary).
- **Return value:** A string (`str`) containing a two-to-four-sentence caption that mentions the item, its price, and its platform once each and describes the outfit's vibe.
- **Empty-case behavior:** If `outfit` is empty or contains only whitespace, returns a descriptive message instead of raising an error.

### `search_listings`

- **What it does:**
- **Inputs:** <!-- name and type each: `max_price` (float), not "a price" -->
- **Returns:**
- **When it has nothing:**

### `suggest_outfit`

- **What it does:**
- **Inputs:**
- **Returns:**
- **When it has nothing:**

### `create_fit_card`

- **What it does:**
- **Inputs:**
- **Returns:**
- **When it has nothing:**

---

## Planning Loop

<!-- Your branch rule, stated as a rule — the condition AND both paths — plus
     the file and function that holds it.

     Like this:
       "If search_listings returns an empty list, put a message in the session
        and stop. Otherwise take the first result and go to suggest_outfit."
        — agent.py::run_agent

     The grader checks your code against what you claim here, so the file and
     function have to be real. -->

**Branch rule:**  If search_listings returns an empty list ([]), set session["error"] to a helpful message explaining how the user can adjust their search, then return the session without calling suggest_outfit or create_fit_card. Otherwise, store the first result in session["selected_item"], call suggest_outfit, and then call create_fit_card.

**Where it lives:** `agent.py::run_agent` 

**How the query is parsed:** Query parsing is not implemented yet. The planned implementation will extract a description, size, and maximum price from the user's natural-language query and store them in session["parsed"]. The parsing method will be documented once selected and implemented.

**What moves through the session:** The user's request, the search results, the selected listing, the outfit suggestions, and the generated fit-card caption, in that order. 
query → parsed → search_results → selected_item → outfit_suggestion → fit_card. The session also stores wardrobe and error.
---

## Sample Run

<!-- Two things go here.

     1. One FULL query and its output, pasted as text.
     2. Your three per-tool terminal tests — the command and what it printed. -->

**One full query**

```
$ python app.py ask '...'

```

**The three tools, tested one at a time**

```
$ python -c "from tools import search_listings; print(search_listings('graphic tee', max_price=30))"

```

```
$ python -c "from tools import suggest_outfit; ..."

```

```
$ python -c "from tools import create_fit_card; ..."

```

---

**One full query** The full query will be documented after the tools are connected through the planning loop in agent.py and the end-to-end query has been tested.

```
$ python app.py ask '...'

```
**The three tools, tested one at a time**

**1. `search_listings` — matching search**

Command:
```powershell
python -c "from tools import search_listings; print(search_listings('graphic tee', max_price=30))"
```

Output: Returned matching listing dictionaries within the $30 price limit.

**Empty search test**

Command:
```powershell
python -c "from tools import search_listings; print(search_listings('zzznomatchitem123'))"
```

Output:
```text
[]
```

**2. `suggest_outfit` — outfit suggestions**

Command:
```powershell
python -c "from tools import suggest_outfit; from utils.data_loader import get_example_wardrobe, load_listings; print(suggest_outfit(load_listings()[0], get_example_wardrobe()))"
```

Output: Returned outfit suggestions for the selected vintage Levi's 501 jeans.

**Empty wardrobe test**

Command:
```powershell
python -c "from tools import suggest_outfit; from utils.data_loader import load_listings; print(suggest_outfit(load_listings()[0], {'items': []}))"
```

Output: Returned general styling advice without an error.

**3. `create_fit_card` — caption generation**

Command:
```powershell
python -c "from tools import create_fit_card; from utils.data_loader import load_listings; print(create_fit_card('jeans and white sneakers', load_listings()[0]))"
```

Output: Generated a social media caption mentioning the vintage Levi's 501 jeans, $38.0 price, Depop platform, and styling vibe.

**Empty outfit test**

Command:
```powershell
python -c "from tools import create_fit_card; from utils.data_loader import load_listings; print(create_fit_card('   ', load_listings()[0]))"
```

Output: Returned a descriptive message containing the item name, price, and platform.

## How I Used AI

<!-- Two specific moments. What you asked, what came back, what you changed.

     "I used Claude to help me code" is not enough.

     "I gave Claude my search_listings spec. It returned None on no match
     instead of an empty list, so I changed it" is the level we want. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

<!-- ═══════════════════════ UNIT 4 — THE TEST ═══════════════════════

     Don't fill these in during unit 3.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Run Log — Before

<!-- Five criteria, five tries each, in this exact format.

     Five, because your criteria are written out of five. Mark each try PASS
     or FAIL, count the passes, and read that count against your target — a
     row targeting 4 of 5 with three PASS cells is MISSED (3/5).

     `python run_eval.py --label before` runs everything and writes the table
     into results/. Paste it here and fill in the verdicts. -->

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |  |  |
| 2.  |  |  |  |  |  |  |  |
| 3.  |  |  |  |  |  |  |  |
| 4.  |  |  |  |  |  |  |  |
| 5.  |  |  |  |  |  |  |  |

**Real output from one try**, pasted as text, naming the file and function
that produced it:

```

```

---

## Verdicts and Diagnoses

<!-- MET or MISSED per criterion against LAST UNIT's target, plus a sentence on
     how you decided.

     Then, for every miss: which of the four places it happened — a tool, the
     loop's branch, the session, or the model's output — AND the mechanism.

     Not a diagnosis:  "The fit card was bad."
     A diagnosis:      "The fit card criterion missed on 2 of 5 items. Both had
                        an empty brand field. My prompt puts the brand in the
                        first sentence, so the card opened with a blank and read
                        like a fragment. The tool worked; the prompt assumed a
                        field that isn't always there."

     Look for a pattern. Three misses on the same tool is one problem, not
     three. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**



---

## Loop Trace

<!-- One full run, printed step by step, with the MCP call visible in it.

     `python app.py ask '...' --trace` once you've added the trace.step()
     calls in Milestone 2.

     Worth pasting BOTH the happy path and the empty-search path. The empty
     one should be visibly shorter, because it stops. If your two traces are
     the same length, your branch isn't working — and this is the fastest way
     anyone will ever find that out. -->

**Happy path**

```

```

**Empty search**

```

```

**On the MCP move:** <!-- what changed in your code, and whether anything
behaved differently afterwards. If the rewire didn't work, say exactly where it
broke — the error text and the last thing that worked. That earns the point in
full. -->



---

## The Improvement

<!-- What you changed, why your diagnosis pointed at it, and the after-run in
     the same table format. One change, measured properly.

     `python run_eval.py --label after` -->

**What I changed:**

**Which failure it was meant to fix:**

### Run Log — After

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |  |  |
| 2.  |  |  |  |  |  |  |  |
| 3.  |  |  |  |  |  |  |  |
| 4.  |  |  |  |  |  |  |  |
| 5.  |  |  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it made things worse, say that. Honestly reported, that earns full
     credit and is more interesting than one that worked. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped where
     you did. "I ran out of time" is fine if it's true. Pretending nothing is
     left is not. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 3

       [ ] criteria.md has five numbered criteria, each with a target
       [ ] Each criterion has a reason underneath it
       [ ] All five unit 3 sections above have real content
       [ ] Tool Inventory: all three tools, inputs WITH TYPES, a specific
           return value, and the empty case
       [ ] Planning Loop names the branch rule and agent.py::run_agent
       [ ] Sample Run: one full query plus the three per-tool tests, as text
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN, you submit the same one
           next unit

     SUBMISSION CHECKLIST — unit 4

       [ ] mcp_server.py exists with one tool registered
           (or a written record of exactly where the rewire broke)
       [ ] Run Log — Before, five criteria, five tries each
       [ ] Real output pasted underneath, naming file and function
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, naming a place AND a mechanism
       [ ] Loop Trace, with the MCP call visible in it
       [ ] All three failure modes triggered and handled
       [ ] One improvement, with Run Log — After in the same format
       [ ] What's Still Broken
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository. Your commit history is what
     shows your criteria existed before your results did.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
