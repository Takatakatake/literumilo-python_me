# Literumilo - Comprehensive Spell Checker and Morphological Analyzer for Esperanto

Literumilo is an advanced spell checker and morphological analyzer designed specifically for Esperanto, the international auxiliary language. It validates Esperanto word spelling and decomposes words into their constituent morphemes (prefixes, roots, suffixes, and grammatical endings), providing deep linguistic analysis.

## Overview

Literumilo exploits the highly regular and agglutinative morphology of Esperanto to perform sophisticated word analysis. The system uses a recursive backtracking algorithm combined with a comprehensive morpheme database to break down even the most complex compound words into their meaningful components.

### What Makes Literumilo Powerful

Esperanto's systematic word formation allows for creating complex compound words by combining morphemes according to well-defined rules. Literumilo implements these rules to:

1. **Validate spelling** by checking if morpheme combinations are grammatically correct
2. **Decompose words** into morphemes, revealing their internal structure
3. **Understand meaning** through semantic category analysis (118 meaning types)
4. **Check grammar** by validating prefix-suffix compatibility, verb transitivity, and part-of-speech constraints

### Example Analyses

```
miskomprenita → mis.kompren.it.a
  mis-     : prefix (wrongly, incorrectly)
  kompren  : verb root (understand) [transitive]
  -it-     : passive past participle suffix
  -a       : adjective ending
  meaning  : "misunderstood"

ĉirkaŭiris → ĉirkaŭ.ir.is
  ĉirkaŭ   : prepositional prefix (around, surrounding)
  ir       : verb root (go, walk)
  -is      : past tense ending
  meaning  : "went around, circled"

gepatroj → ge.patr.oj
  ge-      : prefix (both sexes)
  patr     : noun root (parent) [kinship term]
  -oj      : plural nominative ending
  meaning  : "parents"

fingromontri → fingr.o.montr.i
  fingr    : noun root (finger)
  -o-      : separator vowel (aids pronunciation)
  montr    : verb root (show, point)
  -i       : infinitive ending
  meaning  : "to point (with finger)"

neforgesebla → ne.forges.ebl.a
  ne-      : negative prefix (not)
  forges   : verb root (forget) [transitive]
  -ebl-    : possibility suffix (able to be)
  -a       : adjective ending
  meaning  : "unforgettable, not able to be forgotten"
```

## Key Features

### Core Capabilities

- **Recursive Morphological Analysis**: Breaks down compound words using intelligent backtracking (supports up to 9 morphemes per word)
- **Dictionary-Based Validation**: Uses a 10,695-entry morpheme database (10,543 unique morphemes loaded) with detailed grammatical metadata
- **Synthesis Rule Checking**: Validates morpheme combinations according to Esperanto's word formation rules
- **Context-Aware Validation**: Checks compatibility based on:
  - **Part of Speech**: 19 types (noun, verb, adjective, adverb, prefix, suffix, participle, etc.)
  - **Semantic Meaning**: 118 categories (person, animal, place, profession, kinship, ethnicity, etc.)
  - **Verb Transitivity**: Transitive, intransitive, or both
  - **Synthesis Type**: Prefix, suffix, participle, unlimited/limited combinability, no combination
  - **Capitalization**: Minuscule, majuscule, all-caps

### Input/Output Flexibility

- **Flexible Input Formats**:
  - Unicode accents: ĉ, ĝ, ĥ, ĵ, ŝ, ŭ
  - X-system notation: cx, gx, hx, jx, sx, ux (automatically converted)
- **Two Analysis Modes**:
  - **Morpheme mode** (mode=True): Returns text with words divided into morphemes
  - **Spell-check mode** (mode=False): Returns list of unknown/invalid words
- **Multiple Processing Levels**:
  - Single word analysis (`check_word`)
  - String analysis (`analyze_string`)
  - File analysis (`analyze_file`)

### Advanced Features

- **Separator Vowel Recognition**: Handles pronunciation-aiding vowels inserted between morphemes
  - Example: `fingr.o.montr.i` (easier) vs `fingr.montr.i` (harder to pronounce)
  - Supports 'o', 'a', or 'e' as separator (maximum one per word)
- **Exception Handling**: Special processing for edge cases
  - Accusative pronouns: ĝin→ĝi.n, vin→vi.n, lin→li.n, min→mi.n, sin→si.n
  - Possessive forms: lian→li.an, cian→ci.an
- **Capital Letter Preservation**: Maintains original capitalization in analysis results
  - `RIĈULO` → `RIĈ.UL.O` (not `riĉ.ul.o`)
- **Abbreviation Support**: Recognizes hyphenated abbreviations
  - `n-rojn` → `n-r.ojn` (numerojn = numbers)

## Requirements

- Python 3.7 or later

## Installation

Install via pip:

```bash
python3 -m pip install literumilo
```

## Quick Start

```python
from literumilo import check_word

# Check a single word
result = check_word("miskomprenita")
print(f"Word: {result.word}")      # Output: mis.kompren.it.a
print(f"Valid: {result.valid}")    # Output: True

# Check an invalid word
result = check_word("miskomprenito")  # Wrong ending
print(f"Valid: {result.valid}")    # Output: False
```

## Detailed Usage

### 1. x_to_accent - X-System Conversion

Converts x-system notation (cx, gx, etc.) to Unicode accented letters.

**Function Signature:**
```python
x_to_accent(word: str) -> str
```

**Examples:**

```python
from literumilo import x_to_accent

# Basic conversion
print(x_to_accent("cxirkaux"))
# Output: ĉirkaŭ

# Full sentence
print(x_to_accent("Gxi estas bela tago."))
# Output: Ĝi estas bela tago.

# Mixed usage
print(x_to_accent("cxiutage mangxas pomojn"))
# Output: ĉiutage manĝas pomojn

# Preserves capitalization
print(x_to_accent("CXapelo"))
# Output: Ĉapelo
```

**X-System Mapping:**
| X-System | Unicode | Letter Name  |
| -------- | ------- | ------------ |
| cx, CX   | ĉ, Ĉ    | c-circumflex |
| gx, GX   | ĝ, Ĝ    | g-circumflex |
| hx, HX   | ĥ, Ĥ    | h-circumflex |
| jx, JX   | ĵ, Ĵ    | j-circumflex |
| sx, SX   | ŝ, Ŝ    | s-circumflex |
| ux, UX   | ŭ, Ŭ    | u-breve      |

### 2. check_word - Single Word Analysis

Validates spelling and performs morphological analysis of a single Esperanto word.

**Function Signature:**
```python
check_word(word: str) -> AnalysisResult
```

