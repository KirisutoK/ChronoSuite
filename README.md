# ChronoSuite

A multi-application **Java console productivity suite** built around the Java **Date and Time API**. Track life milestones and subscriptions, with password-protected **JSON save files** and per-file activity logs.

> **Status:** Work in progress. This is the CLI (console) version of ChronoSuite.

---

## Applications

| App | Status | What it does |
|-----|--------|--------------|
| **MileStone Tracker** | ✅ Working | Tracks **age-based** milestones (e.g., "Age 21: graduate college") and **day-based** milestones (e.g., "Day 10,000 alive"), calculated from your birthday |
| **Subscription Tracker** | ✅ Working | Stores subscriptions with name, price, billing cycle and start date (today or an earlier date) |
| **Work Hours Tracker** | 🚧 Planned | Clock-in/clock-out logger with daily totals, overtime flags and weekly summaries |

---

## Features

- **User profile:** enter a username and birthday at startup, with input validation (correct date format, no future birthdays). Both can be changed later from the main menu.
- **Save file management:** create, load, view and delete save files, including deleting the current file, a selected file or all files.
- **Password-protected saves:** passwords are hashed with **SHA-256** and are never stored as plain text.
- **JSON persistence:** data is saved as `.json` files using **Gson**, with a custom `TypeAdapter` for `LocalDate`.
- **Activity logging:** actions like logging in, logging out and adding subscriptions are recorded in a `.log` file for each save file.
- **Error handling:** clear error messages for invalid input, bad dates and file problems instead of crashes.

---

## Built With

