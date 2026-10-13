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

The user asks for outfits at a thrift shop. The system returns listings that match the user's query. The system also suggests outfits that match the user's wardrobe, if any. Finally, it produces a caption that matches the suggested outfit.

---

## Tool Inventory

<!-- Four lines per tool. This is worth 2 points and it's the single most
     common place students lose them.

     "Returns a list" earns NOTHING. The description has to say what is IN
     the list.

     The empty case isn't optional either — it's the thing your loop branches
     on, and if you don't decide it here you'll discover it as a crash in
     Milestone 5. -->

### `search_listings`

- **What it does:** Search the listings data for items matching a description, and optionally a size and a price ceiling.
- **Inputs:** <!-- name and type each: `max_price` (float), not "a price" --> `description` (str), `size` (str or None), `max_price` (float or None)
- **Returns:** A list of matching listing dictionaries, returning best match first.
- **When it has nothing:** Returns an empty list

### `suggest_outfit`

- **What it does:** Given a thrifted item and the user's wardrobe, it suggests one or two outfits.
- **Inputs:** `new_item` (dict), `wardrobe` (dict)
- **Returns:** A non-empty string with outfit suggestions
- **When it has nothing:** Returns general styling advice rather than raising or returning "".

### `create_fit_card`

- **What it does:** Writes a short caption someone would actually post about the find
- **Inputs:** `outfit` (str), `new_item` (dict)
- **Returns:** A two-to-four sentence caption.
- **When it has nothing:** Returns a descriptive message rather than raising "".

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

**Branch rule:** If search_listings returns an empty list, put a message in the session and stop. Otherwise, take the first result and go to suggest_outfit

**Where it lives:** `agent.py::run_agent`

**How the query is parsed:** I will use regex to extract the description, size, and maximum price from the user's query.