**Returns:** `AnalysisResult` object with attributes:
- `word` (str): Word divided into morphemes, separated by periods
- `valid` (bool): True if the word is correctly spelled, False otherwise

**Basic Examples:**

```python
from literumilo import check_word

# Valid compound word with prefix and participle
result = check_word("ĉirkaŭiris")
print(f"OK> {result.word}" if result.valid else f"Bad> {result.word}")
# Output: OK> ĉirkaŭ.ir.is

# Complex compound with multiple morphemes
result = check_word("misliterumita")
print(result.word)   # mis.liter.um.it.a
print(result.valid)  # True

# Invalid word (wrong suffix usage)
result = check_word("kuraciisto")  # Should be "kuracisto"
print(result.word)   # kuraciisto (unchanged)
print(result.valid)  # False

# Using x-system input
word = x_to_accent("cxiutage")
result = check_word(word)
print(result.word)   # ĉiu.tag.e
print(result.valid)  # True
```

**Advanced Examples:**

```python
# Separator vowel
result = check_word("fingromontri")
print(result.word)   # fingr.o.montr.i
# The 'o' aids pronunciation between 'fingr' and 'montr'

# Multiple suffixes
result = check_word("malĝentileco")
print(result.word)   # mal.ĝentil.ec.o
# mal- (opposite) + ĝentil (polite) + -ec- (quality) + -o (noun)

# Accusative pronoun (special case)
result = check_word("vin")
print(result.word)   # vi.n
# Pronoun 'vi' (you) + accusative 'n'

# Abbreviation with hyphen
result = check_word("n-rojn")
print(result.word)   # n-r.ojn
print(result.valid)  # True

# Capitalization preserved
result = check_word("RIĈULO")
print(result.word)   # RIĈ.UL.O
```

**How check_word Works Internally:**

The function follows this decision tree:

```
1. Single character?
   → Return valid if letter

2. Abbreviation (second char is hyphen)?
   → Dictionary lookup

3. Exception (accusative pronoun)?
   → Return hardcoded analysis

4. No ending word (ne, dum, post)?
   → Dictionary lookup with without_ending=Yes

5. Detect grammatical ending (o, a, e, i, u, is, as, os, us, oj, aj, ojn, ajn, en)?
   → Remove ending

6. Simple word (root in dictionary)?
   → Return root + ending

7. Compound word?
   → Recursive morpheme analysis:
      a. Try all possible splits (longest to shortest)
      b. Check each part in dictionary
      c. Validate synthesis rules
      d. Try separator vowels if needed

8. Restore original capitalization
```

### 3. analyze_string - Text String Analysis

Analyzes an entire string of Esperanto text, processing each word individually.

**Function Signature:**
```python
analyze_string(text: str, mode: bool) -> str
```

**Parameters:**
- `text` (str): Esperanto text to analyze
- `mode` (bool):
  - `True`: Morpheme mode (returns text with morpheme divisions)
  - `False`: Spell-check mode (returns newline-separated list of unknown words)

**Morpheme Mode (mode=True):**

```python
from literumilo import analyze_string

# Simple sentence
text = "Birdoj estas belaj."
result = analyze_string(text, True)
print(result)
# Output: Bird.oj est.as bel.aj.

# Complex sentence with compound words
text = "La malbonkomprenita misdirita vorto kaŭzis grandan problemon."
result = analyze_string(text, True)
print(result)
# Output: La mal.bon.kompren.it.a mis.dir.it.a vort.o kaŭz.is grand.an problem.on

# Scientific text
text = "Birdoj (Aves) estas klaso de vertebruloj kun ĉirkaŭ 9 ĝis 10 mil vivantaj specioj."
result = analyze_string(text, True)
print(result)
# Output: Bird.oj (Aves) est.as klas.o de vertebr.ul.oj kun ĉirkaŭ 9 ĝis 10 mil viv.ant.aj speci.oj
```

**Spell-Check Mode (mode=False):**

```python
# Find unknown words
text = "Birdoj (Aves) estas klaso de vertebruloj."
result = analyze_string(text, False)
print(result)
# Output: Aves
# (Only "Aves" is not in the Esperanto dictionary)

# Detect misspellings
text = "Mi volas mangxi pomojn kaj bananojn aujourdhui."
result = analyze_string(text, False)
print(result)
# Output: aujourdhui
# (French word, not Esperanto)

# Multiple errors
text = "La katto mangxis fiŝon kaj trinkis aqua."
result = analyze_string(text, False)
print(result)
# Output: aqua
# (Should be "akvon")
```

**How analyze_string Works:**

```
For each character in text:
  1. Is it a word character (letter, accented letter, hyphen)?
     YES → Accumulate into current word
     NO  →
       a. Process accumulated word with check_word()
       b. In morpheme mode: output result.word
       c. In spell-check mode: if invalid, add to bad_words set
       d. Output the non-word character (space, punctuation, etc.)
       e. Reset word accumulator

Return:
  - Morpheme mode: Complete text with morpheme divisions
  - Spell-check mode: Newline-separated list of unique unknown words
```

### 4. analyze_file - File Analysis

Reads an entire file and performs the same analysis as `analyze_string`.

**Function Signature:**
```python
analyze_file(filename: str, mode: bool) -> str
```

**Parameters:**
- `filename` (str): Path to file containing Esperanto text
- `mode` (bool): Same as `analyze_string`

**Examples:**

```python
from literumilo import analyze_file

# Morpheme analysis of a file
result = analyze_file("esperanto_article.txt", True)
print(result)
# Prints entire file with words divided into morphemes

# Spell check an entire document
result = analyze_file("my_essay.txt", False)
if result.strip():
    print("Unknown words found:")
    print(result)
else:
    print("No spelling errors detected!")

# Process a book chapter
result = analyze_file("chapters/chapter1.txt", True)
with open("chapters/chapter1_analyzed.txt", "w") as f:
    f.write(result)
```

**File Format Requirements:**
- Plain text file
- UTF-8 encoding (for Esperanto accented characters)
- Any text format (paragraphs, lists, dialogue, etc.)

## Internal Architecture: How Literumilo Works

### 1. Dictionary Structure

The morpheme database (`literumilo/data/vortaro.tsv`) contains **10,695 rows** (10,543 unique morphemes after loading), each with **9 fields**:

```tsv
morpheme	part_of_speech	meaning	transitivity	without_ending	with_ending	synthesis	rarity	flag
divid	VERBO	N	T	N	KF	NLM	1	R
mis	ADVERBO	N	N	N	KF	P	0	R
ul	SUBST	PERSONO	N	N	KF	S	0	R
```

**Field Descriptions:**

