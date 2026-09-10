# LaTeX paper rules

Follow these in full, without being asked, in every task that writes or changes a LaTeX
manuscript. Where the target venue ships a class file or an author kit, the venue's
requirement wins over any rule here.

Project-specific facts — the venue, the build command, the notation source of truth —
belong under a `## Project` heading appended to the end of this file, never mixed into
the rules above it.

## Toolchain

- Build with the current TeX Live release. Do not work against a distribution more than one
  release behind, and update packages with `tlmgr update --self --all` before diagnosing any
  build failure that looks like a package bug.
- Prefer the modern engine: LuaLaTeX first, XeLaTeX second, pdfLaTeX only when something
  requires it. A venue class that loads its fonts through the classic Type 1 mechanism is
  exactly such a requirement, and switching engines under one silently substitutes fonts and
  changes the page count.
- The engine choice is a decision, not a default. State it in the build config with the
  reason, so nobody 'modernises' it back and reflows the paper.
- Drive every build through `latexmk`. Never chain compiler and bibliography runs by hand.
- Build into a separate output directory. Keep generated files out of version control, with
  the single exception of a deliberately tagged submission PDF.
- Never edit a generated file. Fix the source that produced it.

## Layout

- `main.tex` holds document wiring only: class, preamble includes, chapter includes. No prose.
- Split the preamble into packages, macros, and title metadata. Front matter must be
  reworkable without touching the wiring.
- One file per section or chapter, named for its content.
- Vendor the venue's class file into the project and pin the search path to it. A class the
  distribution also ships is a version conflict waiting to change your layout.

## Packages

- Load order matters. `hyperref` goes late, `cleveref` after `hyperref`, and anything that
  patches references after both.
- Load `microtype`. It is the cheapest quality gain available.
- Use `siunitx` for every number carrying a unit or an uncertainty, `booktabs` for every
  table, `graphicx` with a configured `\graphicspath`.
- Never load a superseded package: use `subfig` or `subcaption` over `subfigure`, `graphicx`
  over `epsfig`, `amsmath` over the plain-TeX forms, and a font package over `times`.
- Do not redefine a core command or patch a class internal unless there is no alternative.
  When you must, comment why in the line above.

## Mathematics

- Every symbol used more than once gets a macro in the notation file. Renaming a quantity
  must be one edit, never a search across chapters.
- Declare operators with `\DeclareMathOperator`, never as upright text inside math.
- Use `amsmath` environments. `eqnarray` is broken and forbidden; use `align`, `gather`, or
  `equation` with `aligned`.
- Number an equation only if something references it. Use `\nonumber` or the starred form
  otherwise.
- Punctuate displayed equations as part of the sentence containing them.
- Define every symbol in prose at its first use. A reader must never meet an undefined glyph.
- Size delimiters with `\left`/`\right` or a fixed size, never a bare bracket around a
  fraction or a stacked construct.

## Floats and tables

- Tables use `booktabs` rules only. No vertical rules, no `\hline`.
- Figures are vector where the source allows it. Raster only for photographs and rendered
  scenes, at a resolution that holds up in print.
- A figure caption sits below the figure, a table caption above the table.
- Never force placement with `[h]`. Let LaTeX float, and fix the surrounding text if the
  result is wrong.
- Every float is referenced from the text before it appears.

## References and citations

- Label immediately after the caption or the sectioning command, with a type prefix:
  `fig:`, `tab:`, `eq:`, `sec:`, `alg:`.
- Reference through `cleveref` (`\cref`, `\Cref`). Do not hand-write 'Figure~\ref{...}',
  which drifts from the venue's style and from itself.
- Take every bibliography entry from the publisher's or DBLP's official record. Do not
  hand-type one, and do not trust a search engine's citation box.
- Keep bibliography keys in one stable scheme. Never renumber or reorder by hand.
- Cite the published version when one exists, not the preprint.

## Prose

- One sentence per source line, or one clause for a long sentence. It makes a diff readable
  and a review comment addressable.
- Expand every acronym at first use, once, and never again.
- Use a non-breaking tie before a reference and inside a name: `Fig.~\ref{}`, `Alg.~1`.
- Write what the work does, not what it will do. Reserve the future tense for future work.
- Do not use `\\` to break a line in running prose. It is for tabular and verse only.
- State a claim or drop it. Hedged filler such as 'very', 'quite', and 'it is worth noting
  that' costs space and adds nothing.

## Drafting and numbers

- Every number in the text or a table comes from a logged result or a generating script,
  never typed from memory. A table that a script can emit should be emitted by that script.
- Mark every gap with a `\todo` macro that is switched off for submission, so an unfinished
  passage cannot reach a reviewer silently. Make it robust: running heads and captions are
  case-folded at shipout and will mangle a fragile definition.
- Remove the draft switch's effect before building a submission PDF, and check that the page
  count still meets the limit afterwards.

## Self-review before reporting done

- Build the document and read the log. Report undefined references, undefined citations,
  multiply-defined labels, and overfull boxes. Do not report a task complete over a build
  that emits any of them.
- Re-read every file you changed and fix what breaks these rules.
- Check that any symbol you introduced has a macro and a prose definition, and that any
  float you added is referenced and captioned.
- Leave untouched sections untouched. A problem elsewhere in the manuscript is something to
  report to the user, not to rewrite uninvited.
