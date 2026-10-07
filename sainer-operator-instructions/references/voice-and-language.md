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

### Interruption is one switch, and "off" means muted

The app has a single turn-taking switch, **Beller mag onderbreken** (Allow caller
to interrupt). Switching it off does two things at once: the operator finishes its
sentence, and whatever the caller says over it is dropped instead of answered
afterwards. In the config that is `activityHandling: NO_INTERRUPTION` together
with `muteCallerWhileSpeaking: true`. New operators start this way.

Treat it as that one switch, never as two settings:

- **Do not flag the pair as a risk or a warning.** It is the default, and nothing
  in the app can separate the two again.
- **Never propose turning muting off while interruption stays off.** The app
  cannot produce that state, and the user cannot see or undo it there.
- To change it, set `activityHandling` only. Switching interruption off mutes as
  well; switching it on clears the mute keys.
- The one exception is an older operator stored as `NO_INTERRUPTION` without
  muting. There the caller can no longer cut the operator off, but anything said
  over it is still answered once it finishes, which is often the actual
  complaint. Propose `muteCallerWhileSpeaking: true` to complete the pair, and
  say it brings the operator in line with what the switch does today.

Only raise the switch when the complaint is about rhythm. Interruption on suits a
conversation where people interrupt each other normally; off suits a menu, a
legal notice or a noisy line (a workshop, a shop floor, speakerphone in a car),
at the cost that the caller genuinely cannot break in.

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