| Field              | Values                                                                                                                                                                                        | Description                                          |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| **morpheme**       | String                                                                                                                                                                                        | Base morpheme form (lowercase, no accents in x-form) |
| **part_of_speech** | SUBST, VERBO, ADJ, ADVERBO, PREFIKSO, SUFIKSO, PREPOZICIO, KONJUNKCIO, SUBJUNKCIO, INTERJEKCIO, TEHXPREFIKSO, ARTIKOLO, PARTICIPO, MALLONGIGO, LITERO, NUMERO, PRONOMO, PRONOMADJ, SUBSTVERBO | Grammatical category                                 |
| **meaning**        | N, PERSONO, ANIMALO, URBO, LANDO, PROFESIO, PARENCO, ETNO, etc. (118 types)                                                                                                                   | Semantic category for synthesis validation           |
| **transitivity**   | T (transitive), N (intransitive), X (both)                                                                                                                                                    | For verbs only                                       |
| **without_ending** | SF (sen finaĵo - can stand alone), N (no)                                                                                                                                                     | Words like 'ne', 'dum', 'post'                       |
| **with_ending**    | KF (kun finaĵo - takes ending), N (no)                                                                                                                                                        | Most roots take endings                              |
| **synthesis**      | P (prefix), S (suffix), PRT (participle), LM (limited), NLM (not limited), N (no combination)                                                                                                 | Combinability rules                                  |
| **rarity**         | 0-4                                                                                                                                                                                           | 0=very common, 4=rare                                |
| **flag**           | R (root), K (compound), X (exclude)                                                                                                                                                           | X=exclude from dictionary                            |

**Key Insight:** The synthesis and meaning fields enable context-aware validation. For example:
- A prefix marked with meaning=PARENCO (kinship) can only attach to kinship terms
- A suffix with synthesis=S can only follow certain parts of speech
- Transitive verbs (transitivity=T) can take passive participles; intransitive cannot

### 2. Grammatical Endings

Literumilo recognizes **16 standard Esperanto endings**:

| Category      | Endings              | Examples                                     |
| ------------- | -------------------- | -------------------------------------------- |
| **Noun**      | o, on, oj, ojn       | hund.o, hund.on, hund.oj, hund.ojn           |
| **Adjective** | a, an, aj, ajn       | bel.a, bel.an, bel.aj, bel.a.jn              |
| **Verb**      | i, is, as, os, u, us | kur.i, kur.is, kur.as, kur.os, kur.u, kur.us |
| **Adverb**    | e, en                | rapid.e, rapid.en                            |

The analyzer retains each grammatical ending as a single segment in its output, so accusative forms appear as `hund.on` rather than `hund.o.n`.

**Ending Detection Algorithm:**

```python
def get_ending(word):
    """
    Checks the last 1-3 characters of a word to identify the ending.
    Returns Ending object or None.
    """
    last_char = word[-1]

    if last_char == 'o': return SUB_O
    if last_char == 'a': return ADJ_A
    if last_char == 'e': return ADV_E
    if last_char == 'i': return VERB_I
    if last_char == 'u': return VERB_U

    if last_char == 's':  # is, as, os, us
        second_last = word[-2]
        if second_last == 'i': return VERB_IS
        if second_last == 'a': return VERB_AS
        # ... etc.

    if last_char == 'n':  # on, an, en, ojn, ajn
        # ... complex logic for accusative and plural

    if last_char == 'j':  # oj, aj
        # ... logic for plural

    return None
```

### 3. Morpheme Analysis Algorithm

The core of Literumilo is a **recursive backtracking algorithm** that tries all possible word divisions:

```
check_word(word)
├─ Preprocess: lowercase, remove hyphens, detect capitalization
├─ Handle exceptions: accusative pronouns (ĝin, vin, etc.)
├─ Check words without endings: dictionary lookup with without_ending=Yes
├─ Detect and remove grammatical ending
├─ Try simple word: dictionary lookup with with_ending=Yes
└─ Try compound word:
   └─ find_morpheme(word_without_ending, index=0, morpheme_list)
      ├─ Base cases:
      │  ├─ Empty string → FAIL
      │  └─ Index >= 9 (max morphemes) → FAIL
      │
      ├─ Try full match (last morpheme):
      │  └─ If found and combinable → check_synthesis() → scan_morphemes()
      │
      ├─ Try partial matches (not last morpheme):
      │  └─ For size from (length-2) down to 2:
      │     ├─ Split: left[0:size] + right[size:]
      │     ├─ Check if left in dictionary
      │     ├─ Check if left can combine (synthesis != No)
      │     ├─ Recursively analyze right: find_morpheme(right, index+1)
      │     └─ Validate combination: check_synthesis()
      │
      └─ Try separator vowel (o, a, e):
         └─ If index > 0 and first char is o/a/e:
            ├─ Create temporary separator entry
            ├─ Recursively analyze rest: find_morpheme(rest[1:], index+1)
            └─ Validate: check_synthesis()
```

**Example Trace for "miskomprenita":**

```
check_word("miskomprenita")
│
├─ Ending detected: "a" (adjective)
├─ Word without ending: "miskomprenit"
├─ Dictionary lookup "miskomprenit" → NOT FOUND
│
└─ find_morpheme("miskomprenit", index=0)
   │
   ├─ Try size=10: "miskompren" → NOT FOUND
   ├─ Try size=9: "miskompreni" → NOT FOUND
   ├─ Try size=8: "miskompre" → NOT FOUND
   ├─ Try size=7: "miskompr" → NOT FOUND
   ├─ Try size=6: "miskompre" → NOT FOUND
   ├─ Try size=5: "miskomp" → NOT FOUND
   ├─ Try size=4: "misko" → NOT FOUND
   ├─ Try size=3: "mis" → FOUND! (adverb prefix)
   │  │
   │  ├─ synthesis=P (prefix) → Can combine ✓
   │  ├─ check_synthesis("komprenit", index=0)
   │  │
   │  └─ find_morpheme("komprenit", index=1)
   │     │
   │     ├─ Try size=7: "kompren" → FOUND! (verb root)
   │     │  │
   │     │  ├─ synthesis=NLM (unlimited) → Can combine ✓
   │     │  ├─ check_synthesis("it", index=1)
   │     │  │
   │     │  └─ find_morpheme("it", index=2)
   │     │     │
   │     │     └─ Full match: "it" → FOUND! (participle)
   │     │        │
   │     │        ├─ synthesis=PRT (participle) → Can combine ✓
   │     │        ├─ Last morpheme → call scan_morphemes()
   │     │        │
   │     │        └─ scan_morphemes([mis, kompren, it])
   │     │           │
   │     │           ├─ Check "mis" (index 0):
   │     │           │  └─ check_prefix("mis")
   │     │           │     └─ Adverbial prefix, needs verb → "kompren" is VERBO ✓
   │     │           │
   │     │           ├─ Check "kompren" (index 1):
   │     │           │  └─ synthesis=NLM → OK ✓
   │     │           │  └─ transitivity=T (transitive) → OK ✓
   │     │           │
   │     │           └─ Check "it" (index 2):
   │     │              └─ check_participle("it")
   │     │                 └─ Passive participle, needs transitive verb
   │     │                 └─ "kompren" is transitive ✓
   │     │
   │     │        └─ VALID!
   │     │
   │     └─ Return True
   │
   └─ Return True

Result: "mis.kompren.it.a" (VALID)
```

