I've been frustrated by trying to fix problems in [nuweb](https://sourceforge.net/projects/nuweb/). It can be really hard to work out how to make a change - for example, how to
fix [bug 2965157][2965157] - if it is indeed a bug!

[2965157]: https://sourceforge.net/tracker/?func=detail&aid=2965157&group_id=7449&atid=107449

Part of this is down to the fact that nuweb is implemented in C, but I think that mostly it's because we developers haven't treated it as a proper Literate Programming project. There ought to be some explanation of why (referring to [bug 2965157][2965157] again) there's a scrap reference inside the parameter list. I think (from discussions on the nuweb-users mailing list) that it was deliberate, but I see no clue as to the intention.

As another problem (far from the only one!), what is a block comment?

Anyway, to scratch this itch I've been reworking nuweb in Python.

So far, the parts implemented are:

* files (`@o` and `@O`, but no flags)
* fragments (`@d` and `@D`, but no flags)
* scraps (delimited by `@{` `@}` only)
* user-defined identifiers
* old-style fragment parameters
* indices `@f`, `@m` and `@u` (I've laid `@u` out a little differently)
* `@%` (anywhere in the document, not just in scraps)
* `@#` (put code line at left margin)
* `@@` handling (this one was tricky, and I may not have caught all the cases)
* switch `-r` (generate hyperlinks), aliased `--hyperlinks`.

It won't handle the current `nuweb.w`, because that web uses new-style parameters. However, it will process it and generate LaTeX. `nuweb.w` reveals one shortcoming in the Python version, which is that it's slow at processing user-defined identifiers. It takes 8s on this Macbook Pro to process `nuweb.w`, against 0.07s for the C version. The fix for this (if it's worth it; the other webs I have take about a second overall, which isn't very painful) may be to implement the [string search](https://en.wikipedia.org/wiki/Aho–Corasick_string_matching_algorithm) which `nuweb.w` uses, though a quick trial of [acora](https://pypi.python.org/pypi/acora) suggests that a pure Python implementation will be slower than the current regular expression-based implementation in `nuweb.py`.

So why haven't I written `nuweb.py` as a web? I suppose the answer is, that I've been exploring the problem while writing the code, which doesn't seem to be a very 'literate' approach. Now that the overall structure is reasonably clear, maybe it can be webified. That would at least force me to provide some user-oriented documentation!