**What moves through the session:** The query is parsed into `description`, `size`, and `max_price`; then `search_results` are stored, the first result becomes `selected_item`, `outfit_suggestion` is stored, and finally `fit_card` is stored.

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
[{'id': 'lst_002', 'title': 'Y2K Baby Tee — Butterfly Print', 'description': 'Super cute early 2000s baby tee with butterfly graphic. Fitted crop length. Tag says medium but fits like a small.', 'category': 'tops', 'style_tags': ['y2k', 'vintage', 'graphic tee', 'cottagecore'], 'size': 'S/M', 'condition': 'excellent', 'price': 18.0, 'colors': ['white', 'pink', 'purple'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_006', 'title': 'Graphic Tee — 2003 Tour Bootleg Style', 'description': 'Vintage-style bootleg tee with faded graphic. Slightly boxy fit. 100% cotton, soft and worn-in.', 'category': 'tops', 'style_tags': ['graphic tee', 'vintage', 'grunge', 'streetwear', 'band tee'], 'size': 'L', 'condition': 'good', 'price': 24.0, 'colors': ['black'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_017', 'title': 'Mesh Long-Sleeve Top — Black', 'description': 'Sheer black mesh long-sleeve. Great for layering under a graphic tee or over a bralette. Stretchy material, fits true to size.', 'category': 'tops', 'style_tags': ['y2k', 'grunge', 'goth', 'layering'], 'size': 'S/M', 'condition': 'excellent', 'price': 15.0, 'colors': ['black'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_033', 'title': 'Vintage Band Tee — Faded Grey', 'description': 'Faded grey band-style tee with distressed graphic. Crew neck. Fits boxy. Well-loved but no holes or major damage.', 'category': 'tops', 'style_tags': ['vintage', 'grunge', 'band tee', 'graphic tee', 'streetwear'], 'size': 'L', 'condition': 'fair', 'price': 19.0, 'colors': ['grey', 'charcoal'], 'brand': None, 'platform': 'depop'}, {'id': 'lst_011', 'title': 'Low-Rise Cargo Pants — Khaki', 'description': 'Y2K era low-rise cargo pants. Lots of pockets. Khaki color, slightly distressed at the hems. Great for layering with a long tee.', 'category': 'bottoms', 'style_tags': ['y2k', 'cargo', '2000s', 'streetwear'], 'size': 'W29', 'condition': 'fair', 'price': 27.0, 'colors': ['khaki', 'tan'], 'brand': None, 'platform': 'poshmark'}, {'id': 'lst_015', 'title': 'Vintage Graphic Hoodie — Faded Black', 'description': 'Faded black pullover hoodie with barely-visible vintage graphic on the chest. Cozy interior. Some pilling but adds to the worn-in look.', 'category': 'tops', 'style_tags': ['vintage', 'grunge', 'graphic', 'streetwear'], 'size': 'L', 'condition': 'fair', 'price': 26.0, 'colors': ['black', 'charcoal'], 'brand': None, 'platform': 'depop'}]

```

```
$ python -c "from tools import suggest_outfit; ..."
python -c "from tools import suggest_outfit; from utils.data_loader import load_listings, get_example_wardrobe; print(suggest_outfit(load_listings()[0], get_example_wardrobe()))"
**Outfit 1: Effortless Everyday Minimalist**
*   **Pieces used:** Vintage Levi's 501 Jeans, White ribbed tank top, Black crossbody bag, Chunky white sneakers.
*   **Why it works:** The fitted white tank creates a clean, balanced contrast against the relaxed, medium-wash denim. Adding the chunky white sneakers and minimal black bag keeps the look grounded, fresh, and effortlessly classic.

**Outfit 2: Streetwear Casual**
*   **Pieces used:** Vintage Levi's 501 Jeans, Oversized grey crewneck sweatshirt, Brown leather belt, Black combat boots.
*   **Why it works:** Tucking the crewneck in with the brown leather belt adds structure to the vintage denim. Pairing it with black combat boots introduces a subtle edge that leans into a cool, grounded streetwear aesthetic

python -c "from tools import suggest_outfit; from utils.data_loader import load_listings, get_empty_wardrobe; print(suggest_outfit(load_listings()[0], get_empty_wardrobe()))"
Since these Vintage Levi's 501s are a classic, mid-wash straight-leg denim, they are extremely versatile. Here is how to style them:

**Color Palette:**
*   **Neutrals:** White, black, grey, and beige/cream create a timeless, effortless look.
*   **Earth Tones:** Olive green, brown, and mustard play well with the vintage blue wash.
*   **Pops of Color:** Red, forest green, or burgundy add a retro or streetwear edge.

**Clothing Categories:**
*   **Tops:** Crisp white t-shirts, oversized graphic tees, cropped baby tees, classic button-downs (blue oxford or flannel), and chunky knit sweaters.
*   **Outerwear:** Leather biker jackets, oversized denim jackets (for a denim-on-denim look), trench coats, or collegiate varsity jackets.

**Shoes & Accessories:**
*   **Shoes:** White leather sneakers, retro runners, black loafers, or classic leather boots (like Chelsea or combat boots).
*   **Accessories:** A black or brown leather belt, silver or gold minimalist jewelry, a baseball cap, and a canvas tote bag or leather crossbody.

```

```
$ python -c "from tools import create_fit_card; ..."
python -c "from tools import create_fit_card; from utils.data_loader import load_listings; print(create_fit_card('Jeans, a white ribbed tank top, and chunky white sneakers', load_listings()[0]))"
Nothing beats the effortless cool of a true vintage pair, and these medium wash Levi's 501s are the ultimate everyday staple. Grab them on Depop for just $38.0 to instantly nail that off-duty model aesthetic. Style them with a crisp white ribbed tank and chunky sneakers for the easiest, most timeless summer fit.

python -c "from tools import create_fit_card; from utils.data_loader import load_listings; print(create_fit_card('   ', load_listings()[0]))"
I cannot create a fit card because no outfit suggestion was provided.

```

---

## How I Used AI

<!-- Two specific moments. What you asked, what came back, what you changed.

     "I used Claude to help me code" is not enough.

     "I gave Claude my search_listings spec. It returned None on no match
     instead of an empty list, so I changed it" is the level we want. -->

**Moment 1**

- _What I asked for:_ I asked it to build search_listings by pasting the lab instructions.
- _What came back:_ The full logic, including all required criteria.
- _What I changed:_ Nothing! I took the time to understand the code.

**Moment 2**

- _What I asked for:_ To fill in run_agent() by pasting the lab instructions.
- _What came back:_ The full code for run_agent().
- _What I changed:_ Nothing! I took the time to understand the code.

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

| Criterion                                            | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5        | Verdict      |
| ---------------------------------------------------- | ------ | ----- | ----- | ----- | ----- | ------------ | ------------ |
| 1. matching query completes                          | FAIL   | FAIL  | PASS  | PASS  | PASS  | PASS         | MISSED (3/5) |
| 2. impossible query stops early                      | PASS   | PASS  | PASS  | PASS  | PASS  | MET (5/5)    |
| empty wardrobe _(diagnostic — not one of your five)_ | FAIL   | FAIL  | FAIL  | FAIL  | FAIL  | MISSED (1/5) |
| 3. right item passed to suggest_outfit               | FAIL   | PASS  | PASS  | PASS  | PASS  | MISSED (4/5) |
| 4. fit card names selected item                      | FAIL   | PASS  | PASS  | PASS  | FAIL  | MISSED (3/5) |
| 5. fit card has complete sentences                   | PASS   | FAIL  | PASS  | PASS  | PASS  | MISSED (4/5) |

**Real output from one try**, pasted as text, naming the file and function
that produced it:

```
**Criterion 1**
```

stopped early: no

- selected_item: Y2K Baby Tee — Butterfly Print ($18.0, depop)
- search_results: 10

Outfit suggestion:

```
Here are two complete outfit ideas using your new Y2K baby tee:

**Outfit 1: High-Contrast Streetwear**
*   **Top:** Y2K Baby Tee (New item)
*   **Bottoms:** Baggy straight-leg jeans, dark wash
*   **Footwear:** Chunky white sneakers
*   **Outerwear:** Black cropped zip hoodie
*   **Accessories:** Black crossbody bag
*   **Why it works:** This creates the classic Y2K proportion play—the ultra-fitted baby tee balances out the voluminous baggy jeans. Layering the black cropped hoodie over top adds texture and edge, while the chunky white sneakers tie the whole streetwear aesthetic together.

**Outfit 2: Casual Retro-Earth**
*   **Top:** Y2K Baby Tee (New item)
*   **Bottoms:** Wide-leg khaki trousers
*   **Footwear:** Black combat boots
*   **Accessories:** Brown leather belt, Black crossbody bag
*   **Why it works:** The pink and purple butterfly print pops against the neutral khaki trousers for an effortless, vintage-casual look. Tucking in the fitted tee and cinching it with the belt defines your waist against the wide-leg cut, and the black combat boots ground the softer cottagecore/Y2K elements with a touch of grunge.
```

Fit card:

```
🦋 Oh, to be a Y2K pop princess in this pink and purple butterfly baby tee! Score this ultimate vintage throwback on my Depop for just $18. It’s giving total early 2000s nostalgia, perfect for styling with baggy denim or retro trousers. ✨
```

Trace:

```
[1] search_listings (via MCP)
      in:  dict with keys: description, size, max_price
      out: 10 items: Y2K Baby Tee — Butterfly Print, Graphic Tee — 2003 Tour Bootleg Style, Vintage Band Tee — Faded Grey … +7 more
[2] suggest_outfit
      in:  dict with keys: new_item, wardrobe
      out: Here are two complete outfit ideas using your new Y2K baby tee:  **Outfit 1: High-Contrast Streetwear** *   **…
[3] create_fit_card
      in:  dict with keys: outfit, new_item
      out: 🦋 Oh, to be a Y2K pop princess in this pink and purple butterfly baby tee! Score this ultimate vintage throwba…
```

```
**Criterion 2**
- stopped early: yes — No matching listings were found. Try changing the description, size, or maximum price.
- selected_item: (none)
- search_results: 0

Trace:

```

[1] search_listings (via MCP)
in: dict with keys: description, size, max_price
out: [] (empty)

```

```

**Criterion 3**

- stopped early: no
- selected_item: Mesh Long-Sleeve Top — Black ($15.0, depop)
- search_results: 9

Outfit suggestion:

```
Here are two complete outfits featuring the Mesh Long-Sleeve Top:

### Outfit 1: 90s Grunge Streetwear
* **Key pieces used:** Mesh Long-Sleeve Top, White ribbed tank top, Baggy straight-leg jeans (dark wash), Black combat boots, Black crossbody bag.
* **Why it works:** Layering the sheer mesh top over the fitted white ribbed tank creates a high-contrast, textured look that leans into the top's Y2K/grunge roots. Pairing it with relaxed, dark wash baggy jeans and heavy combat boots balances the fitted top with volume and edge.

### Outfit 2: Contrast & Textures (Edgy Minimal)
* **Key pieces used:** Mesh Long-Sleeve Top, Black cropped zip hoodie, Wide-leg khaki trousers, Chunky white sneakers, Black crossbody bag.
* **Why it works:** This look uses the mesh top as a sleek, textured base layered underneath the black cropped zip hoodie for a cool streetwear contrast. Grounding the dark, fitted top half with relaxed khaki trousers and chunky white sneakers keeps the outfit modern, breathable, and wearable.
```

Fit card:

```
The ultimate 90s grunge layering piece just landed in the shop! Grab this versatile black mesh long-sleeve top on my Depop for just $15. It’s giving major Y2K edgy minimal vibes—throw it on over a white tank or under a zip hoodie and you're good to go. 🖤✨
```

Trace:

```
[1] search_listings (via MCP)
      in:  dict with keys: description, size, max_price
      out: 9 items: Mesh Long-Sleeve Top — Black, Y2K Baby Tee — Butterfly Print, Oversized Flannel Shirt — Plaid Red/Black … +6 more
[2] suggest_outfit
      in:  dict with keys: new_item, wardrobe
      out: Here are two complete outfits featuring the Mesh Long-Sleeve Top:  ### Outfit 1: 90s Grunge Streetwear * **Key…
[3] create_fit_card
      in:  dict with keys: outfit, new_item
      out: The ultimate 90s grunge layering piece just landed in the shop! Grab this versatile black mesh long-sleeve top…
```

```
**Criterion 4**
 stopped early: no
- selected_item: Y2K Baby Tee — Butterfly Print ($18.0, depop)
- search_results: 10

Outfit suggestion:

```

**Outfit 1: Y2K Streetwear Contrast**

- **Pieces used:** Baggy dark wash straight-leg jeans, black cropped zip hoodie, chunky white sneakers, black crossbody bag.
- **Why it works:** The fitted, graphic nature of the baby tee balances the oversized proportions of the baggy jeans and zip hoodie. It leans fully into the Y2K aesthetic while keeping the color palette grounded with black, white, and dark blue.

**Outfit 2: Casual Vintage Mix**

- **Pieces used:** Wide-leg khaki trousers, vintage black denim jacket, chunky white sneakers, brown leather belt.
- **Why it works:** The butterfly print adds a playful, nostalgic pop to the neutral earth tones of the khakis and brown belt. Layering the vintage black denim jacket on top ties the look together with a cool, effortless edge.

```

Fit card:

```

Channel your inner early 2000s pop star with this adorable white, pink, and purple butterfly print baby tee. Priced at just $18.00, it's the ultimate nostalgia piece for mastering that effortless Y2K streetwear vibe. Grab it now exclusively on Depop before it’s gone! 🦋✨

```

Trace:

```

[1] search_listings (via MCP)
in: dict with keys: description, size, max_price
out: 10 items: Y2K Baby Tee — Butterfly Print, Graphic Tee — 2003 Tour Bootleg Style, Vintage Band Tee — Faded Grey … +7 more
[2] suggest_outfit
in: dict with keys: new_item, wardrobe
out: **Outfit 1: Y2K Streetwear Contrast** \* **Pieces used:** Baggy dark wash straight-leg jeans, black cropped zip…
[3] create_fit_card
in: dict with keys: outfit, new_item
out: Channel your inner early 2000s pop star with this adorable white, pink, and purple butterfly print baby tee. P…

```
**Criterion 5**

```

stopped early: no

- selected_item: Y2K Baby Tee — Butterfly Print ($18.0, depop)
- search_results: 10

Outfit suggestion:

```
Here are two complete outfit ideas using your new Y2K Butterfly Baby Tee and pieces from your existing wardrobe:

### Outfit 1: Streetwear Contrast
* **Top:** Y2K Baby Tee — Butterfly Print (New)
* **Bottoms:** Baggy straight-leg jeans (dark wash)
* **Outerwear:** Black cropped zip hoodie
* **Shoes:** Chunky white sneakers
* **Accessories:** Black crossbody bag

**Why it works:** This outfit plays on the classic Y2K proportion contrast: a tight, cropped baby tee paired with ultra-baggy bottoms. Layering the black cropped zip hoodie on top keeps the silhouette balanced while leaning into your streetwear aesthetic. The chunky white sneakers tie the white in the graphic tee together for a cohesive look.

### Outfit 2: Casual Vintage Mix
* **Top:** Y2K Baby Tee — Butterfly Print (New)
* **Bottoms:** Wide-leg khaki trousers
* **Outerwear:** Vintage black denim jacket
* **Shoes:** Black combat boots
* **Accessories:** Brown leather belt, Black crossbody bag

**Why it works:** This combination bridges the gap between Y2K and minimal earth tones. Tucking the fitted baby tee into the wide-leg khakis and cinching it with the brown leather belt defines your waist. Throwing on the vintage black denim jacket and combat boots adds an edgy, grounded contrast to the playful, pastel butterfly print.
```

Fit card:

```
Butterflies are officially back, and this Y2K butterfly baby tee is the ultimate nostalgic thrift score. Grab it on Depop for just $18.00 to instantly channel that effortless early-2000s streetwear vibe. Pair it with baggy denim or wide-leg trousers for the ultimate fitted-meets-baggy proportion play! ✨🦋
```

Trace:

```
[1] search_listings (via MCP)
      in:  dict with keys: description, size, max_price
      out: 10 items: Y2K Baby Tee — Butterfly Print, Graphic Tee — 2003 Tour Bootleg Style, Vintage Band Tee — Faded Grey … +7 more
[2] suggest_outfit
      in:  dict with keys: new_item, wardrobe
      out: Here are two complete outfit ideas using your new Y2K Butterfly Baby Tee and pieces from your existing wardrob…
[3] create_fit_card
      in:  dict with keys: outfit, new_item
      out: Butterflies are officially back, and this Y2K butterfly baby tee is the ultimate nostalgic thrift score. Grab …
```

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

| #   | Criterion                                            | Target | Verdict      | How I decided                                                                                                                          |
| --- | ---------------------------------------------------- | ------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | A matching query completes all three tools           | 4/5    | MISSED (3/5) | Tries 1 and 2 crashed with a temporary model-service 503; tries 3, 4, and 5 completed with fit cards.                                  |
| 2   | An impossible query stops before the second tool     | 5/5    | MET (5/5)    | All five tries returned an empty result and stopped before `suggest_outfit`.                                                           |
| 3   | The selected item reaches `suggest_outfit` unchanged | 5/5    | MISSED (4/5) | Four tries completed and showed the selected item flowing to the next tool; one try crashed because the model service was unavailable. |
| 4   | The fit card names the selected item                 | 5/5    | MISSED (3/5) | Three completed fit cards named the selected item; two tries crashed with a temporary model-service 503.                               |
| 5   | The fit card has at least two complete sentences     | 5/5    | MISSED (4/5) | Four completed fit cards contained at least two sentences; one try crashed before producing a card.                                    |

**Diagnoses**

**Criterion 1 — matching query completes: MISSED (3/5).** The failure occurred
in the model-backed tools, not in the search or branching logic. Tries 1 and 2
received a `503 UNAVAILABLE` response from the model service, so the agent
crashed before it could finish all three tool calls. The other three tries
completed and returned fit cards. The immediate improvement is to retry the
evaluation when the model service is available and keep the original run as
evidence of the external availability failure.

**Criterion 2 — impossible query stops early: MET (5/5).** The loop branch
worked correctly. Each search returned an empty list, the session received a
message explaining that the description, size, or maximum price could change,
and `suggest_outfit` was not called. This was a successful branch behavior,
not a tool or model failure.

**Criterion 3 — selected item reaches `suggest_outfit`: MISSED (4/5).** The
four completed tries passed the selected listing into the next tool, but one
try failed at the model-service boundary with a `503 UNAVAILABLE` error before
the complete workflow could be observed. The session and loop were working in
the completed tries; the miss was caused by model availability rather than by
changing the selected item.

**Criterion 4 — fit card names the selected item: MISSED (3/5).** Three tries
produced fit cards that identified the selected listing, while two tries
crashed when the model service returned `503 UNAVAILABLE`. The failure was in
the model-backed `create_fit_card` path, so no caption was available to check
on those attempts.

**Criterion 5 — fit card has complete sentences: MISSED (4/5).** Four tries
returned fit cards containing at least two complete sentences. One try failed
before producing a card because the model service was unavailable. This was a
model-output availability failure, not a sentence-count failure in a returned
caption.

---

## Loop Trace

<!-- One full run, printed step by step, with the MCP call visible in it.

     `python app.py ask '...' --trace` once you've added the trace.step()
     calls in Milestone 2.

     Worth pasting BOTH the happy path and the empty-search path. The empty one should be visibly shorter, because it stops. If your two traces are the same length, your branch isn't working — and this is the fastest way anyone will ever find that out. -->

**Happy path**

```
python app.py ask 'vintage graphic tee under $30' --trace
[1] search_listings (via MCP)
      in:  dict with keys: description, size, max_price
      out: 10 items: Y2K Baby Tee — Butterfly Print, Graphic Tee — 2003 Tour Bootleg Style, Vintage Band Tee — Faded Grey … +7 more
[2] suggest_outfit
      in:  dict with keys: new_item, wardrobe
      out: **Outfit 1: Y2K Streetwear Contrast** * **Pieces used:** Baggy straight-leg dark wash jeans, black cropped zip…
[3] create_fit_card
      in:  dict with keys: outfit, new_item
      out: Fluttering straight out of 2004, this white Y2K baby tee featuring a pastel pink and purple butterfly print is…

  Found:    Y2K Baby Tee — Butterfly Print — $18.0 on depop

  Outfit:   **Outfit 1: Y2K Streetwear Contrast**
* **Pieces used:** Baggy straight-leg dark wash jeans, black cropped zip hoodie, chunky white sneakers, black crossbody bag.
* **Why it works:** The fitted, feminine baby tee contrasts perfectly with the heavy, masculine proportions of the baggy dark wash jeans. Layering the black cropped zip hoodie on top keeps you warm while framing the graphic, and chunky white sneakers tie the retro Y2K aesthetic together.

**Outfit 2: Casual Earth-Tone Mix**
* **Pieces used:** Wide-leg khaki trousers, brown leather belt, chunky white sneakers, black crossbody bag.
* **Why it works:** Pairing the pastel pink and purple butterfly tee with khaki trousers bridges the gap between Y2K and cottagecore/earthy styles. Adding the brown leather belt pulls the tan tones together, while white sneakers keep the overall look fresh, effortless, and grounded.

  Fit card: Fluttering straight out of 2004, this white Y2K baby tee featuring a pastel pink and purple butterfly print is the ultimate nostalgic staple. Snagged this beauty for just $18.0 over on Depop, and it’s giving major effortless mall-rat energy. Style it with baggy denim for instant street cred or wide-leg khakis for an earthy twist! 🦋✨

0 model calls this session, 2 served from cache
```

**Empty Search**

```
python app.py ask 'designer ballgown size XXS under $5' --trace
[1] search_listings (via MCP)
      in:  dict with keys: description, size, max_price
      out: [] (empty)

  No matching listings were found. Try changing the description, size, or maximum price.

0 model calls this session
```

**Empty wardrobe**

```
$ python app.py ask 'vintage graphic tee under $30' --empty-wardrobe
(running with an empty wardrobe)

Found:    Y2K Baby Tee — Butterfly Print — $18.0 on depop

  Outfit:   Here is how to style the **Y2K Baby Tee — Butterfly Print**:

*   **Complementary Colors:** Stick to pastels like baby blue, mint green, and lilac to match the pink and purple tones, or use black and denim to ground the bright print.
*   **Clothing Categories:** Pair the fitted baby tee with contrasting silhouettes like baggy cargo pants, low-rise denim, pleated mini skirts, or flared leggings.
*   **Footwear:** Platform sneakers, chunky loafers, or strappy sandals.
*   **Accessories:** Butterfly hair clips, baguette shoulder bags, chunky resin rings, and beaded necklaces.

  Fit card: Obsessed with this Y2K baby tee with the cutest butterfly print! ✨ Grab it on Depop for just $18.00 to instantly channel total early 2000s pop-star energy. Style it with low-rise denim and platform sneakers for the ultimate nostalgic fit.

2 model calls this session, 319 prompt + 191 output tokens

```

**Empty search**

```
$ python app.py ask 'designer ballgown size XXS under $5' --trace

No matching listings were found. Try changing the description, size, or maximum price.
```

**Model Unavailable**

```
$ python app.py ask 'black jacket under $50'
1 model calls this session

ModelUnavailable: The model rejected your API key. Check GEMINI_API_KEY in your .env file, or create a fresh key at aistudio.google.com.
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
| --------- | ------ | ----- | ----- | ----- | ----- | ----- | ------- |
| 1.        |        |       |       |       |       |       |         |
| 2.        |        |       |       |       |       |       |         |
| 3.        |        |       |       |       |       |       |         |
| 4.        |        |       |       |       |       |       |         |
| 5.        |        |       |       |       |       |       |         |

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