### 4. Prefix Validation: Context-Dependent Rules

Each prefix has specific requirements about what morphemes can follow it. The validation occurs in `scan_morphemes()` after the word is fully divided.

**Prefix Categories:**

**A. Adverbial Prefixes** (modify verbs)
```python
mis-, re-, ek-, for-, dis-, pli-
```
- **Rule**: Must be followed by a verb or substantive-verb
- **Examples**:
  - `mis.kompren.is` ✓ (mis + understand-verb)
  - `mis.bel.a` ✗ (mis + beautiful-adjective)

**B. Prepositional Prefixes** (can act as prepositions)
```python
al-, de-, el-, en-, kun-, sen-, sub-, super-, sur-, tra-, trans-,
per-, por-, pro-, pri-, antaŭ-, post-, apud-, ĉe-, dum-, ĝis-, laŭ-, preter-
```
- **Rule**: Can attach to verbs, or create adjectives/adverbs
- **Examples**:
  - `kun.ir.is` ✓ (together + went)
  - `sub.mar.a` ✓ (under + sea-adjective)

**C. Semantic Prefixes** (meaning-specific)

| Prefix      | Meaning            | Attaches To                      | Examples                                                       |
| ----------- | ------------------ | -------------------------------- | -------------------------------------------------------------- |
| **mal-**    | opposite           | verbs, adjectives, adverbs       | `mal.feliĉ.a` (unhappy), `mal.bona` (bad)                      |
| **ge-**     | both sexes         | persons, animals                 | `ge.patr.oj` (parents), `ge.hund.oj` (male and female dogs)    |
| **bo-**     | in-law             | kinship terms only               | `bo.patr.o` (father-in-law), `bo.frat.o` (brother-in-law)      |
| **eks-**    | ex-, former        | persons only                     | `eks.prezident.o` (ex-president)                               |
| **ne-**     | not                | adjectives, adverbs, participles | `ne.taŭg.a` (unsuitable), `ne.far.ebl.a` (unfeasible)          |
| **pra-**    | ancient, great-    | people, time periods             | `pra.avino` (great-grandmother), `pra.hom.o` (primitive human) |
| **pseŭdo-** | false, pseudo-     | nouns, adjectives                | `pseŭdo.scienc.o` (pseudoscience)                              |
| **sin-**    | self- (reflexive)  | transitive verbs only            | `sin.kritik.i` (to criticize oneself)                          |
| **po-**     | apiece, at rate of | creates adverbs                  | `po.pec.e` (by pieces)                                         |
| **ĉi-**     | this here          | creates adjectives/adverbs       | `ĉi.vesper.e` (this evening)                                   |
| **cis-**    | on near side       | rivers, mountains                | `cis.alp.a` (cisalpine)                                        |

**Implementation Example:**

```python
def check_mal(index, morpheme_list):
    """
    Prefix mal- (opposite) can only attach to:
    - Verbs (mal.kompren.i - to misunderstand)
    - Adjectives (mal.bon.a - bad)
    - Adverbs (mal.facil.e - difficultly)
    """
    if index != 0:  # Must be first
        return False

    last = morpheme_list.get_last_index()
    type_of_ending = morpheme_list.type_of_ending()

    # Check ending type
    if type_of_ending in [POS.Verb, POS.Adjective, POS.Adverb]:
        return True

    # Check morpheme parts of speech
    for i in range(index + 1, last + 1):
        entry = morpheme_list.get(i)
        if entry.part_of_speech in [POS.Verb, POS.SubstantiveVerb, POS.Adjective]:
            return True

    return False
```

### 5. Suffix Validation: Compatibility Rules

Suffixes transform the meaning and often the part of speech of the morphemes they attach to.

**Major Suffixes:**

| Suffix    | Meaning                  | Attaches To                                                    | Creates            | Example                        |
| --------- | ------------------------ | -------------------------------------------------------------- | ------------------ | ------------------------------ |
| **-ul**   | person characterized by  | nouns/verbs/adjectives/participles (not persons), prepositions | PERSONO            | `riĉ.ul.o` (rich person)       |
| **-ej**   | place                    | nouns/verbs/adjectives (not already "place")                   | LOKO               | `manĝ.ej.o` (restaurant)       |
| **-il**   | tool, instrument         | verbs                                                          | ILO                | `ŝraŭb.il.o` (screwdriver)     |
| **-in**   | female                   | persons, animals                                               | female version     | `patr.in.o` (mother)           |
| **-id**   | offspring                | animals, ethnicities                                           | young animal       | `kat.id.o` (kitten)            |
| **-an**   | member of group          | nouns (not persons)                                            | PERSONO            | `urb.an.o` (city dweller)      |
| **-ist**  | professional, supporter  | nouns/adjectives/verbs (not persons)                           | PERSONO            | `den.ist.o` (dentist)          |
| **-estr** | leader                   | nouns                                                          | PERSONO            | `ŝip.estr.o` (ship captain)    |
| **-ig**   | to cause, make           | morphemes ≤ adverb, prepositions/prefixes                      | VERBO-transitive   | `ruĝ.ig.i` (to redden)         |
| **-iĝ**   | to become                | morphemes ≤ adverb, prepositions/prefixes                      | VERBO-intransitive | `ruĝ.iĝ.i` (to become red)     |
| **-ad**   | continuous action        | verbs, nouns                                                   | VERBO              | `kur.ad.i` (to run repeatedly) |
| **-aĉ**   | bad quality              | any                                                            | same POS           | `hund.aĉ.o` (mongrel)          |
| **-ebl**  | able to be [verb]-ed     | transitive verbs                                               | ADJ                | `vid.ebl.a` (visible)          |
| **-em**   | tendency to              | nouns/adjectives/verbs                                         | ADJ                | `labor.em.a` (industrious)     |
| **-ec**   | quality, state           | adjectives, nouns                                              | SUBST              | `bel.ec.o` (beauty)            |
| **-ar**   | collection of            | nouns, participles                                             | SUBST              | `arb.ar.o` (forest)            |
| **-aĵ**   | concrete thing           | nouns/verbs/adjectives, prepositions, participles              | SUBST              | `manĝ.aĵ.o` (food)             |
| **-eg**   | augmentative (big)       | nouns, adjectives, verbs                                       | same POS           | `dom.eg.o` (mansion)           |
| **-et**   | diminutive (small)       | nouns, adjectives, verbs                                       | same POS           | `dom.et.o` (cottage)           |
| **-end**  | must be [verb]-ed        | transitive verbs                                               | ADJ                | `pag.end.a` (payable)          |
| **-ind**  | worthy to be [verb]-ed   | transitive verbs                                               | ADJ                | `vid.ind.a` (worth seeing)     |
| **-er**   | piece, fragment          | nouns                                                          | SUBST              | `mon.er.o` (coin)              |
| **-ik**   | science, field           | nouns                                                          | SUBST              | `fizik.o` (physics)            |
| **-ing**  | holder                   | nouns                                                          | SUBST              | `kandel.ing.o` (candlestick)   |
| **-ism**  | doctrine, practice       | nouns                                                          | SUBST              | `ideal.ism.o` (idealism)       |
| **-uj**   | container, tree, country | nouns                                                          | SUBST              | `pom.uj.o` (apple tree)        |
| **-obl**  | multiple                 | numbers                                                        | ADJ                | `du.obl.e` (double)            |
| **-on**   | fraction                 | numbers                                                        | SUBST              | `kvar.on.o` (quarter)          |
| **-op**   | collective               | numbers                                                        | SUBST              | `tri.op.o` (trio)              |

