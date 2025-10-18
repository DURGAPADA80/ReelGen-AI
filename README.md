
# 🎬 **ReelGen AI**

An intelligent Flask-based web app that transforms your uploaded images into stunning **short vertical reels (1080×1920)** with **AI-generated voiceovers** using ElevenLabs.
Upload → Generate → Watch your AI reel come to life! 🤖🎧

---

## ✨ **Key Features**

* 🌐 **Web Interface** — Upload multiple image files and add a short description
* 🗣️ **AI Voiceover** — Generates natural audio using **ElevenLabs TTS** (`audio.mp3`)
* 🎞️ **Automatic Reel Creation** — Combines visuals + audio into a 1080x1920 MP4 using **ffmpeg**
* ⚙️ **Background Processing** — Simple queueing via `done.txt` and a polling worker

---

## 🖼️ **Project Preview**

### 💻 Desktop View

|                        View 1                        |                        View 2                        |                        View 3                        |
| :--------------------------------------------------: | :--------------------------------------------------: | :--------------------------------------------------: |
| ![Desktop Screenshot-1](./images/desktop-view-1.png) | ![Desktop Screenshot-2](./images/desktop-view-2.png) | ![Desktop Screenshot-3](./images/desktop-view-3.png) |

### 📱 Mobile View

|                       View 1                       |                       View 2                       |                       View 3                       |
| :------------------------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
| ![Mobile Screenshot-1](./images/mobile-view-1.png) | ![Mobile Screenshot-2](./images/mobile-view-2.png) | ![Mobile Screenshot-3](./images/mobile-view-3.png) |

---

## 📂 **Repository Structure**

| File/Folder           | Description                                                             |
| --------------------- | ----------------------------------------------------------------------- |
| `main.py`             | Flask web app (upload + gallery) 🧩                                     |
| `generate_process.py` | Background worker (reads uploads, generates audio, runs ffmpeg) ⚙️      |
| `text_to_audio.py`    | ElevenLabs TTS wrapper (`audio.mp3` generator) 🎙️                      |
| `templates/`          | HTML templates (base, index, create, gallery) 🖥️                       |
| `static/`             | Static files (CSS, reels output) 🗂️                                    |
| `user_uploads/`       | Runtime upload folders with `desc.txt`, `input.txt`, and `audio.mp3` 📁 |
| `done.txt`            | Keeps track of processed folders ✅                                      |

---

## ✅ **Requirements**

* Python **3.9+**
* **ffmpeg** installed and available in PATH
* **ElevenLabs API key**

> 💡 *Recommended:* Use a virtual environment to keep dependencies clean.

---

## ⚡ **Quick Setup (Windows)**

1. **Create and activate virtual environment**

   ```bash
   python -m venv venv
   venv\Scripts\activate
   ```

   *(PowerShell)*

   ```bash
   python -m venv venv
   .\venv\Scripts\Activate.ps1
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

   Or manually:

   ```bash
   pip install flask elevenlabs
   ```

3. **Set ElevenLabs API key**

   * CMD:

     ```bash
     set ELEVENLABS_API_KEY=your_api_key_here
     ```
   * PowerShell:

     ```bash
     $env:ELEVENLABS_API_KEY="your_api_key_here"
     ```

4. **Install ffmpeg**

   * Download from [ffmpeg.org](https://ffmpeg.org/) and add to PATH
   * Verify installation:

     ```bash
     ffmpeg -version
     ```

5. **Prepare runtime folders**

   ```bash
   mkdir user_uploads
   mkdir static\reels
   type nul > done.txt
   ```

---

## ▶️ **Run the App**

1. **Start Flask server**

   ```bash
   python main.py
   ```

   Access UI at: **[http://127.0.0.1:5000/](http://127.0.0.1:5000/)** 🖥️

2. **Start background worker**

   ```bash
   python generate_process.py
   ```

   The worker polls `user_uploads/` and processes new folders not listed in `done.txt`. 🔁

---

## 🧭 **How It Works**

1. User uploads images and a short description through `/create`.
2. Flask saves the files in `user_uploads/{uuid}/`:

   * `desc.txt` — text description
   * `input.txt` — ffmpeg concat list
3. The worker:

   * Generates voiceover with **ElevenLabs** (`audio.mp3`)
   * Uses **ffmpeg** to combine visuals + audio → `static/reels/{uuid}.mp4`
   * Updates `done.txt` to mark as complete

---

## 🛠️ **Notes & Tips**

* Upload filenames are sanitized via `secure_filename()`
* ffmpeg uses `-shortest` so video ends with audio
* If processing fails, check:

  * Console logs
  * Existence of `desc.txt` and `input.txt`
  * Correct entries in `done.txt`

---

## 💡 **Future Enhancements**

* 🧾 Validate allowed file extensions before saving
* 🧰 Use `os.makedirs(..., exist_ok=True)` to avoid race conditions
* 🗃️ Replace `done.txt` with a lightweight database or queue
* 🌐 Improve error handling and retries
* 🧩 Add authentication/CSRF protection before public deployment
* ⚡ Optimize reel generation performance

---

## 🩺 **Troubleshooting**

| Issue                                          | Solution                              |
| ---------------------------------------------- | ------------------------------------- |
| `ffmpeg: command not found`                    | Install ffmpeg and add to PATH        |
| ElevenLabs auth error                          | Verify `ELEVENLABS_API_KEY`           |
| `FileNotFoundError` for `desc.txt`/`input.txt` | Ensure uploads completed successfully |

---

## 📜 **License**

No license file included — add your preferred license before publishing.

---

## 🤝 **Contributing**

Contributions are welcome!
Feel free to open issues, submit pull requests, or suggest improvements.
