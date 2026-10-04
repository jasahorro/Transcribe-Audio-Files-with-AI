<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Transcribe Audio Files with AI

**Project Link:** [View Project](https://nextwork.ai/projects/1542c25d-ea3b-5dc5-87fc-a77416aabb87)

**Author:** Maria Jasmin Ahorro  
**Email:** jasminahorro23@gmail.com

---

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_o2p3q4r1)

## Introducing Today's Project!

In this project, I will demonstrate how to automatically convert video and audio files into accurate, clean text using Amazon Transcribe and Amazon S3. I'm doing this project to learn how to store media assets in the cloud, configure custom vocabularies to handle specialized technical terms and jargon, apply vocabulary filters to remove filler words, and utilize features like speaker identification and subtitling.

### Tools and concepts

Services I used were Amazon S3 to store media files in the cloud and Amazon Transcribe to convert spoken audio into text. Key concepts I learnt include running a baseline transcription as a performance benchmark, using a custom vocabulary to specify domain-specific terms and correct formatting (such as mapping four-or-three-forbidden to 403 Forbidden), configuring vocabulary filters to automatically remove unwanted filler words like "um" and "uh", and applying speaker partitioning and subtitling to enhance transcript clarity.

### Project reflection

This project took me approximately 30 minutes to complete. The most challenging part was configuring the custom vocabulary parameters without hitting syntax errors, particularly ensuring proper phrase structures and formatting. It was most rewarding to see the enhanced transcription successfully apply my custom vocabulary mappings and eliminate filler words to create a clean, accurate final transcript.

I chose to do this project today because I wanted to gain hands-on experience using cloud AI services like Amazon Transcribe and Amazon S3 to build automated, accurate speech-to-text workflows. Something that would make learning with NextWork even better is having more real-world, interactive troubleshooting scenarios or live sample datasets to practice edge-case configurations.

## S3 and Transcribe

To set up for this project, I'm using an S3 bucket to store my media files because Amazon Transcribe requires the video or audio file to be located in cloud storage before it can process and transcribe it. The file I'm transcribing is 1-min-clip.mp4.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_k1l4m7n0)

## Run A Transcription Job

The steps to run a transcription job include navigating to the Amazon Transcribe console, selecting Create job, specifying the language settings and model type (such as the General model), browsing S3 to select the input audio/video file (e.g., 1-min-clip.mp4), choosing the output location, and reviewing optional settings before submitting the job. Overall, this process took me about 2 to 3 minutes to set up and complete.

Amazon Transcribe uses model types to determine how machine learning algorithms interpret and convert specialized audio patterns into text. Use cases of model types include using general models for everyday speech, media broadcasts, or customer service calls, and training custom language models for domain-specific contexts containing specialized jargon, such as medical transcriptions or legal proceedings.

You can customise a transcription further with subtitling, which adds time-stamped captions to video files so viewers can follow along with visual text, and speaker partitioning, which helps to identify and label different speakers in an audio file so you can clearly see who spoke each line.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_g0h1i2j3)

## Baseline Transcript Review

To start using Amazon Transcribe, I first ran a baseline transcription job, which means running the initial transcription without applying any custom settings, custom vocabularies, or filters. This is because it creates a clean benchmark that allows me to identify inaccuracies and measure improvements after applying custom settings later.

While reviewing the baseline transcript, I noticed several inaccuracies and unnecessary filler words. Specifically, technical terms like "403 forbidden" were written in lowercase, "player A" was not capitalized as a proper name, "repositories" was misspelled as "repositoriesies", and filler words like "Um" and "Uh-huh" were included. These happened because the default general model transcribes speech strictly based on standard conversational audio without domain-specific formatting or automatic filler filtering.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_s3t6u9v2)

## Custom Vocabulary

I can resolve transcription inaccuracies using a custom vocabulary, which is a customized list of specific terms, acronyms, brand names, or proper nouns paired with their intended display formats. A custom vocabulary improves transcription accuracy by providing domain-specific context to Amazon Transcribe's machine learning model, ensuring that specialized jargon, technical terminology, and distinct casing preferences are correctly recognized and formatted in the output text.

To create an item in a custom vocabulary, you need to define two values. They are Phrase and DisplayAs. The Phrase is the exact word or mis-transcribed pattern you want Transcribe to look for, while DisplayAs tells Transcribe how that word should actually appear in the final transcription output.

My custom vocabulary defines specific rules for domain-specific words, acronyms, and proper formatting so Amazon Transcribe can correctly render terms that were mis-transcribed during the baseline job. It maps the lowercase word "player" to "Player" to capitalize it as a proper title, corrects spoken repetition by changing "repositoriesies" to "repositories", and converts the spoken phrase "four-or-three-forbidden" into the properly formatted HTTP status code "403 Forbidden".

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_e3f4g5h6)

## Vocabulary Filters

Another feature in Transcribe is vocabulary filtering, which allows you to define a list of unwanted words—such as profanity, filler words, or sensitive information—that should be masked, removed, or tagged in your transcription output. It's different from custom vocabularies because while custom vocabularies are used to teach Transcribe how to recognize and properly format specific or domain-specific terms, vocabulary filters are designed to suppress or remove unwanted content from the final transcript.

My vocabulary filter removes unwanted speech hesitations and filler words, specifically "um" and "uh", from the transcribed text. To set up this filter, I first created a text file containing the list of words I wanted to filter out, saved it locally, and then uploaded it to Amazon Transcribe under the Vocabulary Filtering section to define the suppression rules.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_u7v8w9x0)

## Enhanced Transcription

### I ran a new transcription with my custom vocabulary and filtering settings

The enhanced transcription is better than the baseline because it fixes domain-specific jargon and removes unwanted noise from the final text. Specifically, it uses a custom vocabulary to correctly capitalize terms like "Player" and accurately format technical concepts like "403 Forbidden," while applying a vocabulary filter to mask or remove filler words like "um" and "uh," resulting in a cleaner and more professional output.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/1542c25d-ea3b-5dc5-87fc-a77416aabb87_k1l2m3n4)

## Real Time Transcription

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/1542c25d-ea3b-5dc5-87fc-a77416aabb87)*