**Key Property Inheritance:**

Some suffixes **inherit** properties from the previous morpheme:

```python
def check_acx(index, morpheme_list):
    """
    Suffix -aĉ (bad quality) doesn't change the essential nature.
    If kri.as is intransitive, then kri.aĉ.as is also intransitive.
    Properties must be transferred.
    """
    previous_entry = morpheme_list.get(index - 1)
    current_entry = morpheme_list.get(index)

    if previous_entry.part_of_speech <= POS.Adjective:
        # Transfer properties
        current_entry.part_of_speech = previous_entry.part_of_speech
        current_entry.meaning = previous_entry.meaning
        current_entry.transitivity = previous_entry.transitivity
        return True

    return False
```

**Property Transformation:**

Other suffixes **transform** properties:

```python
def check_ad(index, morpheme_list):
    """
    Suffix -ad (continuous action) transforms to verb.
    martel (hammer-noun) + -ad → martel.ad.i (to hammer repeatedly)
    Takes transitivity from previous morpheme.
    """
    previous_entry = morpheme_list.get(index - 1)
    current_entry = morpheme_list.get(index)

    if previous_entry.part_of_speech <= POS.Verb:
        # Transform to verb, inherit transitivity
        current_entry.part_of_speech = POS.Verb
        current_entry.transitivity = previous_entry.transitivity
        return True

    return False
```

### 6. Participle Endings: Transitivity Requirements

Esperanto has **6 participle endings** expressing time and voice:

| Ending    | Time    | Voice   | Requires            | Example                       |
| --------- | ------- | ------- | ------------------- | ----------------------------- |
| **-ant-** | present | active  | any verb            | `kur.ant.a` (running)         |
| **-int-** | past    | active  | any verb            | `kur.int.a` (having run)      |
| **-ont-** | future  | active  | any verb            | `kur.ont.a` (about to run)    |
| **-at-**  | present | passive | **transitive verb** | `vid.at.a` (being seen)       |
| **-it-**  | past    | passive | **transitive verb** | `vid.it.a` (seen)             |
| **-ot-**  | future  | passive | **transitive verb** | `vid.ot.a` (about to be seen) |

**Critical Rule:** Passive participles require transitive verbs.

```python
def check_participle(index, morpheme_list):
    """
    Validates participle endings.
    Passive participles (-at-, -it-, -ot-) require transitive verbs.
    """
    entry = morpheme_list.get(index)
    previous_entry = morpheme_list.get(index - 1)

    participle = entry.morpheme  # "ant", "int", "ont", "at", "it", "ot"

    if previous_entry.part_of_speech in [POS.Verb, POS.SubstantiveVerb]:
        # Check if passive participle
        if len(participle) == 2:  # "at", "it", "ot" (2 chars = passive)
            # Must be transitive
            if previous_entry.transitivity != Transitivity.Transitive:
                return False

        # Allow certain suffixes after participle
        if index < last:
            next_entry = morpheme_list.get(index + 1)
            if next_entry.morpheme in ["aĵ", "ul", "in", "ec", "ar"]:
                return True

        return True

    return False
```

**Examples:**

```python
# Valid: transitive verb + passive participle
check_word("forgesita")    # forges.it.a (forgotten)
# forges = transitive verb → passive participle OK ✓

# Invalid: intransitive verb + passive participle
check_word("dormita")      # dormita
# dorm = intransitive verb → passive participle FAIL ✗

# Valid: intransitive verb + active participle
check_word("irinta")       # ir.int.a (having gone)
# ir = intransitive verb → active participle OK ✓
```

### 7. Separator Vowels: Pronunciation Aid

Esperanto allows inserting a **separator vowel** (o, a, or e) between morphemes to ease pronunciation.

**Rules:**
1. Only **one separator per word**
2. Cannot be the **first morpheme** (index must be > 0)
3. Must be compatible with **surrounding morphemes**
4. The separator takes the **part of speech of the vowel** (o=noun, a=adjective, e=adverb)

**Examples:**

```
fingr.montr.i    → harder to pronounce (consonant cluster: r.m)
fingr.o.montr.i  → easier (separator 'o' breaks up cluster)

last.temp.e      → harder (st.t cluster)
last.a.temp.e    → easier (separator 'a')

unu.foj.e        → OK
unu.a.foj.e      → easier (separator 'a')
```

**Implementation:**

