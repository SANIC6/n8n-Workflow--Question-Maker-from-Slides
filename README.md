# Slides Question Maker

An n8n workflow that automatically generates quiz questions from Google Slides presentations using AI. Perfect for teachers, trainers, and educators who want to quickly create assessment materials from their lecture slides.

## 🎯 What It Does

This workflow:
1. Takes a Google Slides presentation ID as input
2. Extracts all text content from the slides
3. Uses Google Gemini AI to generate approximately 40 quiz questions
4. Automatically saves questions to a Google Sheets spreadsheet

Each question includes:
- Question text
- One correct answer
- Three incorrect answers (distractors)

## 📋 Prerequisites

- [n8n](https://n8n.io/) installed (self-hosted or cloud)
- Google account with access to:
  - Google Slides API
  - Google Sheets API
- [Google Gemini API](https://ai.google.dev/)(or any other AI provider like ChatGPT) account and API key

## 🚀 Setup Instructions

### 1. Import the Workflow

1. Download or copy the contents of `Question_Maker_From_Slides.json`
2. In n8n, go to **Workflows** → **Import from File** or **Import from URL**
3. Paste the JSON content or select the downloaded file

### 2. Configure Credentials

You'll need to set up three credential connections:

#### Google Slides OAuth2
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable Google Slides API
4. Create OAuth 2.0 credentials
5. In n8n, add the credentials to the "Get slides from a presentation" node

#### Google Gemini API
1. Visit [Google AI Studio](https://ai.google.dev/)
2. Generate an API key
3. In n8n, add the credentials to the "Google Gemini Chat Model" node

#### Google Sheets OAuth2
1. In Google Cloud Console, enable Google Sheets API
2. Use the same OAuth credentials or create new ones
3. In n8n, add the credentials to the "Append row in sheet" node

### 3. Configure the Google Sheets Destination

1. Open the "Append row in sheet" node
2. Click on the `documentId` field and select or enter your Google Sheets spreadsheet ID
3. Ensure your sheet has these column headers in the first row:
   - Question
   - Correct Answer
   - Incorrect Answer 1
   - Incorrect Answer 2
   - Incorrect Answer 3

### 4. Set Your Configuration Values

1. Open the **"Edit Fields"** node
2. Update the three fields with your values:
   - **Slides ID**: Replace `YOUR_SLIDES_PRESENTATION_ID` with your actual Google Slides ID
   - **System Prompt**: Customize the AI instructions (or keep the default)
   - **How many Questions**: Set your desired number of questions (default is 40)

### 5. Activate and Run the Workflow

1. Click the **"Execute workflow"** button in n8n
2. The workflow will process your slides and generate questions automatically

## 📝 How to Use

1. Open the workflow in n8n
2. Click on the **"Edit Fields"** node to configure:
   - **Slides ID**: The ID from your Google Slides URL
     - Example: If your URL is `https://docs.google.com/presentation/d/1abc123xyz/edit`, the ID is `1abc123xyz`
   - **System Prompt**: Custom instructions for the AI
     - Example: "Create questions suitable for high school students" or "Focus on key concepts and definitions"
   - **How many Questions**: Number of questions to generate (default: 40)
3. Click **"Execute workflow"** button in the top right
4. Wait for processing (may take 1-2 minutes depending on slide count)
5. Check your Google Sheets for the generated questions

## 🎨 Customization Options

### Adjust Number of Questions

In the **"Edit Fields"** node, change the "How many Questions" value to your desired number.

### Custom System Prompts

Modify the "System Prompt" field in the **"Edit Fields"** node to guide question generation:
- "Create questions that test critical thinking skills"
- "Focus only on factual recall questions"
- "Generate questions suitable for middle school students"
- "Include questions about historical dates and events"

### Change Output Format

Modify the "Structured Output Parser" node's JSON schema to add or remove answer options, or change the question structure.

## 🏗️ Workflow Architecture

```
Manual Trigger (Execute button)
    ↓
Edit Fields (Set configuration)
    ↓
Get Slides from Presentation
    ↓
Format Slides Text (extract text content)
    ↓
Aggregate (combine all slides)
    ↓
Basic LLM Chain (generate questions with Gemini)
    ↓
Split Out (separate individual questions)
    ↓
Append to Google Sheets
```

## 🐛 Troubleshooting

### "Invalid Presentation ID" Error
- Double-check the presentation ID from the URL
- Ensure the presentation is accessible by the Google account used in credentials
- Make sure the presentation has content on the slides

### No Questions Generated
- Verify your Google Gemini API key is valid and has quota remaining
- Check that slides contain text content (images alone won't work)
- Review the execution log in n8n for specific error messages

### Questions Are Low Quality
- Provide a more detailed system prompt with specific instructions
- Ensure slides have clear, substantial text content
- Try adjusting the number of questions requested

### API Rate Limits
- Google Gemini has rate limits on free tier
- Consider adding a delay between requests for large presentations
- Upgrade to paid tier if processing many presentations


## 📄 License

MIT License - feel free to use and modify for your needs.

## 📧 Support

If you encounter issues:
1. Check the n8n execution logs for detailed error messages
2. Review the troubleshooting section above
3. Open an issue on GitHub with details about your problem
