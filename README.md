# 🎬 AI-Based Video Summarizer

A full-stack web application that automatically generates a text transcript**, an **AI-powered summary, and a highlight video from any uploaded video file or YouTube URL.



## ✨ How It Works

Upload a video (MP4, MOV, AVI, MKV, up to 500 MB) or paste a YouTube URL, choose an output language (English or Hindi), and click Generate Summary. The app then runs four steps automatically:

Step 1: Download (YouTube only).If a YouTube URL is pasted, `yt-dlp` downloads the video to the server using Chrome browser cookies.

Step 2: Transcription. FFmpeg extracts the audio and converts it to low-bitrate mono MP3. Because the Groq Whisper API has a 25 MB per-request limit, the audio is split into 4-minute chunks. Each chunk is transcribed with Whisper Large V3**, then all chunks are combined into one full transcript.

Step 3: Summarization. The transcript is sent to a LLaMA model on Groq. Summary length adjusts to video duration: 3-4 sentences for short videos, 4-5 for medium, and 6-7 for videos longer than 10 minutes.

Step 4: Highlight video.** OpenCV scores every frame using three parameters:

- Motion score: how much changed between frames
- Edge score: visual detail, using Canny edge detection
- Brightness score: filters out frames that are too dark or too bright
   
  The video is divided into equal segments and the highest-scoring frame from each segment is selected. FFmpeg then combines the selected frames with the original audio to produce a 28-second highlight video**, playable and downloadable directly in the browser.
   
- Results appear one by one as each step finishes, using Server-Sent Events (SSE)** streaming, so you don't have to wait for everything to complete before seeing output.

- Key Features
   
- Supports video files up to 500 MB
- Handles long videos through chunked audio processing
- English and Hindi summary output
- Real-time streaming output: results appear progressively
- YouTube URL input with automatic download
- Copy button for transcript and summary
- Downloadable highlight video (MP4)
- Mobile-responsive UI with a dark glassmorphism design
                   
-  Tech Stack
                   
                  
 - | Frontend | React.js |
 - | Backend | Python, Flask |
 - | AI | Groq API: Whisper Large V3 (transcription), LLaMA (summarization) |
 - | Video Processing | OpenCV, FFmpeg |
 - | YouTube Support | yt-dlp |

 
                   
                  
