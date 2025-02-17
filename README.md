[English](https://github.com/RafaelGodoyEbert/Auto-Synced-Translated-Dubs-with-UI/blob/main/README.md) | [Português](https://github.com/RafaelGodoyEbert/Auto-Synced-Translated-Dubs-with-UI/blob/main/README-pt_BR.md)

If you want me to do this for your channel, send me a message

# Auto-Synced Translated Dubs
Automatically translates the text of a video based on a subtitle file, and also uses AI voice to dub the video, while keeping it properly synced to the original video using the subtitle's timings. Now with a UI for easy setup and a more detailed tutorial.

![UI](https://cdn.discordapp.com/attachments/1124221552779612282/1175910532402921472/image.png)
![UI](https://cdn.discordapp.com/attachments/1124221552779612282/1175912731627491389/image.png)

## Update
Added internationalization with i18n (pt_BR, en_US, es_ES)

Added EDGE-TTS for TTS without API

Added BARK-TTS another TTS option without API (but very heavy)

Improved interface

## Google Colab
### Colab Auto-Synced Translated Dubs [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1MNHeuTBe48kKV4Sfk7yM3CDR8LnEy_He?usp=sharing)
- Sorry, I didn't translate Google Colab into English
- Yes, I tried to create a version for Google Colab, but only later realized that Rubberband doesn't have a version for Linux.
- If you have the APIs, the program will probably work very well, as it generates accelerated TTS directly from Azure. (Not sure because I didn't pay for the APIs)
- So on Colab, you can do everything else, except the rubberband function, which is the acceleration and deceleration of audio.

### Other Recommended Colabs
**WHISPER - Generate automatic subtitles to facilitate editing** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1XWig4fk9BN0gwcj9kp3n6yXevAcDLM8j?usp=sharing)
- Sorry, I didn't translate Google Colab into English
- Generate automatic subtitles
 - It generates from audio, so convert your video to mp3 and upload (uploading video to Google Colab is very bad)
 - After generating and downloading, use [Aegisub](https://github.com/Aegisub/Aegisub) (Just open their website and download) to organize the subtitles. Remember, the better and more synchronized the subtitles are, the better your final result will be.

**EDGE-TTS - TTS without API** [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Em_fn0QmN5Bln9uXr4mlnQZLOiG4tO2L?usp=sharing)
- The tutorial below is only if you use the COLAB version, if you use edge within the program, ignore the information below
- Sorry, I didn't translate Google Colab into English
- I recommend using **EDGE-TTS** for TTS because you don't need an API
   - In this Colab, you send the .SRT file, it does the TTS
   - It downloads the ZIP
   - You'll have to extract it to the `workingFolder` folder
   - You will have to leave the "Skip translation," "Skip synthesis," and "Force stretching in the second pass" option enabled ✅
 
## Help the project, translate the UI to your language
- Open the `i18n` folder, make a copy of the English one, rename it to your language.
- Open the `.json` file and translate the right column.
  
### How It Works
If you already have a human-made SRT subtitles file for a video, this program will:
1. Use Google Cloud/DeepL to automatically translate the text and create new translated SRT files.
   - Optional: Use [EDGE-TTS](https://github.com/rany2/edge-tts), no need for an API.
2. Create text-to-speech audio clips of the translated text (using more realistic neural voices).
3. Use the timings of the subtitles to calculate the correct duration of each spoken audio clip.
4. Stretch or shrink the translated audio clip to be exactly the same length as the original speech, and inserted at the same point in the audio. Therefore, the translated speech will remain perfectly in sync with the original video.
    - Optional (Enabled by default): Instead of stretching the audio clips, you can do a second pass to synthesize each clip through the API using the proper speaking speed calculated during the first pass. This greatly improves audio quality.

### Additional Key Features
- Creates translated versions of the SRT subtitle file.
- Batch processing of multiple languages in sequence.
- Config files to save translation, synthesis, and language settings for re-use.
- Included script for adding all language audio tracks to a video file.
   - With the ability to merge a sound effects track into each language track.
- Included script for translating a YouTube video Title and Description to multiple languages.

----

# Instructions

### External Requirements:
- ffmpeg must be installed (https://ffmpeg.org/download.html)
   - Install to PATH for assurance. [RANDOM INTERNET GUIDE](https://academy.streamholics.live/guias/guia-ffmpeg/)
- You'll need the binaries for a program called 'rubberband' (https://breakfastquay.com/rubberband/). Doesn't need to be installed, just put the .exe files and the .dll file in the same directory/folder as the scripts.

## Setup & Configuration
1. Download or clone the repo and install the requirements using `pip install -r requirements.txt`
   - I wrote this using Python 3.9 but it will probably work with earlier versions too
2. Install the programs mentioned in the 'External Requirements' above.
3. Setup your Google Cloud (See Wiki), Microsoft Azure API access and/or DeepL API Token, and set the variables in `cloud_service_settings.ini`. 
   - I recommend Azure for the TTS voice synthesizing because they have newer and better voices in my opinion, and in higher quality (Azure supports sample rate up to 48KHz vs 24KHz with Google). 
   - Google Cloud is faster, cheaper and supports more languages for text translation, but you can also use DeepL.
4. Set up your configuration settings in `config.ini`. The default settings should work in most cases, but read through them especially if you are using Azure for TTS because there are more applicable options you may want to customize.
   - This config includes options such as the ability to skip text translation, setting formats and sample rate, and using two-pass voice synthesizing
5. Finally open `batch.ini` to set the language and voice settings that will be used for each run. 
   - In the top `[SETTINGS]` section you will enter the path to the original video file (used to get the correct audio length), and the original subtitle file path
   - Also you can use the `enabled_languages` variable to list all the languages that will be translated and synthesized at once. The numbers will correspond to the `[LANGUAGE-#]` sections in the same config file. The program will process only the languages listed in this variable.
   - This lets you add as many language presets as you want (such as the preferred voice per language), and can choose which languages you want to use (or not use) for any given run.
   - Make sure to check supported languages and voices for each service in their respective documentation.

## Usage Instructions
- **How to Run:** After configuring the config files, simply run the main.py script using `python main.py` and let it run to completion
   - Resulting translated subtitle files and dubbed audio tracks will be placed in a folder called 'output'
- **Optional:** You can use the separate `TrackAdder.py` script to automatically add the resulting language tracks to an mp4 video file. Requires ffmpeg to be installed.
   - Open the script file with a text editor and change the values in the "User Settings" section at the top.
   - This will label the tracks so the video file is ready to be uploaded to YouTube. HOWEVER, the multiple audio tracks feature is only available to a limited number of channels. You will most likely need to contact YouTube creator support to ask for access, but there is no guarantee they will grant it.
- **Optional:** You can use the separate `TitleTranslator.py` script if uploading to YouTube, which lets you enter a video's Title and Description, and the text will be translated into all the languages enabled in `batch.ini`. They wil be placed together in a single text file in the "output" folder.

## How to add more languages?

- Open the file 'idiomas.ini'
- At the end of the file, duplicate the last language
- Set a new number for your language. Example: 14 <br>

`translation_target_language` = add the language abbreviation, which identifies the subtitle. Example: pt <br>
`synth_language_code` = add language code with country. Example: pt-BR <br>
`synth_voice_name` = add the voice that will be synthetized. Example: pt-BR-FabioNeural <br>
- But **EDGE** is defined in another file, called `'voice_map.py'` <br>
  - The languages that are in 'SUPPORTED_VOICES' will be the voices defined for each language. You can see the list of commented language below <br>
- and **BARK** is defined in `'SRT2Bark.py'` which are inside the SCRIPTS folder <br>
  - At the end of line 12 you define the SPEAKER, you can see all speakers in this [link](https://suno-ai.notion.site/8b8e8749ed514b0cbf3f699013548683?v=bc67cff786b04b50b3ceb756fd05f68c) <br>
  - Test the quality first, as the bark is still in development and may have poor results. <br>
  - Remembering that the BARK option uses 100% of your GPU <br>
  
`synth_voice_gender` = add gender. Example: MALE <br>

- When opening the program again, in batch.ini define the new number (Example: 14) and save
- Enjoy

----

## Additional Notes:
- This works best with subtitles that do not remove gaps between sentences and lines.
- For now the process only assumes there is one speaker. However, if you can make separate SRT files for each speaker, you could generate each TTS track separately using different voices, then combine them afterwards.
- It supports both Google Translate API and DeepL for text translation, and both Google and Azure for Text-To-Speech with neural voices.
- This script was written with my own personal workflow in mind. That is:
    - I use [**OpenAI Whisper**](https://github.com/openai/whisper) to transcribe the videos locally, then use [**Descript**](https://www.descript.com/) to sync that transcription and touch it up with corrections.
    - Then I export the SRT file with Descript, which is ideal because it does not just butt the start and end times of each subtitle line next to each other. This means the resulting dub will preserve the pauses between sentences from the original speech. If you use subtitles from another program, you might find the pauses between lines are too short.
    - The SRT export settings in Descript that seem to work decently for dubbing are *150 max characters per line*, and *1 max line per card*.
- The "Two Pass" synthesizing feature (can be enabled in the config) will drastically improve the quality of the final result, but will require synthesizing each clip twice, therefore doubling any API costs.

## For more information on supported languages by service:
- [Google Cloud Translation Supported Languages](https://cloud.google.com/translate/docs/languages)
- [Google Cloud Text-to-Speech Supported Languages](https://cloud.google.com/text-to-speech/docs/voices)
- [Azure Text-to-Speech Supported Languages](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support#text-to-speech)
- [DeepL Supported Languages](https://www.deepl.com/docs-api/translating-text/request/)

----

### For Result Examples See: [Examples Wiki Page](https://github.com/ThioJoe/Auto-Synced-Translated-Dubs/wiki/Examples)
### For Planned Features See: [Planned Features Wiki Page](https://github.com/ThioJoe/Auto-Synced-Translated-Dubs/wiki/Planned-Features)
### For Google Cloud Project Setup Instructions See: [Instructions Wiki Page](https://github.com/ThioJoe/Auto-Synced-Translated-Dubs/wiki/Instructions:-Obtaining-an-API-Key)
### For Microsoft Azure Setup Instructions See: [Azure Instructions Wiki Page](https://github.com/ThioJoe/Auto-Synced-Translated-Dubs/wiki/Instructions:-Microsoft-Azure-Setup)

