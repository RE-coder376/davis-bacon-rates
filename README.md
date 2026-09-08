# Davis-Bacon Rate Lookup

**Live: https://re-coder376.github.io/davis-bacon-rates/**

Every current US federal construction wage determination, parsed from SAM.gov and
republished as a county-level reference. Free, no sign-up, nothing tracked.

The government publishes these as PDFs that are close to unusable in practice: a
contractor who needs the fringe rate for one trade in one county has to read a
60-page document. This is one page per county per construction type.

| | |
|---|---|
| Determinations parsed | **4,009** |
| Fetch failures | **0** |
| Counties covered | **3,063** |
| Classifications, with base rate and fringe | **114,283** |
| Pages published | **7,125** |
| Construction types | Building, Residential, Highway, Heavy |

## Three parser faults worth knowing about

All three dropped rates *silently* — the output looked correct and was short.

1. **Optional fringe fields.** A determination line may carry no fringe at all. The
   first parser treated the missing field as a malformed row and skipped the line.
2. **Labels containing colons.** Trade names with a colon in them split on the wrong
   delimiter and lost everything after it.
3. **Fringes with a third decimal and a footnote marker.** A value like `12.345**`
   failed to parse as a number and the whole classification was discarded. This one
   was dropping **every elevator mechanic in the country**, and nothing in the
   rendered output looked wrong.

The third is the reason this repository states its fetch and parse counts: a scraper
that reports success while discarding a whole trade is worse than one that crashes.

## How it is built

Determinations are fetched from SAM.gov with two concurrent workers, parsed into
structured rows, and rendered to static HTML. All 7,125 URLs were submitted through
IndexNow and Search Console, because a reference site nobody can find is not a
reference.

This branch (`gh-pages`) holds the generated site only — HTML, the data directory,
sitemap and stylesheet. It is output, not source.

## Accuracy

Rates are reproduced from the published determinations and are only as current as the
last refresh. **Always confirm the figure against the determination governing your
specific contract before relying on it.** Each page links back to its source document
so that check takes one click.