```python
def find_morpheme(rest_of_word, dictionary, index, morpheme_list):
    # ... after trying normal morpheme splits ...

    # Try separator vowel
    if index == 0 or length_of_word < 3:
        return False

    # Check if first character is o, a, or e
    separator_entry = EspDictEntry.new_separator(rest_of_word[0])
    if separator_entry:
        morpheme_list.put(index, separator_entry)
        rest_of_word2 = rest_of_word[1:]  # Remove separator
        valid = check_synthesis(rest_of_word2, dictionary, index, morpheme_list, False)
        if valid:
            return True

    return False

def valid_separator(pos, index, morpheme_list):
    """
    Validates that separator vowel is compatible with context.
    """
    if index == 0:  # Can't be first
        return False

    previous = morpheme_list.get(index - 1)
    previous_pos = previous.part_of_speech
    type_of_ending = morpheme_list.type_of_ending()

    # Substantive separator: previous must not be adjective
    if pos == POS.Substantive and previous_pos > POS.Adjective:
        return False

    # Adjective/Adverb separator: ending must match
    elif pos in [POS.Adjective, POS.Adverb]:
        if type_of_ending not in [POS.Adjective, POS.Adverb]:
            return False
        if previous_pos > POS.Adverb:
            return False

    return True
```

### 8. Limited Synthesis: Ambiguity Prevention

Short morphemes can create **ambiguities**, so they have **limited combinability**.

**Categories:**

**A. Limited Verbs (synthesis=LM, POS=Verb)**
```
Can only combine with:
  - Prefixes (mis-, re-, ek-, etc.)
  - Suffixes (-ul, -ej, -il, etc.)
  - Participle endings (-ant, -it, etc.)
Cannot combine with:
  - Other roots
```

Example: `ir` (go) is a short verb
```
✓ re.ir.is        (prefix + ir + ending)
✓ ir.ant.o        (ir + participle + ending)
✓ ir.ej.o         (ir + suffix + ending)
✗ ir.salt.i       (ir + another root)
```

**B. Limited Animals (synthesis=LM, meaning=ANIMALO/BIRDO/etc.)**
```
Can only combine with:
  - vir- (male)
  - -in (female)
  - -id (offspring)
  - -aĵ (thing)
  - -ov (egg)
```

Example: `kat` (cat)
```
✓ vir.kat.o       (male cat)
✓ kat.in.o        (female cat)
✓ kat.id.o        (kitten)
✗ kat.dom.o       (cat-house - not allowed, should be kat.ej.o)
```

**C. Limited Kinship Terms (synthesis=LM, meaning=PARENCO)**
```
Can only combine with:
  - pra- (great-)
  - ge- (both sexes)
  - bo- (in-law)
  - -in (female)
```

Example: `patr` (parent/father)
```
✓ ge.patr.oj      (parents)
✓ bo.patr.o       (father-in-law)
✓ patr.in.o       (mother)
✓ pra.patr.o      (grandfather)
✗ patr.dom.o      (father-house - not allowed)
```

**D. Limited Ethnicities (synthesis=LM, meaning=ETNO)**
```
Can only combine with:
  - ge- (both sexes)
  - -in (female)
  - -id (descendant)
  - -land (country)
  - -stil (style)
```

Example: `german` (German person)
```
✓ german.in.o     (German woman)
✓ german.id.o     (child of German)
✗ german.hund.o   (German dog - not allowed, should be germana hundo)
```

**Implementation:**

```python
def check_limited_synthesis(morpheme, index, morpheme_list):
    """
    Enforces limited combinability rules.
    """
    entry = morpheme_list.get(index)
    pos = entry.part_of_speech
    meaning = entry.meaning

    # Limited verbs
    if pos in [POS.Verb, POS.SubstantiveVerb]:
        if index > 0:
            prev = morpheme_list.get(index - 1)
            if prev.synthesis != Synthesis.Prefix:
                return False
        if index < last:
            next = morpheme_list.get(index + 1)
            if next.synthesis not in [Synthesis.Suffix, Synthesis.Participle]:
                return False

    # Limited animals
    elif is_animal(meaning):
        if index > 0:
            prev = morpheme_list.get(index - 1)
            if prev.morpheme != "vir":
                return False
        if index < last:
            next = morpheme_list.get(index + 1)
            if next.morpheme not in ["in", "id", "aĵ", "ov"]:
                return False

    # Limited kinship
    elif meaning == Meaning.PARENCO:
        if index > 0:
            prev = morpheme_list.get(index - 1)
            if prev.morpheme not in ["bo", "ge", "pra"]:
                return False
        if index < last:
            next = morpheme_list.get(index + 1)
            if next.morpheme != "in":
                return False

    # Limited ethnicity
    elif meaning == Meaning.ETNO:
        if index > 0:
            prev = morpheme_list.get(index - 1)
            if prev.morpheme != "ge":
                return False
        if index < last:
            next = morpheme_list.get(index + 1)
            if next.morpheme not in ["in", "id", "land", "stil"]:
                return False

    return True
```

### 9. Exception Handling: Special Cases

Certain words cannot be handled by the general algorithm and require **hardcoded exceptions**.

**Accusative Pronouns:**

```python
# In check_word():
if length_of_word < 5:
    if word == "ĝin": return AnalysisResult(original_word, "ĝi.n", True)
    if word == "lin": return AnalysisResult(original_word, "li.n", True)
    if word == "min": return AnalysisResult(original_word, "mi.n", True)
    if word == "sin": return AnalysisResult(original_word, "si.n", True)
    if word == "vin": return AnalysisResult(original_word, "vi.n", True)
    if word == "lian": return AnalysisResult(original_word, "li.an", True)
    if word == "cian": return AnalysisResult(original_word, "ci.an", True)
```

**Why Exceptions Are Needed:**

| Word  | Problem                                                           | Solution            |
| ----- | ----------------------------------------------------------------- | ------------------- |
| `vin` | Could be pronoun `vi.n` (you-accusative) OR wine root `vin.o`     | Hardcoded as `vi.n` |
| `sin` | Could be pronoun `si.n` (self-accusative) OR prefix `sin-` (self) | Hardcoded as `si.n` |
| `min` | Could be pronoun `mi.n` (me-accusative) OR root `min.o` (mine)    | Hardcoded as `mi.n` |

The dictionary cannot have duplicate keys, so these pronouns are excluded from the dictionary and handled as special cases.

## Complete Analysis Example: "neforgesebla"

Let's trace the full analysis of **"neforgesebla"** (unforgettable):

**Step 1: Input Processing**
```
Input: "neforgesebla"
Lowercase: "neforgesebla"
Original capitalization: Minuscule
```

**Step 2: Exception Check**
```
Length: 12 (>5, not a special pronoun)
No hyphen in position 1
Not in exceptions list
```

**Step 3: Ending Detection**
```
get_ending("neforgesebla")
→ Last char: 'a' → ADJ_A (adjective ending)
→ Word without ending: "neforgesebl"
→ Ending length: 1
```

**Step 4: Simple Word Check**
```
Dictionary lookup: "neforgesebl" → NOT FOUND
Proceed to compound analysis
```

