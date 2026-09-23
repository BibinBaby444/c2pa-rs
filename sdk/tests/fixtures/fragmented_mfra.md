# Fragmented MP4 TFRA Fixture

`fragmented_mfra.mp4` is Bibin's original regression fixture from
[PR #1](https://github.com/mstattma/c2pa-rs/pull/1), commit
`6c1328cd83fe170123cf630a5a15b049a384c063`. It was imported unchanged from that
commit. The original description identifies an ffmpeg `testsrc` video with
five fragments; an exact generation command is not recorded here.

The fixture exercises preservation of the intended `moof` targets in `mfra/tfra`
when a C2PA manifest is inserted, grown, shrunk, or removed. Its one-entry-per-moof
layout is specific to this fixture, not a general TFRA requirement.

The diagnosis, original per-entry fix, and fixture were contributed by
BibinBaby444 (bibinbaby444@gmail.com). This backport adapts source commits
`6399956b7774d35ea23bab7c4197c1ccd0138d00` and
`dc415665c25f4efb779f4eb7f8817fdbeb2af2b9`.

On this stable branch the unchanged fixture has both `moov` and `moof`, so
public writes and full Store signing exercise the existing specialized
single-file path. The no-`moov` synthetics in `bmff_io/tfra_tests.rs` deliberately
exercise the legacy metadata writer, XMP reference embedding, and placeholder
insertion instead. They are table tests, not additional supported media layouts.
Do not add `moov` to those synthetics: that would bypass the legacy regression.

TFRA validation is not transactional; discard output on error. Same-size
manifest replacement skips relocation and is not a table-validation pass.
No reconstruction of already corrupted offsets, new SIDX/CENC/layout support,
general offset-table correction, or uniqueId change is included.
