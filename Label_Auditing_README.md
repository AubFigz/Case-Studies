Most of the dataset wasn't what its label said

A case study in auditing labels before trusting them.

Summary

I was handed a labeled panel of about 36 cell lines, most of them tagged as patient-derived
tumor lines. Before using them for anything, I checked whether each line actually was what
its label claimed. I built a classifier from 63 cell-identity markers and scored every line
against known reference identities. About 24 of 25 patient-derived lines scored as
fibroblast or stromal, not the tumor-epithelial type everyone assumed. A separate
subtype-validation step reproduced known references at 91 percent concordance, flagged one
reference line as anomalous, and traced an implausibly high subtype call rate to a
normalization artifact. This is a story about validating your labels before you model on
them.

30 seconds of background

A cell line is supposed to carry a stable identity: the tissue and cell type it came from.
In practice, lines get mislabeled, cross-contaminated, or slowly overtaken by a faster
growing cell type, so a culture that is filed as tumor tissue can quietly become something
else. If you draw conclusions from mislabeled samples, every downstream result inherits the
error. In data-science terms, label integrity is a precondition, not an afterthought, and
almost nobody checks a label that looks fine on paper.

The data


About 36 cell lines: a set of patient-derived lines plus established reference lines with
known, published identities
A gene-expression profile for each line
The assumption baked into the panel: the patient-derived lines are tumor epithelial


The assumption, and why it was tempting

The panel arrived pre-labeled from a credible source, and the labels were plausible. The
easy path is to accept them and move straight to the interesting analysis. Re-checking a
label that looks correct feels like wasted effort right up until it is the only thing that
mattered.

The catch

I scored each line with a 63-marker composite classifier, using markers whose expression
cleanly separates cell identities (epithelial, fibroblast and stromal, and so on). About 24
of 25 patient-derived lines scored as fibroblast or stromal rather than epithelial. That is
not a subtle shift. It means most of the "tumor" panel was, by its own expression identity,
a different kind of cell. Any tumor-biology conclusion drawn from that panel would in fact
have been describing fibroblasts.

How I confirmed it

I did not want to overturn a whole panel's labels on a hunch, so I used three checks.

A composite, not a single marker. Any one gene can mislead. A 63-marker composite is
robust to any single marker misbehaving, and the fibroblast calls were consistent across
the whole marker set rather than driven by one or two genes.

Positive controls. I ran the same classifier on established reference lines whose
identities are known and reproduced the expected calls. That confirms the classifier works
and that the fibroblast calls are not a pipeline bug.

An independent second opinion. A separate intrinsic-subtyping method (47 of 50 required
genes present) reproduced reference subtypes at 91 percent concordance and flagged one
reference line as an anomaly worth follow-up. When I folded in an external vendor's subtype
calls for the same samples, they showed an 86 percent rate for a single subtype, which is
implausibly high. Tracing it showed the vendor calls came through a normalization that
inflated that subtype, the same class of composition problem from the normalization case
study. I flagged it rather than passing it through.

Result

A clear map of which lines are actually what, one documented anomaly, and a caught
normalization artifact in the external calls. The practical upshot is direct: this panel
cannot be treated as a tumor-epithelial dataset until identity is addressed first, and
anyone who skipped that step would have published conclusions about the wrong cells.

The general lesson

Before you model, audit your labels with something orthogonal to how the labels were
assigned. A classifier makes an excellent label auditor if you build it from features the
label did not come from, validate it on examples whose truth you already know, and use a
composite rather than any single feature so no one variable can fool you. When most of your
labeled examples turn out to be a different class, that is not a footnote, it rewrites every
conclusion that comes after it. The audit is cheap. Skipping it taxes every result
downstream.
