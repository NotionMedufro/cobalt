# YouTube MP3 Converter API

This document explains how to use our YouTube to MP3 converter endpoint built on the Cobalt API. This service allows you to automatically convert any YouTube video to MP3 format at the lowest possible quality (8 kbps) for maximum bandwidth efficiency.

> [!IMPORTANT]
> This service uses the Cobalt API under the hood. For full documentation on the underlying API, please refer to the [original Cobalt documentation](https://github.com/imputnet/cobalt/blob/main/docs/api.md).
## How to Use the YouTube MP3 Converter

Our service provides a simple URL format that automatically converts a YouTube video to an MP3 file in the lowest available quality (8 kbps):

```
https://notionmedufro.github.io/cobalt/api/{YOUTUBE_VIDEO_ID}
```

When you visit this URL, you'll be automatically redirected to a Cobalt API endpoint with the correct parameters to convert the YouTube video to an MP3 at the lowest quality setting.

### URL Format

Replace `{YOUTUBE_VIDEO_ID}` with the ID of the YouTube video you want to convert. The video ID is the string that appears after `v=` in a YouTube URL.

For example, if the YouTube video URL is:
```
https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

The YouTube video ID is `dQw4w9WgXcQ`, and the conversion URL would be:
```
https://notionmedufro.github.io/cobalt/api/dQw4w9WgXcQ
```
## API Configuration

Our endpoint uses the following Cobalt API configuration to ensure you get the smallest possible MP3 file:

```json
{
  "url": "https://www.youtube.com/watch?v={YOUTUBE_VIDEO_ID}",
  "audioFormat": "mp3",
  "audioBitrate": "8",
  "downloadMode": "audio",
  "disableMetadata": true
}
```

This configuration:
- Extracts only the audio track (`downloadMode: "audio"`)
- Converts it to MP3 format (`audioFormat: "mp3"`)
- Uses the lowest possible bitrate of 8 kbps (`audioBitrate: "8"`)
- Removes metadata to further reduce file size (`disableMetadata: true`)

## Implementation

The implementation uses JavaScript to redirect the user to the Cobalt API with the proper parameters:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Redirecting to MP3...</title>
  <script>
    // Extract YouTube video ID from URL
    const pathParts = window.location.pathname.split('/');
    const videoId = pathParts[pathParts.length - 1];
    
    // Redirect to Cobalt API with low quality MP3 configuration
    if (videoId) {
      const youtubeUrl = `https://www.youtube.com/watch?v=${videoId}`;
      const apiUrl = `https://cobalt.tools/api/json?url=${encodeURIComponent(youtubeUrl)}&audioFormat=mp3&audioBitrate=8&downloadMode=audio&disableMetadata=true`;
      window.location.href = apiUrl;
    } else {
      document.body.innerHTML = '<h1>Error: No YouTube video ID provided</h1>';
    }
  </script>
</head>
<body>
  <p>Redirecting to MP3 download...</p>
</body>
</html>
```
## Examples of Usage

### Direct Link
To create a direct link to convert a YouTube video to MP3:

```
https://notionmedufro.github.io/cobalt/api/dQw4w9WgXcQ
```

### HTML Embed
You can embed a link in your HTML:

```html
<a href="https://notionmedufro.github.io/cobalt/api/dQw4w9WgXcQ">
  Download as MP3
</a>
```

### Use in a Webpage
You can create a simple form that allows users to input YouTube video IDs:

```html
<form onsubmit="convertVideo(event)">
  <label for="videoId">YouTube Video ID:</label>
  <input type="text" id="videoId" name="videoId" placeholder="