- **Java 17+** (Date and Time API: `LocalDate`, `Period`, `ChronoUnit`)
- **[Gson](https://github.com/google/gson)** for JSON serialization
- `java.security.MessageDigest` for SHA-256 hashing

---

## Project Structure

```
ChronoSuite/
├── Menu.java                      # Entry point (main) + Main Menu
├── Applications/
│   ├── Application.java
│   ├── MileStoneTracker/          # Milestone logic, data class + menu
│   ├── SubscriptionTracker/       # Subscription logic, data classes + menu
│   └── WorkHoursTracker/          # Planned
└── Misc/
    ├── DataManager.java           # Age calculation and password hashing
    ├── FileManager.java           # Creating, loading and saving JSON files
    ├── Logger.java                # Activity logs
    ├── ReuseableMethodsCLI.java   # Shared input/output helpers
    └── GSON_Adapters/
        └── GsonAdapter_Date.java  # LocalDate ↔ JSON adapter
```

Save files and logs are created automatically at runtime in a `Saves/` folder:

```
Saves/
└── <AppName>/
    ├── Datas/   # .json save files
    └── Logs/    # .log activity files
```

---

## How to Run

### Step 1: Install Java
You need the **Java Development Kit (JDK) 17 or newer**.

1. If you don't have it, download a free JDK from [Adoptium](https://adoptium.net/) and install it. Use the default options; on Windows, make sure **"Set JAVA_HOME"** and **"Add to PATH"** are checked.
2. Check that it worked by opening a terminal and running:
   ```bash
   java -version
   javac -version
   ```
   Both should print a version number (17 or higher). If you see "not recognized" or "command not found," see [Troubleshooting](#troubleshooting).

### Step 2: Download the project
Choose one:

- **With Git:**
  ```bash
  git clone https://github.com/KirisutoK/ChronoSuite.git
  cd ChronoSuite
  ```
- **Without Git:** on this page, click the green **Code** button → **Download ZIP**, then unzip it.

### Step 3: Download Gson
ChronoSuite uses **Gson** to save files as JSON.

1. Go to [Gson on Maven Central](https://central.sonatype.com/artifact/com.google.code.gson/gson) and download the latest `gson-x.x.x.jar` (click **Browse** → pick the newest version → download the `.jar` file).
2. Inside the **ChronoSuite** folder, create a folder named `lib`.
3. Move the `.jar` file into `lib`. It should look like this:
   ```
   ChronoSuite/
   ├── lib/
   │   └── gson-x.x.x.jar
   ├── Menu.java
   └── ...
   ```

### Step 4: Run it

> **Important:** run the program **from the ChronoSuite folder**. The `Saves/` folder is created wherever the program is started, so starting it from another folder will save your files in the wrong place.

#### Option A: IntelliJ IDEA
1. Open IntelliJ → **File → Open** → select the **ChronoSuite** folder → **OK**.
2. If IntelliJ asks for a JDK (a yellow bar at the top), click **Setup SDK** and choose the JDK you installed.
3. Add Gson: **File → Project Structure → Libraries → + → Java** → select the `.jar` in `lib` → **OK**.
4. In the Project panel on the left, right-click **Menu.java** → **Run 'Menu.main()'**.
5. The program starts in the **Run** window at the bottom. Click inside it to type your answers.

#### Option B: VS Code
1. Install the **Extension Pack for Java** from the Extensions tab.
2. **File → Open Folder** → select the **ChronoSuite** folder.
3. Add Gson: in the **Java Projects** panel (bottom left), open **Referenced Libraries** → click **+** → select the `.jar` in `lib`.
4. Open **Menu.java** and click **Run** above the `main` method.
5. Type your answers in the **Terminal** panel at the bottom.

#### Option C: Command Line (CMD / PowerShell / Terminal)

**1. Open the command line**
- **Windows:** press the **Windows key**, type `cmd`, and press **Enter**.
- **macOS:** press **Cmd + Space**, type `Terminal`, and press **Enter**.

**2. Copy the path of the ChronoSuite folder**
- **Windows:** in File Explorer, right-click the **ChronoSuite** folder → **Copy as path**.
- **macOS:** in Finder, right-click the folder, hold **Option**, and click **Copy "ChronoSuite" as Pathname**.

**3. Go to the folder** with `cd`, then paste the path (keep the quotes in case the path has spaces):
```bash
cd "C:\Users\YourName\Downloads\ChronoSuite"
```
> **Windows:** if the folder is on a different drive (like `D:`), use `cd /d "D:\path\to\ChronoSuite"`.

**4. Compile** the code (turns the `.java` files into runnable `.class` files):
```bash
javac -encoding UTF-8 -cp "lib/*" Menu.java Applications/Application.java Applications/MileStoneTracker/MileStoneTracker.java Applications/MileStoneTracker/MileStoneTrackerData.java Applications/MileStoneTracker/MileStoneTrackerMenu.java Applications/SubscriptionTracker/SubscriptionData.java Applications/SubscriptionTracker/SubscriptionTracker.java Applications/SubscriptionTracker/SubscriptionTrackerData.java Applications/SubscriptionTracker/SubscriptionTrackerMenu.java Applications/WorkHoursTracker/WorkHoursTracker.java Misc/DataManager.java Misc/FileManager.java Misc/Logger.java Misc/ReuseableMethodsCLI.java Misc/GSON_Adapters/GsonAdapter_Date.java
```
If it worked, nothing is printed and new `.class` files appear in the folders.

**5. Run** the program:
- **Windows:**
  ```bash
  java -cp ".;lib/*" Menu
  ```
- **macOS / Linux:**
  ```bash
  java -cp ".:lib/*" Menu
  ```
You should see **Enter Username:**. After entering your username and birthday, the **ChronoSuite 1.0** main menu appears. To quit, press **Ctrl + C**.

### Troubleshooting

| Problem | Fix |
|---------|-----|
| `'java' / 'javac' is not recognized` or `command not found` | Java isn't installed or isn't on your PATH. Reinstall the JDK with "Add to PATH" checked, then **close and reopen** the terminal. |
| `package com.google.gson does not exist` | Gson is missing. Make sure the `.jar` is inside a folder named `lib` in the ChronoSuite folder, and that the compile command includes `-cp "lib/*"`. |
| `NoClassDefFoundError: com/google/gson/...` when running | The run command is missing Gson. Use `java -cp ".;lib/*" Menu` on Windows or `java -cp ".:lib/*" Menu` on macOS/Linux (Windows uses `;`, macOS/Linux uses `:`). |
| `error: package Applications... does not exist` or `package Misc does not exist` | Make sure you're in the ChronoSuite folder and include **every** file from the compile command above. |
| `Could not find or load main class Menu` | Run the `java` command from the same folder where you ran `javac`, and type `Menu` exactly (capital M, no `.java`). |
| Borders show as `?` or strange symbols | Windows terminal encoding issue. Run `chcp 65001`, then run the program again. Using **Windows Terminal** instead of the old CMD window also helps. |
| Your saved files aren't showing up | You probably started the program from a different folder. `cd` into the ChronoSuite folder and run it again. |

---

## Roadmap

- [x] Migrate saves from Java serialization to JSON (Gson + custom `LocalDate` adapter)
- [x] Hash passwords with SHA-256
- [x] Subscription Tracker (console + JSON)
- [ ] Package as a standalone `.exe`
- [ ] Work Hours Tracker

---

## Author

**Christ Aerjil C. Dampog**, Computer Information Systems student at Finger Lakes Community College
GitHub: [@KirisutoK](https://github.com/KirisutoK)