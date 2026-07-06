Normalization-Composition-Artifact
A case study in catching a preprocessing artifact before it became a conclusion.

Summary

I was comparing gene activity across four species split into two groups. A standard normalization step made several categories of genes look clearly different between the groups. The differences were real in the numbers and would have looked publishable. Two of them were artifacts. By re-running the comparison under three normalization methods and tracing the disagreement back to library composition, I separated the findings that were robust from the ones that existed only because of how one normalization method handles its denominator. This is a story about not trusting a single preprocessing choice.

30 seconds of background

RNA sequencing counts how many times each gene's transcript shows up in a sample. Different samples have different total read counts, so you cannot compare raw counts directly, you have to normalize first. The most common method, counts per million (CPM), divides each gene's count by the sample total and scales up. It assumes the sample total is a fair, comparable denominator across your groups. That assumption is exactly where this broke.

The data

4 species, split into two groups, 3 samples each, one tissue type
Aligned to a reference genome (STAR aligner, Ensembl release 115)
Question: do several categories of DNA-repair genes differ in activity between the two groups

The naive result, and why it was tempting

After CPM normalization, several DNA-repair gene categories showed group differences. Most pointed one way, a couple pointed the other, and all of them looked statistically clean. The easy move is to write every result into the paper and move on. The couple that pointed the other way are the ones that turned out to be false.

The catch

CPM divides each gene by the sample total. That is only fair if the total is made of the same kinds of things across groups. It was not.

One group's samples were dominated by noncoding RNA that had nothing to do with the biological question. Noncoding RNA made up roughly 30 percent of that group's libraries versus roughly 18 percent in the other group, and the top five transcripts alone consumed roughly 53 percent of the library in one group versus 40 percent in the other.

When a large, group-specific chunk of the denominator is unrelated to what you are measuring, every other gene's CPM value gets pushed around by the size of that chunk rather than by its own activity. Genes can appear to move up or down purely because the denominator shifted between groups. This is compositional bias. It is the same reason you cannot compare "percent of budget spent on marketing" across two companies when one of them has a huge one-off expense inflating its total.

How I confirmed it

I renormalized the same data three ways:

CPM (total-count scaling, sensitive to composition)
TMM (trimmed mean of M-values, robust to composition)
log2 median-of-ratios (robust to composition)

Then I checked which findings survived the switch.

The categories higher in group one held under all three methods. Robust.
The two categories that had pointed the other way appeared only under CPM and vanished under both composition-robust methods. Artifact.

The tell was that the artifact findings tracked the noncoding RNA load, not the biology. Once I controlled for composition, they disappeared.

Result

Robust findings kept, two artifacts identified and set aside, and a documented reason for each decision. The corrected analysis is more defensible, and the two removed findings would have been a liability if they had gone into the record uncorrected.

The general lesson

Normalization is a modeling assumption, not a formatting step. Any time you divide by a total, whether that is CPM, a percentage, a rate, or anything per-capita, you are trusting that the total means the same thing across your groups. When it does not, you get differences that are real in the arithmetic and false in the world. The cheap insurance is to compute your result under more than one reasonable normalization and only trust what survives all of them.
