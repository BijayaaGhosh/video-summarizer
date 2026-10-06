# 🎬 AI-Based Video Summarizer

A full-stack web application that automatically generates a **text transcript**, an **AI-powered summary**, and a **highlight video** from any uploaded video file or YouTube URL.

🔗 **Live demo:** [video-summarizer-cyan.vercel.app](https://video-summarizer-cyan.vercel.app)

## ✨ How It Works

Upload a video (MP4, MOV, AVI, MKV, up to 500 MB) or paste a YouTube URL, choose an output language (English or Hindi), and click **Generate Summary**. The app then runs four steps automatically:

**Step 1: Download (YouTube only).** If a YouTube URL is pasted, `yt-dlp` downloads the video to the server using Chrome browser cookies.

**Step 2: Transcription.** FFmpeg extracts the audio and converts it to low-bitrate mono MP3. Because the Groq Whisper API has a 25 MB per-request limit, the audio is split into 4-minute chunks. Each chunk is transcribed with **Whisper Large V3**, then all chunks are combined into one full transcript.

**Step 3: Summarization.** The transcript is sent to a **LLaMA** model on Groq. Summary length adjusts to video duration: 3-4 sentences for short videos, 4-5 for medium, and 6-7 for videos longer than 10 minutes.

**Step 4: Highlight video.** OpenCV scores every frame using three parameters:

- **Motion score:** how much changed between frames
- - **Edge score:** visual detail, using Canny edge detection
  - - **Brightness score:** filters out frames that are too dark or too bright
   
    - The video is divided into equal segments and the highest-scoring frame from each segment is selected. FFmpeg then combines the selected frames with the original audio to produce a **28-second highlight video**, playable and downloadable directly in the browser.
   
    - Results appear one by one as each step finishes, using **Server-Sent Events (SSE)** streaming, so you don't have to wait for everything to complete before seeing output.
   
    - ## 🚀 Key Features
   
    - - Supports video files up to 500 MB
      - - Handles long videos through chunked audio processing
        - - English and Hindi summary output
          - - Real-time streaming output: results appear progressively
            - - YouTube URL input with automatic download
              - - Copy button for transcript and summary
                - - Downloadable highlight video (MP4)
                  - - Mobile-responsive UI with a dark glassmorphism design
                   
                    - ## 🛠️ Tech Stack
                   
                    - | Area | Technology |
                    - |---|---|
                    - | Frontend | React.js |
                    - | Backend | Python, Flask |
                    - | AI | Groq API: Whisper Large V3 (transcription), LLaMA (summarization) |
                    - | Video Processing | OpenCV, FFmpeg |
                    - | YouTube Support | yt-dlp |
                    - | Streaming | Server-Sent Events (SSE) |
                    - | Security | python-dotenv (API key management), werkzeug `secure_filename` (file sanitization) |
                   
                    - ## 📁 Project Structure
                   
                    - ```
                      video-summarizer/
                      ├── frontend/            # React.js app
                      ├── app.py               # Flask backend (API and SSE streaming)
                      ├── video_processor.py   # OpenCV frame scoring and highlight logic
                      └── requirements.txt     # Python dependencies
                      ```

                      ## ⚙️ Getting Started

                      **Prerequisites:** Python 3, Node.js, FFmpeg installed and on your PATH, and a [Groq API key](https://console.groq.com).

                      **1. Clone the repository**

                      ```bash
                      git clone https://github.com/BijayaaGhosh/video-summarizer.git
                      cd video-summarizer
                      ```

                      **2. Add your API key.** Create a `.env` file in the project root:

                      ```
                      GROQ_API_KEY=your_api_key_here
                      ```

                      **3. Start the backend**

                      ```bash
                      pip install -r requirements.txt
                      python app.py
                      ```

                      **4. Start the frontend** (in a second terminal)

                      ```bash
                      cd frontend
                      npm install
                      npm start
                      ```

                      ## 👩‍💻 Author

                      **Bijaya Ghosh** | [LinkedIn](https://www.linkedin.com/in/bijaya-ghoshh) | [GitHub](https://github.com/BijayaaGhosh)
                      