**Step 5: Recursive Morpheme Division**

```
find_morpheme("neforgesebl", index=0, morpheme_list)
│
├─ Try size=9: "neforgeseb" → NOT FOUND
├─ Try size=8: "neforgeseb" → NOT FOUND
├─ Try size=7: "neforges" → NOT FOUND
├─ Try size=6: "neforge" → NOT FOUND
├─ Try size=5: "neforg" → NOT FOUND
├─ Try size=4: "nefor" → NOT FOUND
├─ Try size=3: "nefo" → NOT FOUND
├─ Try size=2: "ne" → FOUND!
│  │
│  ├─ Entry: ADVERBO, synthesis=P (prefix)
│  ├─ Can combine: Yes ✓
│  ├─ morpheme_list[0] = ne
│  │
│  └─ check_synthesis("forgesebl", index=0, last_morpheme=False)
│     │
│     ├─ Suffix check: ne is prefix → Skip
│     │
│     └─ find_morpheme("forgesebl", index=1, morpheme_list)
│        │
│        ├─ Try size=7: "forgeseb" → NOT FOUND
│        ├─ Try size=6: "forgesebl" → NOT FOUND (full match, but not last)
│        ├─ Try size=6: "forgeseb" → NOT FOUND
│        ├─ Try size=5: "forges" → FOUND!
│        │  │
│        │  ├─ Entry: VERBO, transitivity=T, synthesis=NLM
│        │  ├─ Can combine: Yes ✓
│        │  ├─ morpheme_list[1] = forges
│        │  │
│        │  └─ check_synthesis("ebl", index=1, last_morpheme=False)
│        │     │
│        │     ├─ Suffix check: forges is not suffix → Skip
│        │     │
│        │     └─ find_morpheme("ebl", index=2, morpheme_list)
│        │        │
│        │        └─ Full match: "ebl" → FOUND!
│        │           │
│        │           ├─ Entry: ADJ, synthesis=S (suffix)
│        │           ├─ Can combine: Yes ✓
│        │           ├─ morpheme_list[2] = ebl
│        │           │
│        │           └─ check_synthesis("ebl", index=2, last_morpheme=True)
│        │              │
│        │              ├─ Suffix check: ebl is suffix
│        │              │  └─ check_suffix("ebl", 2, morpheme_list)
│        │              │     └─ check_ebl():
│        │              │        ├─ Previous: forges (index 1)
│        │              │        ├─ POS: Verb ✓
│        │              │        └─ Transitivity: Transitive ✓
│        │              │        → VALID ✓
│        │              │
│        │              └─ Last morpheme: True
│        │                 └─ scan_morphemes(morpheme_list)
│        │                    │
│        │                    ├─ Separator count: 0 ✓
│        │                    │
│        │                    ├─ Check morpheme[0]: "ne"
│        │                    │  ├─ synthesis = P (Prefix)
│        │                    │  └─ check_prefix("ne", 0, morpheme_list)
│        │                    │     ├─ Must be first: index=0 ✓
│        │                    │     ├─ Ending type: ADJ ✓
│        │                    │     ├─ Can attach to adjectives/adverbs ✓
│        │                    │     └─ VALID ✓
│        │                    │
│        │                    ├─ Check morpheme[1]: "forges"
│        │                    │  ├─ synthesis = NLM (unlimited)
│        │                    │  └─ No special checks needed ✓
│        │                    │
│        │                    └─ Check morpheme[2]: "ebl"
│        │                       ├─ synthesis = S (Suffix)
│        │                       └─ Already checked in check_synthesis ✓
│        │
│        │                    → All checks passed ✓
│        │
│        └─ Return True
│
└─ Return True
```

**Step 6: Display Form Construction**
```
morpheme_list.display_form()
→ Morphemes: ["ne", "forges", "ebl"]
→ Ending: "a"
→ Result: "ne.forges.ebl.a"
```

**Step 7: Capitalization Restoration**
```
restore_capitals("neforgesebla", "ne.forges.ebl.a")
→ Original: minuscule
→ Result: "ne.forges.ebl.a" (unchanged)
```

**Step 8: Return Result**
```python
AnalysisResult(
    word="ne.forges.ebl.a",
    valid=True
)
```

**Breakdown:**
- `ne-`: negative prefix (not)
- `forges`: verb root (forget) [transitive]
- `-ebl-`: possibility suffix (able to be)
- `-a`: adjective ending
- **Meaning**: "not able to be forgotten" = unforgettable

## Advanced Use Cases

### 1. Batch Processing

```python
from literumilo import check_word

# Process a list of words
words = ["malbona", "belega", "rapide", "forgesita", "neebla"]
results = []

for word in words:
    result = check_word(word)
    results.append({
        'original': word,
        'morphemes': result.word,
        'valid': result.valid
    })

for r in results:
    status = "✓" if r['valid'] else "✗"
    print(f"{status} {r['original']:15} → {r['morphemes']}")

# Output:
# ✓ malbona         → mal.bon.a
# ✓ belega          → bel.eg.a
# ✓ rapide          → rapid.e
# ✓ forgesita       → forges.it.a
# ✗ neebla          → neebla
```

### 2. Educational Tool

```python
from literumilo import check_word

def explain_word(word):
    """Provide detailed analysis of an Esperanto word."""
    result = check_word(word)

    if not result.valid:
        print(f"❌ '{word}' is not a valid Esperanto word.")
        return

    morphemes = result.word.split('.')
    print(f"✓ '{word}' → {result.word}")
    print(f"  Number of morphemes: {len(morphemes)}")
    print(f"  Morpheme breakdown:")

    for i, morph in enumerate(morphemes, 1):
        print(f"    {i}. {morph}")

# Example usage
explain_word("miskomprenita")
# Output:
# ✓ 'miskomprenita' → mis.kompren.it.a
#   Number of morphemes: 4
#   Morpheme breakdown:
#     1. mis
#     2. kompren
#     3. it
#     4. a
```

### 3. Spell Checker Integration

```python
from literumilo import analyze_string

def spell_check_document(text):
    """Check spelling and return detailed report."""
    unknown = analyze_string(text, False)

    if not unknown.strip():
        return "✓ No spelling errors found!"

    errors = unknown.strip().split('\n')
    report = f"Found {len(errors)} unknown word(s):\n"

    for i, word in enumerate(errors, 1):
        report += f"  {i}. {word}\n"

    return report

# Example
text = "Mi volas mangxi pomojn kaj wrong_word bananojn."
print(spell_check_document(text))
# Output:
# Found 1 unknown word(s):
#   1. wrong_word
```

### 4. Morpheme Statistics

