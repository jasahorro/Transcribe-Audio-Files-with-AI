# Transcribe Audio Files with AI: Cheat Sheet

Quick review notes for the NextWork **Transcribe Audio Files with AI** project.
Full write-up: [README.md](README.md)

**Contents**
1. [S3 buckets](#1-s3-buckets)
2. [Amazon Transcribe](#2-amazon-transcribe)
3. [Language settings](#3-language-settings)
4. [Model types](#4-model-types)
5. [Output data](#5-output-data)
6. [Baseline transcription](#6-baseline-transcription)
7. [Custom vocabulary](#7-custom-vocabulary)
8. [Vocabulary filters](#8-vocabulary-filters)
9. [Speaker partitioning & subtitles](#9-speaker-partitioning--subtitles)
10. [Extra features (Extra for experts)](#10-extra-features-extra-for-experts)
11. [What my SRT taught me](#11-what-my-srt-taught-me)
12. [Quick self-quiz](#quick-self-quiz)

---

## The workflow at a glance

```
🎬 1-min-clip.mp4
      │  upload
      ▼
🪣 S3 bucket            ← stores the media file (Transcribe can't store files)
      │  Transcribe reads it
      ▼
🎙️ Amazon Transcribe    ← language settings + model type → speech becomes text
                           + custom vocabulary (fix words)
                           + vocabulary filter (remove words)
                           + speaker partitioning / subtitles
      │  saves results
      ▼
🪣 S3 bucket (output)   ← default bucket (managed by Transcribe) or my own
```

---

## 1. S3 buckets

- **S3 bucket** = a **folder in the cloud** 📁 for storing files, just like folders on your computer.
- Buckets hold **objects**. An **object** is simply a **file** (in this project, video or audio files).

### Why S3 in this project?
- **Amazon Transcribe converts audio/video to text, but it doesn't store files.**
- So the media file has to be in **S3 first**, before Transcribe can process it.
- The finished transcript also has to be saved somewhere, such as an S3 bucket.

---

## 2. Amazon Transcribe

- **Amazon Transcribe** = an AWS service that converts **speech to text** 🎙️ → 📝
- It uses **machine learning** to understand human language, so it can recognize:
  - different **voices**
  - different **accents**
  - the **context** of what's being said
- It works with many sources: **videos, voice recordings, and live streams**.

---

## 3. Language settings

Language settings tell Transcribe **what language is spoken** in your file.

| Option | When to use it | What happens |
|---|---|---|
| **Specific language** | You **know** the language | Best results ✅ |
| **Automatic language identification** | You **don't know** the language | Transcribe detects the **dominant language** from the **first 3 seconds** of speech |
| **Automatic multiple languages identification** | The file has **more than one language** | AI detects when the language changes and **switches** to transcribing that language |

---

## 4. Model types

- **Model type** = a set of instructions that tells Transcribe **how** to convert speech to text.
- Different models understand different kinds of speech:

| Model | Best for |
|---|---|
| **General model** | Everyday speech (what I used) |
| **Custom model** (you train it) | Conversations with specific **jargon and context**, e.g. **medical** or **legal** |

### Is this AI / machine learning? Yes! 🤖
- A model type represents the **machine learning algorithms and data** Transcribe uses.
- Models learn from **lots of audio samples** to recognize **accents, terminology, and background noise**.
- Picking the right model lets Transcribe tailor its output to your needs and be more accurate.

### 🧠 Extra for experts: training your own model
| Step | Detail |
|---|---|
| Where | Right inside the **Transcribe console** |
| What you upload | **Training data**: 10,000+ words of accurate, relevant transcripts |
| How long | Transcribe takes about **2–3 hours** to train it, and does the heavy lifting for you |

---

## 5. Output data

- **Output data** = **where Transcribe saves** the results of your transcription job.

| Option | Who manages the bucket |
|---|---|
| **Default S3 bucket** | Transcribe creates and manages it for you |
| **Your own S3 bucket** | You choose and manage it |

---

## 6. Baseline transcription

- **Baseline** = the first transcription job, run with **no** custom vocabulary, filters, or extra settings.
- **Why:** it's a **benchmark**. You spot the mistakes first, then measure how much your custom settings improve things.

### What my baseline got wrong
| Problem | Example |
|---|---|
| Technical term not formatted | "403 forbidden" (lowercase) |
| Name not capitalized | "player A" |
| Misspelled word | "repositoriesies" |
| Filler words included | "Um", "Uh-huh" |

**Why:** the general model transcribes everyday speech. It doesn't know your domain's formatting, and it doesn't filter out filler words.

---

## 7. Custom vocabulary

- **Custom vocabulary** = a list of specific words or phrases that helps Transcribe recognize terms it might not normally pick up (jargon, acronyms, brand names, proper nouns).

### The table columns
| Column | What it's for | Still used? |
|---|---|---|
| **Phrase** | The word or phrase you want Transcribe to **recognize** | ✅ Yes |
| **DisplayAs** | How that phrase should **appear** in the transcript (standard or preferred format) | ✅ Yes |
| **SoundsLike** | Used to give pronunciation hints | ❌ No longer supported, ignored |
| **IPA** | Used to give phonetic spelling (International Phonetic Alphabet) | ❌ No longer supported, ignored |

> 🧠 **Extra for experts:** AWS will remove SoundsLike and IPA in the future. Anything you type in them is ignored, so only **Phrase** and **DisplayAs** matter.

### My custom vocabulary
| Phrase | DisplayAs |
|---|---|
| `player` | `Player` |
| `repositoriesies` | `repositories` |
| `four-or-three-forbidden` | `403 Forbidden` |
| `forbidden` | `Forbidden` *(added after the project, see [section 11](#11-what-my-srt-taught-me))* |

---

## 8. Vocabulary filters

- **Vocabulary filter** = a list of words to **block out** of your transcript, such as:
  - 🤬 swearing
  - 🗣️ filler words like "um" or "uh"
  - 🔒 sensitive information you want to keep private
- Filtered words can be **masked** (shown as `***`), **removed**, or **tagged**.

### Custom vocabulary vs vocabulary filter
| | Custom vocabulary | Vocabulary filter |
|---|---|---|
| Purpose | Helps Transcribe **understand and format** specific words | **Blocks** words you don't want in the transcript |
| Effect | Adds or fixes words ➕ | Hides or removes words ➖ |
| Example | `four-or-three-forbidden` → `403 Forbidden` | `um` → `***` |

### Input source
- **Input source** = where your filter comes from. It's usually imported as a **text file**:
  - 💻 uploaded directly from your computer, **or**
  - 🪣 a file stored in an **S3 bucket**

### Filter file format 📝
List **one word per line**:
```
uh
um
like
uh-huh
```

---

## 9. Speaker partitioning & subtitles

| Feature | What it does | Why it's useful |
|---|---|---|
| **Speaker partitioning** | Labels the different speakers, i.e. **who said what** | Makes conversations with several people easier to follow. You can also filter to just one person's lines. |
| **Subtitling** | Creates **time-stamped captions** (SRT/VTT files) for videos | Viewers can read along with the video |

### What an SRT entry looks like
```
3                                  ← caption number
00:00:07,130 --> 00:00:10,729      ← start --> end time (hh:mm:ss,ms)
The specific error is a 403 forbidden response which usually   ← text
```

---

## 10. Extra features (Extra for experts)

| Feature | What it does | Why it matters |
|---|---|---|
| **Toxicity detection** | Finds harmful or inappropriate speech by analyzing **words, tone of voice, and emotion**, then gives it a **toxicity score**. High scores get **flagged and categorized**. | Content moderation |
| **Alternative results** | Gives you **several versions** of the transcript, each with a different **confidence level**, so you can pick the best one. Without it, you only get the **highest-confidence** version. | Useful when accuracy really matters |
| **PHI identification** | Uses AI to flag **Protected Health Information** in transcripts and label its type (financial, health, geographic, biometric) | Helps with **HIPAA** compliance |

### PHI & HIPAA, in more detail
- **HIPAA** = a **US regulation** that protects patient information.
- **PHI (Protected Health Information)** = any information that can **identify a patient**.
- The setting is available because the language is **English (US)**.
- Once you know where PHI is, you can protect those parts with **encryption, access controls, and redaction**, so patient info stays safe even if there's a data breach.

---

## 11. What my SRT taught me

My enhanced job's subtitle file ([`project-files/srtSubtitles.srt`](project-files/srtSubtitles.srt)) showed what worked and what didn't:

| Result | What I saw | Lesson |
|---|---|---|
| ✅ | "Player A" capitalized, "repositories" spelled correctly | The custom vocabulary worked |
| ✅ | A filler word masked as `***` (0:41) | The vocabulary filter worked |
| ❌ | "Uh-huh" still there (0:16) | **Filters match whole words only.** `uh` doesn't catch `uh-huh`, so list every variation. |
| ❌ | "403 forbidden" still lowercase (0:07) | Transcribe likely output the **number** `403`, so the phrase `four-or-three-forbidden` never matched. **Check how Transcribe actually writes a word** before you pick the Phrase. |

> 💡 Filters can also catch words you didn't mean to remove. With `like` in the filter, "I like this" loses the word "like" too.

---

## Quick self-quiz

1. What is an S3 bucket? What's an object? <br>→ A folder in the cloud for storing files. An object is a file inside it.
2. Why do we need S3 if we're using Transcribe? <br>→ Transcribe doesn't store files. The media file must be in S3 first, and the transcript also needs somewhere to be saved.
3. What does Amazon Transcribe do? <br>→ Converts speech to text using machine learning
4. What sources can Transcribe handle? <br>→ Videos, voice recordings, and live streams
5. You know the audio is in English. Which language setting? <br>→ Specific language (best results)
6. You don't know the language. Which setting, and how does it work? <br>→ Automatic language identification. It detects the dominant language from the first 3 seconds of speech.
7. The audio switches between English and Tagalog. Which setting? <br>→ Automatic multiple languages identification
8. What is a model type? <br>→ A set of instructions telling Transcribe how to convert speech to text
9. When would you use a custom model instead of the general one? <br>→ For speech with specific jargon, e.g. medical or legal conversations
10. What do you need to train your own model, and how long does it take? <br>→ 10,000+ words of accurate transcripts, and about 2–3 hours
11. What are your two options for output data? <br>→ A default S3 bucket managed by Transcribe, or your own bucket

12. What is a baseline transcription, and why run one first? <br>→ A job with no custom settings. It's a benchmark for measuring improvements.
13. What do the Phrase and DisplayAs columns do? <br>→ Phrase = the word to recognize. DisplayAs = how it should appear in the transcript.
14. What happens to the SoundsLike and IPA columns? <br>→ They're no longer supported. Anything in them is ignored, and AWS will remove them.
15. Custom vocabulary vs vocabulary filter? <br>→ A custom vocabulary helps Transcribe understand and format words. A filter blocks unwanted words.
16. Give 3 kinds of words you might filter. <br>→ Swearing, filler words (um/uh), sensitive information
17. What's an input source for a vocabulary filter? <br>→ Where the filter comes from: a text file uploaded from your computer or stored in S3
18. Why wasn't "Uh-huh" filtered when `uh` was in my filter? <br>→ Filters match whole words only
19. What does speaker partitioning do? <br>→ Labels who said what
20. What does subtitling produce? <br>→ Time-stamped captions (e.g. an SRT file)
21. How does toxicity detection work? <br>→ It analyzes words, tone and emotion, then gives a toxicity score. High scores get flagged and categorized.
22. What are alternative results? <br>→ Several versions of the transcript with different confidence levels. Without it, you get only the highest-confidence version.
23. What do PHI and HIPAA stand for or mean? <br>→ PHI = Protected Health Information (anything that identifies a patient). HIPAA = the US regulation that protects it.
24. Once PHI is found, how can companies protect it? <br>→ Encryption, access controls, and redaction

---

## Related projects
- [Host a Website on Amazon S3](https://github.com/jasahorro/host-website-s3-bucket) (also uses S3 buckets)
