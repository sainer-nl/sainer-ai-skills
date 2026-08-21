# Voice, personality, vocabulary and language

Read this when the operator sounds wrong, when a name comes out mangled on
calls, or when deciding which languages an operator handles.

---

## When the operator sounds wrong

"It's too cheerful." "It's too stiff." "It rushes people." These are complaints
about delivery, and the instinct to answer them by rewriting the instructions is
wrong. Adding "be warmer" to a prompt fights the personality the operator was
given; you end up with a longer prompt that still sounds the same.

Three settings own delivery, and they are independent:

- **Personality** decides register and warmth. Formal or informal address,
  brisk or patient, businesslike or friendly.
- **Voice and gender** decide timbre. A voice has its own gender, and pairing a
  female operator with a male voice is jarring, so move both together.
- **Turn-taking** decides rhythm: whether a caller can interrupt, and how long a
  pause has to be before the operator starts talking. "It cuts me off" is
  almost always this and almost never the instructions.

Personality is the user's choice, not yours. Describe the options in terms of
how the operator would come across on a call, say which one you would pick and
why, and let them decide. Never swap a personality silently — the operator's
voice is part of how the business presents itself, and a change nobody asked for
is one they will hear from a customer first.

Nobody can judge a voice or a personality by reading about it. Once a change is
applied, say so and get them to hear it.

Turn-taking is tuned by ear, one setting at a time. There is no configuration
that is right for every caller: making the operator quicker to answer also makes
it quicker to interrupt someone who paused to think. Change one thing, have them
listen, then decide.

### "It keeps getting interrupted" takes two settings, not one

Turning interruption off is only half of it, and the half on its own surprises
people.

It stops the caller from cutting the operator off mid-sentence. It does **not**
stop the operator from *hearing* them: the caller's audio keeps streaming
throughout, so whatever was said over the top is still picked up and answered as
soon as the operator finishes its turn. On a noisy line — a workshop, a shop
floor, someone on speakerphone in a car — that is often the actual complaint,
and switching interruption off alone does not fix it.

The second setting drops the caller's audio while the operator is talking, so
there is nothing waiting to be answered afterwards. It only takes effect when
interruption is already off, so the two belong together: **propose them as one
change, and say what the pair does.**

Both come with a real cost, so say it plainly rather than burying it: the caller
genuinely cannot break in, and anything they say while the operator is talking is
gone. That is the right trade for a menu, a legal notice or a noisy line, and the
wrong one for a conversation where people interrupt each other normally.

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
