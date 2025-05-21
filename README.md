# Audio Transcription Web App with Whisper & Flask

A Flask-based web application that utilizes OpenAI's Whisper model to transcribe audio files. It provides a user-friendly interface for uploading single or multiple audio files, processing transcriptions with formatted pauses, and allowing users to view or download the resulting text files. The application is designed to be easily run, particularly in environments like Google Colab, using `pyngrok` to expose the local Flask server to the internet.

![image](https://i.ibb.co/k3yG97W/Screenshot-2024-03-15-101908.png)
*(Contoh Antarmuka Pengguna - Ganti dengan screenshot Anda jika perlu)*

## ✨ Features

*   **Audio Upload:** Supports uploading multiple audio files simultaneously.
*   **Whisper Transcription:** Uses the "medium" Whisper model for high-quality transcription.
*   **Automatic Language Detection:** Whisper automatically detects the language of the audio.
*   **Pause Formatting:** Transcriptions are formatted with double line breaks for significant pauses (default > 0.7 seconds).
*   **Web Interface:** Interactive UI built with Flask and vanilla JavaScript.
*   **Public Access via Ngrok:** Exposes the local web server publicly using ngrok, making it accessible from anywhere.
*   **Download & View Transcripts:** Users can directly view transcripts in the browser or download them as `.txt` files.
*   **Processing Queue:** Files are processed one by one, with status updates for each.
*   **Real-time Feedback:**
    *   Displays processing status for each file (processing, success, error).
    *   Shows estimated audio duration (if browser can determine it).
    *   Shows processing time per file and total processing time for the batch.
    *   Indicates the language detected by Whisper for each transcript.
*   **Transcription History:** Maintains a client-side and server-side history of recently processed files.
*   **Service Control:** "Stop Service" button to gracefully shut down the Flask server and ngrok tunnel.
*   **Robust Error Handling:** Provides feedback for common issues (e.g., model loading failure, file errors).

## 🛠️ Prerequisites

*   Python 3.x
*   An [ngrok](https://ngrok.com/) account and an **Auth Token**.
*   (If not using Colab) `ffmpeg` installed and available in your system's PATH. Colab usually has this pre-installed.

## 🚀 Setup & Installation

1.  **Get the Code:**
    Clone this repository or download the Python script.

2.  **Ngrok Authentication Token:**
    This is crucial for `pyngrok` to work reliably.
    *   **If using Google Colab (Recommended for this script):**
        1.  Go to your ngrok dashboard: [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)
        2.  Copy your Authtoken.
        3.  In your Colab notebook, open the "Secrets" tab (key icon on the left panel).
        4.  Add a new secret with the name `MyNGROK` and paste your Authtoken as the value.
        5.  Ensure the script uses `NGROK_AUTH_TOKEN = userdata.get('MyNGROK')`.
    *   **If running locally (and not using Colab secrets):**
        Replace `NGROK_AUTH_TOKEN = userdata.get('MyNGROK')` directly in the script with your token:
        ```python
        NGROK_AUTH_TOKEN = "YOUR_ACTUAL_NGROK_AUTH_TOKEN"
        ```
        **Warning:** Avoid committing your actual token to public repositories.

3.  **Install Required Libraries:**
    If running in Colab, execute the following cell:
    ```python
    !pip install -q openai-whisper ffmpeg numpy scipy noisereduce pydub flask pyngrok
    ```
    If running locally, install them in your Python environment:
    ```bash
    pip install openai-whisper ffmpeg-python numpy scipy noisereduce pydub Flask pyngrok
    ```
    *(Note: `ffmpeg` in the script's pip install is for the Python wrapper, ensure the `ffmpeg` binary is also installed if not on Colab)*

4.  **Run the Script:**
    *   **In Google Colab:** Simply run the Python cell containing the script.
    *   **Locally:** Execute `python your_script_name.py` from your terminal.

## 使い方 (Usage)

1.  After running the script, wait for the ngrok tunnel to be established. The public URL will be printed in the output:
    ```
    ✅ Your application is accessible at: https://xxxx-xx-xxx-xx-xx.ngrok-free.app
    ```
2.  Open this URL in your web browser.
3.  **Upload Audio:**
    *   Click the "Choose Files" button and select one or more audio files.
    *   *(Optional: The HTML contains commented-out code for a language selector if you wish to implement manual language specification instead of auto-detect.)*
4.  **Transcribe:**
    *   Click the "Transcribe" button.
5.  **Monitor Progress:**
    *   The "Processing Status" section will show the progress of each file.
    *   A loader and timers will indicate active processing.
    *   Detected language for each file will be shown upon completion.
6.  **Access Transcripts:**
    *   Once a file is processed, links to "View" and "Download" the transcript will appear in the "Processing Status" and "Transcription History" sections.
7.  **Stop the Service:**
    *   Click the "Stop Service" button in the web interface to shut down the Flask server and ngrok tunnel.
    *   Alternatively, stop the Colab cell or terminate the Python script (Ctrl+C in terminal).

## ⚙️ Configuration & Key Points

*   **Whisper Model:** The script currently uses the `"medium"` model (`whisper.load_model("medium")`). You can change this to other available models (e.g., `"tiny"`, `"base"`, `"small"`, `"large"`) depending on your needs for speed vs. accuracy and available resources.
*   **Pause Threshold:** The `format_transcription_with_pauses` function uses a `pause_threshold` of 0.7 seconds to insert double newlines. This can be adjusted.
*   **File Upload Limit:** The Flask app is configured with a `MAX_CONTENT_LENGTH` of 50MB.
*   **Folders:**
    *   `uploads/`: Stores temporarily uploaded audio files.
    *   `transcripts/`: Stores the generated transcription text files.
    These folders are created automatically.
*   **Error Handling:** The application attempts to handle errors gracefully, such as issues with Whisper model loading or file processing. Check the Colab/terminal output for detailed error messages.
*   **Resource Usage:** The "medium" Whisper model can be resource-intensive. Ensure your environment (especially if free Colab) has sufficient RAM and potentially GPU access for faster processing.
*   **Noise Reduction (Commented Out):** The script includes commented-out imports and potential code lines for `noisereduce`, `scipy.io.wavfile`, and `pydub.AudioSegment`. These were likely intended for audio pre-processing (e.g., noise reduction, format conversion) and can be re-enabled or expanded upon if needed.
*   **Port Clearing:** The `if __name__ == '__main__':` block includes commented-out shell commands (`!lsof ... kill ...`) to attempt clearing port 5000 before starting and after stopping. This can be helpful in environments where the port might not be released immediately.

## 💡 Potential Enhancements

*   Implement the commented-out language selector in the HTML/JavaScript for manual language specification.
*   Integrate the noise reduction capabilities.
*   Allow selection of different Whisper models via the UI.
*   Add more sophisticated audio format handling or conversion.
*   Persist transcription history more permanently (e.g., in a database or local storage).

## 📄 License

This project is open-source. Feel free to use, modify, and distribute. (Consider adding a specific license like MIT if you prefer).
