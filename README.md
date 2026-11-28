# LedgerMath Runs

The measurements of [LedgerMath](https://github.com/smrdotgg/ledgermath): the record of each
execution of a benchmark on a LedgerMath box that reached LedgerMath's bucket, failed and
interrupted executions included; the summary of each boot of a box that reached it; and
AWS's published prices for what is measured, fetched once a day. Not every execution or boot
leaves a file. A box killed outright or losing power, an upload that fails, or a boot that
fails before its runner starts can leave none, and a day whose price fetch fails has no price
file. The source repository's README says exactly what a boot guarantees and what it does
not, in its section "What a boot guarantees, and what it does not". How the files are made,
and the schemas they follow, are in the source repository,
<https://github.com/smrdotgg/ledgermath>.

| Path | What it is |
|---|---|
| `runs/<yyyy>/<mm>/<dd>/<id>.json` | A Run: one execution of one benchmark binary, under the UTC date it started; `<id>` is the Run's id. Schema: the source repository's `schemas/run.schema.json` |
| `boots/<yyyy>/<mm>/<dd>/<boot>.json` | A boot summary: every execution one boot of a box was scheduled to make, and what became of it, under the UTC date the boot's runner started; `<boot>` is the kernel's boot id |
| `prices/<yyyy>/<mm>/<dd>/aws.json` | AWS's published price-list records for what is measured, as fetched that UTC day. Schema: the source repository's `schemas/prices.schema.json` |

Besides those three directories, the repository holds this README and the licence,
[`LICENSE`](LICENSE).

Files are only added, never edited or removed. The source repository's workflows add them:
`runs.yml`, four times a day, copies the Runs and boot summaries here byte for byte from the
bucket the boxes upload to, and `prices.yml`, once a day, makes the price file from AWS's
price lists. The same Run files and boot summaries are also served, at the same paths, under
`https://api.ledgermath.com/` (for example
`https://api.ledgermath.com/runs/<yyyy>/<mm>/<dd>/<id>.json`).

Series and their figures are worked out from the Runs by Methods, each named and versioned,
whose definitions and code are in the source repository (`methods/README.md`). A correction
is a new version of a Method, never a changed file here. What
the Methods derive is published as the data API, a tree of static JSON files that starts at
<https://api.ledgermath.com/v1/index.json>, and charted at <https://ledgermath.com/>.

## Licence

The Runs and boot summaries are published by The LedgerMath Authors under the Creative
Commons Attribution 4.0 International licence (CC BY 4.0), whose legal code is
[`LICENSE`](LICENSE). In short, though the legal code is what governs: you may share and
adapt them for any purpose. When you share them, or material adapted from them, you give
credit, link to the licence, and say if you changed them.

The licence lets credit be given "in any reasonable manner based on the medium, means, and
context", for example by a link to a page that holds the information (its section 3(a)).
One reasonable way, for one Run, is:

> LedgerMath, Run `<id>` (`runs/<yyyy>/<mm>/<dd>/<id>.json`),
> https://github.com/smrdotgg/ledgermath-runs, licensed under CC BY 4.0,
> https://creativecommons.org/licenses/by/4.0/

with a word saying whether what you show was computed from the Runs or changed. For a figure
computed from many Runs, LedgerMath's documentation, <https://ledgermath.com/docs/>, says in
its section "What a citation names" how to name the Series, the Method and the moment; for a
price, the file, SKU and rate code.

## What the licence does not cover

- **Prices.** `prices/` holds AWS's own published price-list records, reproduced as AWS
  published them. Each file gives, for each record's source, its address and the SHA-256 of
  the file read there. LedgerMath claims no rights in AWS's records, and the licence above
  does not apply to them. What LedgerMath adds around them in each file (when they were
  fetched, from where, which rule selected each record) is under CC BY 4.0 like the Runs.
- **Blobs.** A Run names other files by the SHA-256 of their bytes (`sha256:<hex>`): its
  environment dumps, its stdout and stderr, the binary that ran, and the source archive the
  binary was built from. They are not in this repository. Each is served at
  `https://api.ledgermath.com/blobs/sha256/<hex>`, where `<hex>` is the SHA-256 of its
  bytes in 64 lowercase hex digits; check the bytes against it.

  The dumps, stdout and stderr are published with the Runs, under CC BY 4.0, except what the
  dumps reproduce of AWS's records (what the instance metadata service and the EC2 API
  returned to the box), in which LedgerMath claims no rights. The other blobs have licences
  of their own:

  - The source archives hold the source repository's files: LedgerMath's own code, under
    Apache-2.0, and the material from other projects that `THIRD-PARTY-NOTICES.md` lists,
    under its own licences and notices.
  - The benchmark binaries contain third-party code. Each static binary contains parts of
    the GNU C Library, which is covered by the GNU Lesser General Public License, version
    2.1 or later, and parts of GCC's runtime library; `http_get`'s also contains AWS-LC. The
    dynamic build of `memory_read` loads the C library at run time, and contains only the
    start-up files of glibc and GCC and one object of GCC's runtime library.
  - AWS-LC's source archive is AWS-LC's own, under the several licences its `LICENSE` file
    states.
  - The GNU C Library's source package, the one the binaries were built against, is served
    the same way, at
    `https://api.ledgermath.com/blobs/sha256/5c14047c719844c616626f2c139ae5b4fb19023762407565e824186e5812f9cf`
    (`glibc-2.34-231.amzn2023.0.4.src.rpm`).

  `THIRD-PARTY-NOTICES.md` in the source repository lists all of these, with their licences,
  the notices they carry and where their source is; glibc's licence text and notices are in
  its directory `third-party/glibc-2.34/`.

## No warranty

The numbers are measurements, published as they were recorded, failed and interrupted
executions included. They describe the machines and software as they were when measured,
and may be wrong. They come with no warranty of any kind (section 5 of the licence).

The page that charts them: <https://ledgermath.com/>.
