# Edge AI Vision Assistant

An offline-first, AI-powered assistant for Android built with Flutter. A multimodal Gemma model runs **directly on the phone**, with no cloud AI calls, to describe what the camera sees, chat about images, and act as an agent that can manage Google Calendar and read Gmail.

Built during the **"No Signal, No Problem"** Local AI workshop by GDG on Campus APU (Asia Pacific University).

## Features

- **On-device LLM:** runs Google's Gemma 4 E2B (LiteRT `.litertlm` format) locally through `flutter_gemma`, so prompts and images never leave the device.
- **Camera scene description:** detects objects in a photo with an on-device object detector, then asks Gemma to describe the scene, read visible text and point out useful details.
- **Multimodal chat:** chat with the model by text or voice, and attach an image.
- **Agentic tools (skills):** the model can call Dart tools defined by `SKILL.md` files:
  - `list-events`: list Google Calendar events for a date
  - `create-calendar-event`: create a new calendar event
  - `read-latest-email`: read the latest Gmail message (read-only)
  - `kitchen-adventure`: a sample bundled skill
- **Voice in and out:** speech-to-text for input and text-to-speech that reads replies aloud, for accessibility.
- **Google Sign-In** with scoped OAuth access (Calendar events, Gmail read-only).

## Tech stack

| Area | Tools |
| --- | --- |
| App | Flutter, Dart |
| On-device AI | `flutter_gemma`, `flutter_gemma_litertlm`, `flutter_gemma_agent` (Gemma 4 E2B) |
| Vision | `object_detection` (OpenCV via `dartcv4`), `camera` |
| Voice | `flutter_tts`, speech recognition |
| Google APIs | `google_sign_in`, `googleapis` (Calendar v3, Gmail v1) |
| Config | `flutter_dotenv` |

## How it works

1. On first launch, the app signs in to Google (optional), downloads the Gemma model from Hugging Face once, and loads it on the device CPU.
2. Skills are loaded from `assets/skills/*/SKILL.md`. Each file tells the model when and how to call a matching Dart tool in `lib/tool_handlers.dart`.
3. Camera photos go through the object detector first. Its results are added to the prompt so the model gives a more accurate description.
4. Replies stream back token by token and are read aloud.

## Running it

**Requirements:** a physical Android device running Android 11+ (API 30) with about 6 GB of RAM or more, the Flutter SDK, and Android Studio with NDK `28.2.13676358`. Web and desktop builds are not supported.

1. Create a Google Cloud project, enable the **Google Calendar API** and **Gmail API**, configure the OAuth consent screen, and create:
   - an **Android** OAuth client (package `com.gdgapu.visual_assistant` plus your SHA-1)
   - a **Web** OAuth client, whose ID goes in `.env`
2. Create a Hugging Face read token.
3. Copy `.env.example` to `.env` and fill in:
   ```
   HUGGINGFACE_TOKEN=hf_...
   GOOGLE_SERVER_CLIENT_ID=...apps.googleusercontent.com
   ```
4. Run:
   ```
   flutter pub get
   flutter run
   ```

## Known limitations

- There is no cloud fallback, so devices with low RAM or old hardware may fail to load the model.
- Calendar and Gmail features need an internet connection, even though the AI runs locally.
- `flutter_gemma_agent` is pre-1.0 and pinned to `0.2.5`.

## Credits

Workshop and starter template by Samantha / GDG on Campus APU ([original repo](https://github.com/helloSamTee/GDGAPU-EdgeAI-Virtual-Assistive)). Workshop exercises completed by [MAS-07T](https://github.com/MAS-07T).
