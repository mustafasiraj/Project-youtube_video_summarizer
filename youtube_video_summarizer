from youtube_transcript_api import YouTubeTranscriptApi
from transformers import pipeline
import re

# Step 1
def get_transcript(video_id):
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id)
        full_text = " ".join([item['text'] for item in transcript])
        return full_text
    except:
        return None

# Step 2
def summarize_text(text):
    summarizer = pipeline("summarization", model="sshleifer/distilbart-cnn-12-6", framework="pt")

    # Split the text into chunks if too long (1000 tokens max per chunk)
    max_chunk_size = 1000
    chunks = [text[i:i+max_chunk_size] for i in range(0, len(text), max_chunk_size)]

    summary = ""
    for chunk in chunks:
        result = summarizer(chunk, max_length=100, min_length=30, do_sample=False)
        summary += result[0]['summary_text'] + '\n'

    return summary

def main():
    url = input("🔗 Paste YouTube video URL or just the video ID: ")

    # Extract video ID from full URL or short link
    match = re.search(r"(?:v=|youtu\.be/)([\w\-]+)", url)
    video_id = match.group(1) if match else url

    transcript = get_transcript(video_id)

    if transcript:
        print("\n📄 Transcript fetched. Summarizing...\n")
        summary = summarize_text(transcript)
        print("✅ Summary of the video:\n")
        print(summary)

        # Save summary to a text file
        with open("video_summary.txt", "w", encoding="utf-8") as file:
            file.write(summary)

        print("\n💾 Summary saved to 'video_summary.txt'")
    else:
        print("❌ Failed to fetch transcript. Maybe captions are not available.")

if __name__ == "__main__":
    main()