```python
from literumilo import analyze_string
from collections import Counter

def morpheme_statistics(text):
    """Analyze morpheme usage in text."""
    result = analyze_string(text, True)

    # Extract morphemes (exclude punctuation)
    morphemes = []
    for word in result.split():
        if '.' in word:
            morphemes.extend(word.replace('.', '').lower())

    # Count frequency
    freq = Counter(morphemes)

    print("Top 10 morphemes:")
    for morph, count in freq.most_common(10):
        print(f"  {morph}: {count}")

# Example usage with a longer text
text = """
Esperanto estas planlingvo kreita de D-ro Ludoviko Lazaro Zamenhof.
La unua libro pri Esperanto estis publikigita en 1887.
"""
morpheme_statistics(text)
```

## Performance and Limitations

### Computational Complexity

| Operation              | Time Complexity      | Space Complexity               |
| ---------------------- | -------------------- | ------------------------------ |
| Dictionary lookup      | O(1) average         | O(n) where n≈10,700 entries    |
| Simple word check      | O(1)                 | O(1)                           |
| Compound word analysis | O(m² × d) worst case | O(m) where m=max morphemes (9) |
| String analysis        | O(k × m² × d)        | O(k) where k=text length       |

Where:
- m = number of morphemes in word
- d = average morpheme length
- k = text length

**Worst Case:** A very long word with many possible divisions requires trying many splits.

**Best Case:** Simple words are O(1) dictionary lookups.

### Memory Usage

- Dictionary: ~2-3 MB in RAM (loaded once at import)
- Per-word analysis: ~1-2 KB temporary structures

### Current Limitations

1. **Maximum Morpheme Count**: 9 morphemes per word
   ```python
   # If a word has more than 9 morphemes, analysis fails
   ```

2. **Dictionary Dependency**: Only morphemes in `vortaro.tsv` are recognized
   ```python
   # New morphemes require adding to the dictionary
   ```

3. **Single Separator Vowel**: Only one separator per word
   ```python
   # fingr.o.montr.o.far.i would be invalid (two separators)
   ```

4. **Language-Specific**: Only works for Esperanto
   ```python
   # Cannot be used for other languages
   ```

5. **No Semantic Analysis**: Only grammatical validation
   ```python
   # "blua kato" (blue cat) is grammatically valid
   # but semantic oddness is not detected
   ```

6. **Recursion Depth**: Very complex words may hit Python's recursion limit
   ```python
   # Theoretically limited by Python's default recursion depth (usually 1000)
   # In practice, 9-morpheme limit prevents this
   ```

## Testing

Run the unit tests:

```bash
cd literumilo
python3 -m unittest tests/test_literumilo.py
```

Test coverage includes:
- Simple words
- Compound words
- X-system conversion
- File analysis
- Invalid words
- Edge cases

## Troubleshooting

### Common Issues

**Q: Word is valid Esperanto but marked invalid**
```
A: The morpheme might not be in the dictionary (vortaro.tsv).
   Check if the morpheme is a recent addition to Esperanto.
```

**Q: Separator vowel not recognized**
```
A: Check:
   1. Only one separator per word is allowed
   2. Separator must be 'o', 'a', or 'e'
   3. Cannot be the first morpheme
```

**Q: Prefix/suffix combination rejected**
```
A: The combination may violate Esperanto grammar rules.
   Example: "mal-" only attaches to verbs/adjectives,
   not to nouns.
```

**Q: Accusative pronoun not recognized**
```
A: Some pronouns are hardcoded exceptions.
   Currently supported: ĝin, vin, lin, min, sin, lian, cian
```

## Links

- **GitHub**: [https://github.com/Indrikoterio/literumilo-python](https://github.com/Indrikoterio/literumilo-python)
- **PyPI**: [https://pypi.org/project/literumilo/](https://pypi.org/project/literumilo/)

## Developer

Literumilo was developed by **Cleve (Klivo) Lendon**.

## Contact

To contact the developer, send email to **indriko@yahoo.com**.

Preferred languages: English and Esperanto.

Comments, suggestions, and criticism are welcomed.

## Contributing

Contributions are welcome! Areas for improvement:

1. **Dictionary Expansion**
   - Add new morphemes
   - Update meaning categories
   - Refine synthesis rules

2. **Algorithm Optimization**
   - Memoization for repeated word patterns
   - Parallel processing for file analysis

3. **Feature Additions**
   - Confidence scores for ambiguous analyses
   - Alternative valid divisions
   - Semantic validation

4. **Documentation**
   - More examples
   - Tutorial videos
   - Esperanto translation of docs

5. **Testing**
   - Expand test suite
   - Performance benchmarks
   - Edge case coverage

## History

- **May 2020**: First release (v1.0.0)
- **November 2020**: Current version (v1.0.8)

## License

Literumilo is **free software**. It is distributed free of charge, without conditions, and without guarantees.

You may use, modify, and distribute it as you wish. There is no need to ask for permission.

If you use Literumilo's code in your own project and publish it, the author requests only that you acknowledge the source.

## Acknowledgments

This project builds upon the systematic and logical structure of **Esperanto**, created by **L. L. Zamenhof** in 1887.

The morpheme database draws from various Esperanto dictionaries and linguistic resources, including:

- **PIV** (Plena Ilustrita Vortaro): Comprehensive Esperanto dictionary
- **PMEG** (Plena Manlibro de Esperanta Gramatiko): Complete grammar reference
- **ReVo** (Reta Vortaro): Online dictionary
- **Akademio de Esperanto**: Official language academy

Special thanks to the Esperanto community for maintaining these resources.

## References

### Esperanto Resources

- **PIV** (Plena Ilustrita Vortaro): [https://vortaro.net/](https://vortaro.net/)
- **PMEG** (Plena Manlibro de Esperanta Gramatiko): [http://bertilow.com/pmeg/](http://bertilow.com/pmeg/)
- **ReVo** (Reta Vortaro): [http://www.reta-vortaro.de/revo/](http://www.reta-vortaro.de/revo/)
- **Akademio de Esperanto**: [http://akademio-de-esperanto.org/](http://akademio-de-esperanto.org/)

### Linguistic Background

- Zamenhof, L. L. (1887). *Unua Libro* (First Book)
- Kalocsay, K. & Waringhien, G. (1985). *Plena Analiza Gramatiko* (Complete Analytical Grammar)
- Wells, J. (1978). *Lingvistikaj aspektoj de Esperanto* (Linguistic Aspects of Esperanto)

---

**Ĝuu la programon! / Enjoy the program!**

**La programo ankaŭ funkcias en Esperanto!** *(The program also works in Esperanto!)*
