# Transcribe Audio Files with AI: Cheat Sheet

Quick review notes for the NextWork **Transcribe Audio Files with AI** project.
Full write-up: [README.md](README.md)

**Contents**
1. [S3 buckets](#1-s3-buckets)
2. [Amazon Transcribe](#2-amazon-transcribe)
3. [Language settings](#3-language-settings)
4. [Model types](#4-model-types)
5. [Output data](#5-output-data)
6. [Quick self-quiz](#quick-self-quiz)

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

---

## Related projects
- [Host a Website on Amazon S3](https://github.com/jasahorro/host-website-s3-bucket) (also uses S3 buckets)
