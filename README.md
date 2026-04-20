# 📖 FarazAI— Android App

> *A creative space designed for writers, poets, and thinkers.*

---

## 🏗️ Project Structure

```
PoetsPad/
├── app/
│   ├── build.gradle                          ← All dependencies listed here
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── res/values/themes.xml
│       └── java/com/poetsPad/
│           ├── MainActivity.kt               ← Entry point + Navigation
│           ├── domain/model/
│           │   └── Note.kt                   ← Data model + Room Entity
│           ├── data/
│           │   ├── db/
│           │   │   ├── NoteDao.kt            ← All DB queries
│           │   │   └── PoetsPadDatabase.kt   ← Room DB singleton
│           │   └── repository/
│           │       └── NoteRepository.kt     ← Data layer abstraction
│           ├── ui/
│           │   ├── theme/
│           │   │   └── MoodThemes.kt         ← 4 mood themes (Dark/Romantic/Intense/Calm)
│           │   ├── components/
│           │   │   └── Components.kt         ← Reusable composables
│           │   ├── viewmodel/
│           │   │   ├── NoteListViewModel.kt  ← Home screen state
│           │   │   └── EditorViewModel.kt    ← Editor state + AI suggestions
│           │   └── screens/
│           │       ├── HomeScreen.kt         ← Dashboard + notes list
│           │       ├── EditorScreen.kt       ← Poetry writing editor
│           │       ├── MoodThemeScreen.kt    ← Mood/theme picker
│           │       ├── PromptScreen.kt       ← Prompt generator
│           │       ├── StatsScreen.kt        ← Writing statistics
│           │       ├── PinScreen.kt          ← PIN entry / setup
│           │       └── SettingsScreen.kt     ← App settings
│           └── utils/
│               ├── WritingPrompts.kt         ← 20+ prompts + AI line suggestions
│               ├── PinManager.kt             ← PIN storage/verification
│               ├── StreakTracker.kt          ← Daily writing streak logic
│               └── VoiceInputHelper.kt       ← Android SpeechRecognizer wrapper
├── build.gradle
└── settings.gradle
```

---

## 🚀 Setup Instructions

### 1. Open in Android Studio
- Open **Android Studio Hedgehog** (2023.1.1) or newer
- Select **File → Open** and choose the `PoetsPad/` folder
- Wait for Gradle sync to complete

### 2. Requirements
| Item | Version |
|------|---------|
| Android Studio | Hedgehog+ |
| Kotlin | 1.9.22 |
| Min SDK | 26 (Android 8.0) |
| Target SDK | 34 (Android 14) |
| Java | 17 |

### 3. Run the App
- Connect a device or start an emulator (API 26+)
- Press **Run ▶** or `Shift+F10`

---

## 🎨 Architecture

```
UI Layer (Compose Screens)
        ↕ StateFlow / collectAsState()
ViewModel Layer (NoteListViewModel, EditorViewModel)
        ↕ suspend functions / Flow
Repository Layer (NoteRepository)
        ↕ DAO methods
Data Layer (Room DB — PoetsPadDatabase)
```

**Pattern:** MVVM (Model-View-ViewModel)  
**Database:** Room with Kotlin Coroutines + Flow  
**UI:** 100% Jetpack Compose  
**Navigation:** Compose Navigation  

---

## 🌙 Mood Themes

| Mood | Emoji | Feel |
|------|-------|------|
| Dark | 🌙 | Midnight purple — elegant and focused |
| Romantic | 🌸 | Deep rose — warm and tender |
| Intense | 🔥 | Ember amber — passionate and bold |
| Calm | ☁️ | Ocean blue — peaceful and clear |

---

## ✨ Key Features

### Poetry Mode
- Centered text alignment toggle
- Line-by-line writing with auto line breaks
- Live word count + line count in toolbar

### AI Line Suggestion (Placeholder)
- Tap **"✨ Finish Line"** in the editor bottom bar
- Keyword-based suggestion engine in `WritingPrompts.kt`
- **To connect real AI:** Replace `WritingPrompts.suggestNextLine()` with an API call to Claude or OpenAI

### Writing Prompts
- 20+ curated poetry prompts
- Mood-aware: prompts change based on your current theme
- Line starters for when you're stuck on the first word

### Writing Streak
- Tracked daily via `StreakTracker`
- Displayed as 🔥 badge on home screen

### PIN Protection
- 4-digit PIN stored in SharedPreferences
- Apply to individual notes via the lock toggle in editor

---

## 🔌 Connecting Real AI (Claude API)

To replace the placeholder AI with real Claude suggestions:

```kotlin
// In EditorViewModel.kt, replace suggestNextLine() with:

suspend fun suggestNextLine() {
    val lastLine = _content.value.text.lines().lastOrNull { it.isNotBlank() } ?: return
    
    val response = withContext(Dispatchers.IO) {
        // Call Claude API
        ClaudeApiClient.complete(
            prompt = "Complete this poetic line in 1 line:\n\"$lastLine\"\nResponse:"
        )
    }
    _aiSuggestion.value = response
}
```

Add `INTERNET` permission to `AndroidManifest.xml` and your API key to local config.

---

## 📦 Dependencies Summary

| Library | Purpose |
|---------|---------|
| `androidx.compose.bom` | All Compose UI |
| `material3` | Material Design 3 components |
| `material-icons-extended` | All Material icons |
| `navigation-compose` | Screen navigation |
| `room-runtime` + `room-ktx` | Local database |
| `lifecycle-viewmodel-compose` | ViewModel integration |
| `coroutines-android` | Async / background work |

---

## 📝 Adding Your Own Prompts

Open `utils/WritingPrompts.kt` and add to the lists:

```kotlin
val poetryPrompts = listOf(
    // Add your prompts here
    "Your custom prompt...",
    ...
)
```

---

*Built with ❤️ for poets everywhere.*
