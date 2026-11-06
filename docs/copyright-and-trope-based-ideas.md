# Copyright and trope-based idea generation

This document explains how copyright law intersects with a tool that classifies a writer's tastes as tropes, finds comparable titles ("comps") by trope overlap, and generates original premises from the result. It is a lay explainer with sources, written for writers and contributors to this repository. It is not legal advice; it is US-centric; and where it names a case or a report, read the primary source before relying on it. Last reviewed 2026-11-05.

## The short version

1. **Nobody owns an idea.** US copyright protects the expression of a story, never its idea, premise, theme, plot shape or tropes. A premise this tool generates is exactly as free as a premise you thought of in the shower, and so is anyone else's identical premise.
2. **What you can protect is the manuscript you write.** The sentences, the specific sequence of incidents, the dialogue and the fully delineated characters are expression and are yours from the moment you fix them in writing.
3. **Using an AI tool to find the premise does not weaken your copyright in the book.** The US Copyright Office's position (January 2025) is that AI used as a tool in the creative process does not bar protection of the human-authored result. What is not protected is text the model generated and you kept verbatim, so write the book yourself.
4. **The risk, such as it is, lives at the comp level, not the trope level.** Sharing many tropes with a published novel is not infringement. Reproducing a specific chain of plot incidents, a distinctive character or passages of prose from that novel can be. Trope-based generation pushes you toward the first and away from the second, which is the point.
5. **The trope vocabulary itself is free to use; TV Tropes' page text is not free to copy.** Trope names and the fact that a given book uses a trope are facts, which copyright does not cover. TV Tropes' descriptions are licensed CC BY-NC-SA 3.0, so this repository writes its own definitions and links to TV Tropes rather than copying from it.

## 1. Ideas, premises and tropes are not copyrightable

