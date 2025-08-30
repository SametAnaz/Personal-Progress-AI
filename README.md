# 🤖 PersonalProgressAI (PPAI)

A comprehensive personal progress tracking system built with n8n that uses AI to monitor daily habits, physical measurements, and provides intelligent chat-based interaction through Telegram.

## 🚀 Quick Setup Guide

### Step 1: Import the Workflow to Your n8n Instance

1. **Download the Workflow File**
   - Download the `PersonalProgressAI-Samet.json` file from this repository

2. **Import to n8n**
   - Open your n8n instance (cloud or self-hosted)
   - Click on **"Workflows"** in the left sidebar
   - Click **"Import from file"** or **"+"** → **"Import from file"**
   - Select the downloaded `PersonalProgressAI-Samet.json` file
   - Click **"Import"**

3. **Alternative: Import via URL** (if using n8n cloud)
   ```
   https://raw.githubusercontent.com/<YOUR_GITHUB_USERNAME>/n8n-PersonalProgressAi/main/PersonalProgressAI-Samet.json
   ```

### Step 2: Required Services & API Keys

Before activating the workflow, you'll need to set up these services:

#### 🤖 **Telegram Bot** (Required)
1. Message [@BotFather](https://t.me/botfather) on Telegram
2. Send `/newbot` and follow instructions
3. Save your **Bot Token** (looks like: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)
4. Get your **Chat ID**:
   - Send a message to your bot
   - Visit: `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
   - Find your chat ID in the response

#### 🧠 **Google Gemini API** (Required)
1. Go to [Google AI Studio](https://aistudio.google.com/)
2. Create a new API key
3. Save your **Gemini API Key**

#### 🗄️ **MySQL Database** (Required)
- You can use any MySQL hosting service (AWS RDS, Google Cloud SQL, DigitalOcean, etc.)
- Or set up a local MySQL instance
- Note down: **Host, Port, Username, Password, Database Name**

#### ⚡ **Groq API** (Optional - for faster AI responses)
1. Sign up at [Groq Console](https://console.groq.com/)
2. Generate an API key
3. Save your **Groq API Key**

#### 📅 **Google Calendar API** (Optional)
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Enable Google Calendar API
3. Create OAuth2 credentials
4. Save your credentials

### Step 3: Configure Credentials in n8n

1. **Open the imported workflow** in n8n
2. **Set up credentials** for each service:

#### Telegram Bot Setup:
   - Click on any Telegram node (red error indicator)
   - Click **"Create New Credential"**
   - Enter your **Bot Token**
   - Name it (e.g., "My Telegram Bot")
   - **Test & Save**

#### Google Gemini Setup:
   - Click on "Google Gemini Chat Model" node
   - Click **"Create New Credential"**
   - Enter your **Gemini API Key**
   - **Test & Save**

#### MySQL Database Setup:
   - Click on any MySQL node
   - Click **"Create New Credential"**
   - Enter your database connection details:
     - Host: `your-db-host.com`
     - Port: `3306`
     - Database: `your_database_name`
     - Username: `your_username`
     - Password: `your_password`
   - **Test & Save**

### Step 4: Update Personal Settings

1. **Replace Chat ID** in all Telegram nodes:
   - Find nodes with `"chatId": "<YOUR_CHAT_ID>"`
   - Replace `<YOUR_CHAT_ID>` with **your Chat ID**

2. **Update Database Tables** (if needed):
   - The workflow expects tables: `habit_logs`, `physique_logs`, `physique_profile`
   - SQL creation scripts are provided below in the detailed setup section

### Step 5: Test & Activate

1. **Save the workflow** (Ctrl+S)
2. **Activate the workflow** (toggle switch in top-right)
3. **Test by sending a message** to your Telegram bot:
   ```
   -chat Hello! Are you working?
   ```
4. If you get a response, **congratulations! 🎉** Your PersonalProgressAI is ready!

---

## 🔧 Troubleshooting

- **Bot not responding?** Check if the workflow is active and credentials are correct
- **Database errors?** Ensure tables exist and connection details are correct  
- **AI not working?** Verify your API keys and check rate limits
- **Wrong chat ID?** Double-check your Telegram Chat ID

Need help? Open an issue in this repository!

## 🌟 Features

### 📊 **Multi-Modal Progress Tracking**
- **Daily Habit Logging**: Track study, project work, sports, and social activities
- **Physical Measurements**: Monitor weight, body measurements (waist, neck, hip, shoulder, chest)
- **Automated Calculations**: BMI, body fat percentage, and muscle mass calculations
- **Visual Reports**: Automatically generated charts and graphs

### 🤖 **AI-Powered Chat Assistant**
- **Intelligent Message Routing**: Automatically categorizes incoming messages
- **Natural Language Processing**: Understands different command formats
- **Conversational Interface**: Chat with "Abidin" - your personal AI assistant
- **Database Queries**: Ask questions about your progress data

### 📈 **Automated Reporting**
- **Daily Reminders**: Scheduled prompts for habit tracking
- **Weekly Assessments**: Comprehensive progress summaries
- **Visual Analytics**: Chart generation for progress visualization
- **Calendar Integration**: Google Calendar event creation

## 🏗️ Architecture

### Core Components

1. **Telegram Integration**
   - Message reception and processing
   - Photo and text responses
   - User interaction management

2. **AI Language Models**
   - Google Gemini for text processing
   - Groq (Llama models) for conversational AI
   - OpenRouter for additional AI capabilities

3. **Database Management**
   - MySQL database with three main tables:
     - `habit_logs`: Daily activity tracking
     - `physique_logs`: Physical measurement data
     - `physique_profile`: Weekly calculated summaries

4. **Visualization Engine**
   - QuickChart.io integration for graph generation
   - Dynamic chart creation for trends analysis

## 📱 Usage

### Command Formats

#### 1. Daily Habit Logging (`-msg`)
```
-msg 1,1,0,1,today was productive
```
Format: `study,project,sport,social,note`
- `1` = activity completed
- `0` = activity not completed

#### 2. Physical Measurements (`-msr`)
```
-msr 75,180,80,38,95,110,100,feeling strong
```
Format: `weight(kg),height(cm),waist(cm),neck(cm),hip(cm),shoulder(cm),chest(cm),note`

#### 3. Chat Interaction (`-chat`)
```
-chat How has my progress been this week?
```
Free-form conversation with the AI assistant

### Automated Schedules

- **Daily Reminders**: Prompts for habit logging
- **Weekly Physical Check**: Requests for body measurements
- **Weekly Reports**: Comprehensive progress analysis with charts

## 🔧 Technical Implementation

### n8n Workflow Structure

The workflow consists of several interconnected sections:

1. **Message Router** - Analyzes incoming Telegram messages and routes them appropriately
2. **Habit Logger** - Processes daily activity data and stores in database
3. **Physique Tracker** - Handles body measurements and calculations
4. **Chat Bot** - Provides conversational AI interaction
5. **Report Generator** - Creates weekly summaries and visualizations
6. **Scheduler** - Manages automated reminders and reports

### Database Schema

#### `habit_logs`
- `study`, `project`, `sport`, `social` (BOOLEAN)
- `note` (TEXT)
- `date` (DATE)
- `created_at` (TIMESTAMP)

#### `physique_logs`
- `weight`, `height`, `waist`, `neck`, `hip`, `shoulder`, `chest` (DECIMAL)
- `note` (TEXT)
- `date` (DATE)
- `created_at` (TIMESTAMP)

#### `physique_profile`
- `weight`, `height`, `bmi`, `muscle_mass`, `body_fat_pct` (DECIMAL)
- `date` (DATE)
- `created_at` (TIMESTAMP)

### AI Integration

- **Google Gemini**: Primary language model for text processing and analysis
- **Groq (Llama)**: Conversational AI for chat interactions
- **Memory Management**: Maintains conversation context across sessions

## 🚀 Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- MySQL database
- Telegram Bot Token
- Google Gemini API key
- Groq API key (optional)
- Google Calendar API (optional)

### Configuration Steps

1. **Import Workflow**
   ```bash
   # Import the PersonalProgressAI-Samet.json file into your n8n instance
   ```

2. **Configure Credentials**
   - Telegram Bot API
   - Google Gemini API
   - MySQL Database connection
   - Groq API (if using)
   - Google Calendar API (if using)

3. **Database Setup**
   ```sql
   CREATE TABLE habit_logs (
     id INT AUTO_INCREMENT PRIMARY KEY,
     study BOOLEAN,
     project BOOLEAN,
     sport BOOLEAN,
     social BOOLEAN,
     note TEXT,
     date DATE,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE physique_logs (
     id INT AUTO_INCREMENT PRIMARY KEY,
     weight DECIMAL(5,2),
     height DECIMAL(5,2),
     waist DECIMAL(5,2),
     neck DECIMAL(5,2),
     hip DECIMAL(5,2),
     shoulder DECIMAL(5,2),
     chest DECIMAL(5,2),
     note TEXT,
     date DATE,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE physique_profile (
     id INT AUTO_INCREMENT PRIMARY KEY,
     weight DECIMAL(5,2),
     height DECIMAL(5,2),
     bmi DECIMAL(5,2),
     muscle_mass DECIMAL(5,2),
     body_fat_pct DECIMAL(5,2),
     date DATE,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

4. **Update Chat ID**
   - Replace `<YOUR_CHAT_ID>` with your Telegram chat ID in all Telegram nodes

5. **Activate Workflow**
   - Enable the workflow in n8n
   - Test with a simple message to your Telegram bot

## 📊 Sample Interactions

### Daily Check-in
**Bot**: "Bugün ders çalıştın mı? Proje üzerinde çalıştın mı? Spor yaptın mı?"
**User**: `-msg 1,1,0,1,productive day despite no sports`
**Bot**: "Değerler database'e başarıyla kaydedildi. ✅"

### Weekly Assessment
**User**: `-chat How's my progress this week?`
**Bot**: "Bu hafta çok iyisin hacı! Çalışma konusunda stabil gitmiş, sosyal tarafta da fena değil. Sadece sporu biraz ihmal etmişsin, ona biraz daha ağırlık ver derim."

### Physical Tracking
**User**: `-msr 75.5,180,82,36,96,112,102,feeling stronger`
**Bot**: "Değerler database'e başarıyla kaydedildi. ✅"
*[Followed by calculated BMI, body fat %, and muscle mass analysis]*

## 🎯 Key Benefits

- **Automated Tracking**: No manual spreadsheets or apps needed
- **AI-Powered Insights**: Intelligent analysis of your progress patterns
- **Conversational Interface**: Natural language interaction with your data
- **Visual Progress**: Automatic chart generation and trend analysis
- **Comprehensive Monitoring**: Both behavioral and physical metrics in one system
- **Personalized Feedback**: AI assistant provides contextual advice and motivation

## 🔮 Future Enhancements

- Sleep tracking integration
- Nutrition logging capabilities
- Goal setting and achievement tracking
- Multi-user support
- Advanced analytics and machine learning insights
- Mobile app companion

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📞 Support

For questions or support, please open an issue in this repository.

---

**Made with ❤️ using n8n automation platform**
