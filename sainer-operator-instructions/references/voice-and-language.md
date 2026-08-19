# Vocabulary and language settings

Read this when a name comes out wrong on calls, or when deciding which languages
an operator handles.

---

## Vocabulary

A list of terms with a rough phonetic spelling. The operator uses it as a gentle
hint — for transcription accuracy and for pronunciation — automatically, on
every call.

**This is the only correct home for pronunciation.** Never write a phonetic
respelling into the Instructions, a persona, or any other field: the model reads
prose literally and will say the respelling out as letter-soup. "Sainer, that is
Say-ner" in an instruction produces exactly that, out loud, to a caller.

Writing entries:

- **Term** — the word as written: a brand, a surname, a place, a product.
- **Pronunciation** — a rough phonetic spelling in the operator's own language,
  not IPA. "kvar-teer-makers" is right; `ˈkʋɑr.tir` is not.
- Do not split a respelling across spaces that break the word into separate
  spoken pieces unless that is genuinely how it sounds.

Add a term when it is actually said wrong. A long vocabulary of terms that were
already fine adds nothing and makes the real entries harder to maintain.

**Abbreviations are the exception.** Control those with casing where they
appear, rather than adding them here: a lowercase abbreviation is read as a
word, an uppercased one is spelled out letter by letter. Write `vip` to have it
said as a word, `VIP` to have it spelled V-I-P.

---

## Language settings

**Opening language** — what the operator answers in before it knows anything
about the caller. This is a business decision: the language most callers open
in, not the one most staff speak.

**Supported languages** — the languages the operator may switch to when it hears
them. Leaving this empty disables switching entirely, and the operator answers
everyone in the opening language.

Two things worth checking whenever these change:

- **Transfer destinations may not speak them all.** A caller helped in German
  and then transferred to a Dutch-only desk has been handed a worse experience
  than being told upfront. Set the destination's spoken languages so they get a
  warning first.
- **Instructions are written in English regardless.** The instruction language
  and the spoken language are independent. Adding a language does not mean
  rewriting the Instructions into it — see the main skill guide.

Voice and gender are deliberately left to the user. They are brand decisions
best made by listening to the options, not by reading a description of them.