The statute says so directly: "In no case does copyright protection for an original work of authorship extend to any idea, procedure, process, system, method of operation, concept, principle, or discovery, regardless of the form in which it is described" ([17 U.S.C. § 102(b)](https://www.law.cornell.edu/uscode/text/17/102)). Courts call this the **idea-expression dichotomy**. The Copyright Office's own circular on what copyright does not protect lists ideas, concepts, plots, themes, names, titles and short phrases ([Circular 33, "Works Not Protected by Copyright"](https://www.copyright.gov/circs/circ33.pdf)).

Three doctrines apply the dichotomy to fiction, and all three describe what this tool traffics in:

- **The abstractions test** (Judge Learned Hand, *Nichols v. Universal Pictures*, 2d Cir. 1930). Every story can be described at many levels of abstraction, from "a love story across a feud" down to its actual scenes and lines. At some level of generality the description is an idea anyone may use. Hand held that a Jewish-Irish intermarriage comedy and another Jewish-Irish intermarriage comedy shared only ideas and "stock characters," and found no infringement. A trope is, by construction, a description at the abstract end of Hand's ladder.
- **Scènes à faire** ("scenes that must be done"). Incidents, characters and settings that flow naturally from a genre or a basic premise are unprotectable. In *Cavalier v. Random House*, 297 F.3d 815 (9th Cir. 2002), the court held that themes of "teaching children to have confidence, to overcome their fears, and to try" were too general to protect, and that standard elements of children's literature were scènes à faire ([summary](https://www.studicata.com/summaries/united-states-court-of-appeals-ninth-circuit/cavalier-v-random-house-inc-2001-76obpd); the Third Circuit restates the rule in [*Nicassio v. Viacom*](https://www2.ca3.uscourts.gov/opinarch/182085np.pdf): situations and incidents that naturally flow from the basic plot premise are not protected). "Scènes à faire" is the legal name for what TV Tropes catalogs.
- **Merger.** Where an idea can be expressed in only one or a few ways, the expression merges with the idea and is not protected either, so that nobody can monopolize the idea by copyrighting its only expression ([overview](https://www.lexplug.com/outlines/intellectual-property/copyright-law/requirements-for-protection/subject-matter-eligibility-102/ideaexpression-dichotomy-102b)).

Implication for this tool: a premise expressed as a bundle of tropes ("a pacifist medic, alone in a robot-run factory, keeping a stranger alive against the machine's triage") is a description at the level the law declines to protect. You cannot infringe anyone's copyright by using it, and you cannot stop anyone else from using it. Originality, for a writer, is therefore never a property of the premise; it is a property of the execution. The law and the craft agree on this.

## 2. What copyright does protect in a novel

Protection attaches to **expression**: the actual text, and the particular selection and arrangement of incidents, dialogue, pacing and detail, plus characters that are "sufficiently delineated" to be more than a type. Courts have protected specific characters as expression when they are developed in detail and consistent across a work or series (the Ninth Circuit's three-part test in *DC Comics v. Towle*, 802 F.3d 1012 (9th Cir. 2015), for the Batmobile; *Salinger v. Colting*, 607 F.3d 68 (2d Cir. 2010), for Holden Caulfield). A "grizzled mentor who dies in act two" is a trope; Obi-Wan Kenobi is expression.

Infringement is judged by **substantial similarity** of protected expression, after filtering out the unprotectable ideas, scènes à faire and public-domain material. The practical gradient:

| Shared with a published work | Status |
|---|---|
| Genre, themes, tone, a trope or a list of tropes | Unprotectable; free |
| A one-sentence premise or logline | Unprotectable idea |
| The same beat-by-beat sequence of specific incidents | Approaches protected expression; risk rises with specificity and length |
| A distinctive, fully developed character with the same name, history and traits | Protected expression; infringement risk |
| Passages of prose, dialogue | Protected expression; infringement |

This gradient is why "filing off the serial numbers" from fanfiction works legally when it is done thoroughly and fails when it is not: the premise and tropes were always free, but a canon character carried over with a new name and the same delineation is still that character. The craft guidance (keep the core conflict; replace names, world, plot events and voice; rebuild each character's drives) is also the legal guidance.

## 3. Does using an AI tool to find the premise affect the copyright in the book?

No, provided you write the book. The US Copyright Office's *Copyright and Artificial Intelligence, Part 2: Copyrightability* (January 29, 2025) concluded that existing law answers the question without new legislation; that using AI as a tool in the creative process does not bar protection of the resulting work; that prompts alone, however detailed, do not make the user the author of the model's output; and that human selection, arrangement and modification of AI output can be protected ([Copyright Office announcement](https://copyright.gov/newsnet/2025/1060.html); [Library of Congress blog summary](https://blogs.loc.gov/copyright/2025/02/inside-the-copyright-offices-report-copyright-and-artificial-intelligence-part-2-copyrightability/); [Sidley summary](https://datamatters.sidley.com/2025/02/07/u-s-copyright-office-issues-report-on-artificial-intelligence-and-copyrightability/)). The report is fact-specific and case-by-case rather than bright-line.

Applied to this tool:

- **The premise the tool outputs** is an idea, so Section 1 already settles it: nobody holds copyright in it, neither you, nor the tool's authors, nor the model provider. The AI-authorship question never arises for the premise because there is nothing protectable to assign.
- **Your manuscript** is human-authored expression and is yours. The provenance of the idea is irrelevant to that; a premise suggested by a friend, a writing prompt, a tarot card or a language model leaves your authorship of the prose untouched.
- **Model-generated prose you keep verbatim** (a logline you paste into the book, a generated scene) is not protected as your work. If it is more than de minimis, the Copyright Office requires you to disclose and disclaim it when registering ([Registration Guidance, March 2023](https://www.federalregister.gov/documents/2023/03/16/2023-05321/copyright-registration-guidance-works-containing-material-generated-by-artificial-intelligence)). The clean practice is to treat everything the tool outputs as notes, and to write the book from a blank page.
- **Outside the US**, most jurisdictions (EU, Japan, South Korea, China per the Office's survey) also require human authorship. The UK has a statutory "computer-generated works" provision (CDPA s.9(3)) whose future is under review; do not rely on it.

## 4. The trope vocabulary and TV Tropes' license

Trope **names** and the **fact** that a work uses a trope are not copyrightable: names and short phrases are excluded ([Circular 33](https://www.copyright.gov/circs/circ33.pdf)), and facts are not protected even when collected with effort (*Feist Publications v. Rural Telephone*, 499 U.S. 340 (1991)). This repository may therefore use trope names as a controlled vocabulary and tag published works with them freely.

TV Tropes' page **text** (descriptions, examples, commentary) is a different matter. The wiki moved from CC BY-SA to **CC BY-NC-SA 3.0** in July 2012 and relicensed older contributions under the new terms ([Wikipedia](https://en.wikipedia.org/wiki/TV_Tropes)); its plagiarism policy distinguishes its contribution license from its usage license and notes that share-alike makes its text incompatible with differently licensed wikis ([TV Tropes Administrivia: Plagiarism](https://tvtropes.org/pmwiki/pmwiki.php/Administrivia/Plagiarism)). Copying TV Tropes descriptions into this repository would require attribution, would bar commercial use of the copied text, and would force the derivative file under the same license. The repository's rules are therefore:

- Use trope names; write our own one-line definitions; link to the TV Tropes page for the full treatment.
- Do not bulk-scrape TV Tropes into the repository. Its Terms of Use on automated access were not verified for this document; a third-party scraper vendor says only that the user must check them ([Apify listing](https://apify.com/scrapersdelight/tvtropes-scraper/Help%20&%20Support)). Fetching a single page to verify a tag is ordinary reading.
- Existing open datasets are available if a bulk corpus is ever needed, under their own licenses: [DBTropes](https://old.datahub.io/dataset/dbtropes) (Linked Data wrapper, CC BY-SA, dump last extracted 2016), [PicTropes](https://arxiv.org/abs/1809.10959) (films, CC BY-SA 3.0), [tropescraper](https://github.com/rhgarcia/tropescraper) (films, 2020), and the raw [TvTroper](https://huggingface.co/datasets/RyokoExtra/TvTroper) page dump. All are film-heavy and years old. [All The Tropes](https://allthetropes.org) is a CC BY-SA fork from before the 2012 relicense.

## 5. Comps

Naming a published work and stating which tropes it uses is fact and commentary, which copyright permits. Quoting a blurb or a passage to illustrate a comp is fair use in ordinary proportion; reproducing a plot summary copied from a copyrighted source is not necessary and should not be done. Titles and series names may be trademarks, which governs how you brand your own book, not what premise you write.

## 6. What this document does not cover

- **Whether the model provider's training was lawful.** That question belongs to the provider and to the ongoing litigation and the Copyright Office's *Part 3: Generative AI Training* (pre-publication version May 2025). It does not change what a writer owns in a manuscript.
- **Community ethics of fanfiction-to-original publishing.** Fandom norms about marketing a book on its fanfic origin are a real concern and a separate one from the law.
- **Contracts.** A publishing or option contract may contain warranties about originality and AI use that are stricter than copyright law. Read them.

## 7. Practical rules for users of this tool

1. Treat every output (profile, comps, premises, loglines) as notes. Write the book from a blank page.
2. When a premise matches one comp closely, change the particulars until the overlap is tropes only: different beat sequence, different character histories, different world.
3. Never carry a character over from a source canon with a new name; rebuild the character from its function.
4. If you keep generated prose in the manuscript, mark it so you can disclose it at registration or rewrite it.
5. Where a contract asks about AI use, answer accurately; this tool is an ideation aid, and you can say so.

## Sources

- [17 U.S.C. § 102(b)](https://www.law.cornell.edu/uscode/text/17/102)
- [US Copyright Office, Circular 33: Works Not Protected by Copyright](https://www.copyright.gov/circs/circ33.pdf)
- [US Copyright Office, Copyright and Artificial Intelligence, Part 2: Copyrightability (Jan 2025)](https://www.copyright.gov/ai/Copyright-and-Artificial-Intelligence-Part-2-Copyrightability-Report.pdf); [announcement](https://copyright.gov/newsnet/2025/1060.html)
- [US Copyright Office, Registration Guidance for works containing AI-generated material (Mar 2023)](https://www.federalregister.gov/documents/2023/03/16/2023-05321/copyright-registration-guidance-works-containing-material-generated-by-artificial-intelligence)
- *Nichols v. Universal Pictures Corp.*, 45 F.2d 119 (2d Cir. 1930)
- *Cavalier v. Random House, Inc.*, 297 F.3d 815 (9th Cir. 2002)
- *Nicassio v. Viacom International*, 3d Cir. 2019 (non-precedential) ([opinion](https://www2.ca3.uscourts.gov/opinarch/182085np.pdf))
- *DC Comics v. Towle*, 802 F.3d 1012 (9th Cir. 2015); *Salinger v. Colting*, 607 F.3d 68 (2d Cir. 2010)
- *Feist Publications, Inc. v. Rural Telephone Service Co.*, 499 U.S. 340 (1991)
- [TV Tropes license history (Wikipedia)](https://en.wikipedia.org/wiki/TV_Tropes); [TV Tropes Administrivia: Plagiarism](https://tvtropes.org/pmwiki/pmwiki.php/Administrivia/Plagiarism)
- Datasets: [DBTropes](https://old.datahub.io/dataset/dbtropes), [PicTropes](https://arxiv.org/abs/1809.10959), [tropescraper](https://github.com/rhgarcia/tropescraper), [TvTroper](https://huggingface.co/datasets/RyokoExtra/TvTroper)
