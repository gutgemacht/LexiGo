# 🎓 LexiGo

**A self-hosted, game-like language-learning app (PWA) where code owns the learning and AI only assists.**

LexiGo teaches adults a language through daily *study days* made of short modules: listening, speaking, reading, typing, grammar and vocabulary. A word counts as **learned** only after the learner has heard, spoken, translated and written it correctly on several different days and passed spaced checkpoints. Progress, scheduling and statistics are computed on your own server; the browser is a thin client.

> **Status:** closed beta on a private server behind Tailscale. Course content is draft material awaiting native-speaker review (see [Content status](#-13-content-status)).

---

## 📑 Contents

| # | Section |
|---|---|
| 1 | [Courses and languages](#-1-courses-and-languages) |
| 2 | [Learning methodology](#-2-learning-methodology) |
| 3 | [Daily study day and modules](#-3-daily-study-day-and-modules) |
| 4 | [Grammar](#-4-grammar) |
| 5 | [Vocabulary](#-5-vocabulary) |
| 6 | [Per-word statistics: learned or not](#-6-per-word-statistics-learned-or-not) |
| 7 | [Spoken language and pronunciation](#-7-spoken-language-and-pronunciation) |
| 8 | [Personalised audio](#-8-personalised-audio) |
| 9 | [Colloquial language, slang and culture](#-9-colloquial-language-slang-and-culture) |
| 10 | [Progress, statistics and exams](#-10-progress-statistics-and-exams) |
| 11 | [Personalisation and profile](#-11-personalisation-and-profile) |
| 12 | [Architecture, privacy and security](#-12-architecture-privacy-and-security) |
| 13 | [Content status](#-13-content-status) |
| 14 | [Repository layout](#-14-repository-layout) |

---

## 🌍 1. Courses and languages

| Language | Levels | Highlights |
|---|---|---|
| **German** | A1 – C1 | Most complete: themed modules, 36 A1 grammar topics, exams, texts, dialogues, conjugation |
| **Tagalog** | A1 – B2 | Verb-aspect conjugation, Taglish colloquial block, A1–B2 dialogues and texts |
| **English** | A1 – B1 | Context and listening tasks, irregular verbs with IPA and audio |
| **French** | A1 – A2 | Gender/article data, grammar, dialogues, texts, mini-exams |
| **Russian** | A1 – A2 | Selectable course |
| **Thai** | Alphabet + A1 draft | Alphabet pages, vocabulary and plan pages; engine in progress |

- Every language is an **independent track** per learner: own profile, study day, notebook, statistics, voices.
- The interface and all explanations are **English only**; the learned language is the only other language on screen.
- Language-specific rules live in per-language adapters, so adding a language never touches the learning engine.

---

## 🧠 2. Learning methodology

- **Four kinds of evidence per word:** SPEAK, LISTEN, TRANSLATE, WRITE. Required kinds depend on the part of speech.
- **Mastery states** derived from the event log: `new → met → confirmed → mastered`.
- **Spaced repetition (FSRS):** separate *recognition* and *production* cards for every word.
- **Calendar checkpoints** at day 7, 30, 90 and 180 after introduction measure real retention.
- **Score 0–100:** correct answers add points (capped per day), a mistake **halves** the score and the word returns soon.
- **Weak words:** words with repeated mistakes come back automatically with extra tasks and a generated **memory hint**.
- **Vocabulary containment:** lessons, examples, dialogues, answer options and chat use only words the learner has met plus today's new words.
- **Themed curriculum:** shared themes per CEFR level (starting with "About me"), words ordered by theme and frequency.
- **Placement:** level test plus a *word check* (four meanings and "I don't know", three strikes).
- **Controlled pace:** new words arrive in small batches with a cap on simultaneously active words.
- **Flexible days:** "Start next day now" for motivated learners; unfinished days carry over instead of being lost.

---

## 🗓️ 3. Daily study day and modules

A study day is a fixed list of task blocks; task types rotate daily. Modules can be opened in any order and tasks can be skipped and finished later.

| Module | Skill | What the learner does |
|---|---|---|
| New words | listen, translate | Word card with audio, coloured article (German), example with word-by-word English |
| Review | mixed | Due words, recognition and production tasks |
| Listening (Hörverstehen) | hear | App speaks; learner picks or types what was heard; dictation |
| Speaking (Sprechen) | speak | Say words and phrases, describe pictures; speech-checked |
| Reading (Lesen) | read | Short graded texts (dialogue or prose, two-part lesson texts) with questions |
| Writing / Typing | write | Gap fill, dictation, type the word, **Build a sentence** (tap words into order) |
| Words in context | read | Choose the meaning inside example sentences |
| Grammar | grammar | Template-based exercises on the day's topic |
| Dialogues | speak, read | Pre-generated lesson dialogues filled with the learner's profile |
| Alphabet | read, hear | Letter cards with picture, example word and letter-name audio |
| About me | speak | The learner's own sentences (name, town, job, languages) as daily cards |
| Conjugation | grammar | Verb tables with typed forms (German; Tagalog aspects) |
| Culture cards | culture | Two fact cards per study day |
| Assistant chat | write | Text chat restricted to the learner's vocabulary |

---

## 📐 4. Grammar

- **Grammar catalogue per language and level**, e.g. German A1 36 topics, A2 22; English A1/A2 27/21; French A1/A2 26/21; Tagalog A1 33, A2 16, B1 31, B2 20.
- **Lesson flow:** key words → rule → examples built only from met words → practice → retries → spaced return.
- Topics unlock in learning order and count toward plan completion.
- **Language-specific features:**
  - German: article and gender colouring, cases, modal verbs, comparatives and superlatives, conjugation tables.
  - Tagalog: verb aspects (completed, incompleted, contemplated) for 700+ verbs.
  - French: gender, articles, plural and feminine forms, imparfait and future stems.
- Grammar results feed topic statistics and skill scores.

---

## 📚 5. Vocabulary

- **Lexicons per language and level** with part of speech, gender, articles, plural, verb forms and 2–3 example sentences with English translations and audio (e.g. French A1 820 / A2 780 lexemes).
- **My vocabulary notebook (Vokabelheft):** every met and saved word with notes, practice and per-skill progress.
- **Personal words:** add your own words (slang, jargon) and practise them in tasks.
- **Word info panel:** article, part of speech, level, bold key word in the example with English under each word, full translation, memory hint for weak words.
- **Spelling list** and learning-plan overview per course.
- Lexicon data is imported only with a documented source.

---

## 📊 6. Per-word statistics: learned or not

Every word has its own record, **per learner and language**. Nothing is a stored flag: state is recomputed by replaying the event log, so it is consistent and auditable.

### Tracked per word
| Metric | Meaning |
|---|---|
| State | `new`, `met`, `confirmed` (learned), `mastered`; can fall back after new failures |
| Score 0–100 | +10 per independent correct answer (max 3 a day), +10 per checkpoint, ×0.5 after a mistake |
| Evidence | Which of SPEAK / LISTEN / TRANSLATE / WRITE are passed |
| Study days | Number of different days with a correct answer |
| Checkpoints | Day 7 / 30 / 90 / 180, recognition and production both correct, no mistake that day |
| FSRS cards | Separate recognition and production cards with due dates |
| Mistakes | History and per-skill accuracy |
| Weak mark | 2+ errors in 14 days, cleared after 3 correct answers in a row |

Hints, uncertain speech results and practice repeats never change the score.

### When a word counts as *learned*
1. All required evidence collected.
2. Correct on at least 3 different study days.
3. First checkpoint passed.
4. Score ≥ 50.

*Mastered* additionally needs the half-year checkpoint and score ≥ 80. Learned words stay in review so they are not forgotten.

### What the learner sees
- **Percent-to-learned bar** per word (40 % score, 30 % evidence, 15 % study days, 15 % first checkpoint).
- **Colour stages:** new → red → orange → yellow → lime → **green = learned**.
- **Five skill dots** (listening, speaking, reading, typing, grammar) per word.
- Sorted with weakest words first, then new words, then learned ones; filter by level or saved words.
- Counts per level: **total / seen / learned / unseen** and a **vocabulary growth chart**.
- Personal notes and "save to notebook" on any word.

### Why it matters
Green words are proven across skills and days, not just clicked once, and the planner uses the same data to choose missing evidence, due reviews, weak words and the next batch.

---

## 🎙️ 7. Spoken language and pronunciation

- **Speech output:** cached neural TTS for words, examples, dialogues and personal audio; browser voices as fallback (e.g. Tagalog, Thai, French).
- **Speech check:** the learner records a short clip; Whisper and fast Vosk models compare it with the expected word or phrase. Only a clearly correct result gives SPEAK evidence.
- **Free speech in exams:** transcribed, scored by code, with schema-validated AI observations (words per minute, errors).
- **Per-language model choice:** the Whisper model can be chosen per course language.
- **Privacy:** audio exists only in memory-backed temporary storage and is deleted right after the check.

---

## 🔊 8. Personalised audio

LexiGo builds course audio around the learner's **own profile**, so dialogues sound like their real life.

- **Profile interview:** one question per field in the target language with English translation (name, country, town, age, job, languages, family, hobbies, food). Code checks format; a "check" model verifies the *meaning* of each answer. Nothing is rendered until the profile is ready.
- **Personal dialogue lines:** slots (name, city, job, friend) are filled with the learner's data and rendered as private clips (about 1,300 per learner in Tagalog).
- **Gendered voices:** the learner's lines follow the learner's gender, the best friend's lines follow the friend's.
- **Friends:** a required best friend plus up to five friends, with coloured names in dialogues and texts.
- **About me phrases:** own sentences ("Ich bin …", "Ako si …") as listening, say-aloud and typing cards, plus questions to ask others.
- **Whole course at once:** after the profile is ready, all levels are rendered at night in learning order; a "My course audio" page shows progress.
- **Shared library:** audio independent of the learner is baked once per course and reused.
- **Fallbacks:** missing clips fall back to default lines or a browser voice, so lessons never block.
- **Privacy:** personal clips are private, deleted after 60 unused days, included in export and removed on account deletion.

---

## 🗣️ 9. Colloquial language, slang and culture

- **Cultural fact cards** (two per study day): places, cities, nature, dishes, holidays, phrases, **slang** and idioms, marked for regional or informal use, with openly licensed photos.
- **Taglish block** (Tagalog, from A2): "standard word → how people really say it": clipped forms, forms of address, slang, everyday phrases.
- Colloquial and cultural items do not affect scores or FSRS, so they enrich without punishing mistakes.
- Country facts and real-life context cards per language.

---

## 📈 10. Progress, statistics and exams

- **My progress:** words by state and level, per-skill statistics, study-day history, checkpoints.
- **Lessons table:** every study day with date, finished stages and **time per module**.
- **Time on site:** online versus active time (only real input counts), per day and language.
- **Learning plan:** lessons, grammar topics and exams done, due or ahead.
- **Growth chart:** words seen per day, cumulative.
- **Mini-exams after every 10 lessons:** speaking, listening, reading, writing and grammar, each scored 0–100 by code, results under *My progress → Exams*.
- **Content overview:** words, examples, grammar, dialogues and texts per language and level.
- **Daily badges** and replay of completed modules.
- **Data export**, full account deletion and an **"Error on the page"** report button on every screen.

---

## 👤 11. Personalisation and profile

- **Per-course profile** with the interview described above and an avatar studio.
- **Friends and avatars** appear in dialogues and texts.
- **Per-course interface language** setting and an account default.
- **Own cloud-LLM account** support; system LLMs are configured by role (chat, check, text).
- **Developer tools** for admins: users, AI models, speech models, content, night jobs.

---

## 🏗️ 12. Architecture, privacy and security

### Architecture
- **Client:** React, TypeScript, Vite PWA, Tailwind. Thin: no learning data stored locally, no offline mode.
- **Server:** FastAPI, Pydantic v2, SQLAlchemy 2, Alembic, PostgreSQL, Postgres-backed job queue.
- **Learning engine:** pure Python, event-sourced from an immutable, idempotent log, FSRS (pinned `py-fsrs`).
- **AI:** all LLM, TTS and STT calls go through provider adapters with versioned prompts and JSON-Schema validation. AI returns **observations only**; code decides. Scenes, dialogues and texts are **pre-generated and reviewed**; no local LLM is used.
- **Speech:** TTS and Whisper/Vosk STT, one GPU job at a time.
- **Deployment:** Docker Compose on a dedicated Debian server, reachable only through Tailscale.

### Privacy and security
- Non-superuser database role and forced row-level security on every user table.
- No secrets, free text or profile facts in INFO logs; no personal data in event payloads.
- Speech audio never stored; export and deletion supported.
- No money feature; no data import without a documented source.

---

## 🚧 13. Content status

Dictionaries, dialogues, texts, grammar notes and exams are **drafts**. Native-speaker review is pending for most non-German content, and several levels (A2+ in some languages, Thai, French) are still being built. Linguistic review contributions are welcome.

---

## 🗂️ 14. Repository layout

```
apps/web/            React PWA
packages/contracts/  generated API types
server/app/          learning engine, providers, language adapters, API modules
server/prompts/      versioned prompts and JSON Schemas
content/             curriculum, exams, scenes, texts, culture cards
data/lexicon/        lexicons per language and level
tools/               content and lexicon build and check tools
deploy/              install scripts, Docker Compose, speech services
docs/                specs, ADRs, plans, status
```

Start with `docs/spec/00-index.md` and `docs/status.md`.
