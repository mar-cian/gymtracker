# Gym Tracker 2026

Mobile-First Full-Stack Web Application for tracking workouts with a heatmap visualization.

## Features
- **Users**: Giani, Angie, Sämi
- **Heatmap**: 365-day visualization for 2026 (Blue = Cardio, Green = Strength)
- **Mobile Optimized**: Horizontal scrolling, touch-friendly
- **Easter Egg**: Type "gravity" (or tap title and type in hidden field) to activate physics mode!

## Installation on Debian Server

1. **Update System & Install Node.js**
   ```bash
   sudo apt update
   sudo apt install -y nodejs npm
   ```

2. **Clone/Copy Project**
   Copy the files to your server (e.g., via `git clone` or `scp`).
   ```bash
   mkdir gym-tracker
   cd gym-tracker
   # (Upload files here)
   ```

3. **Install Dependencies**
   ```bash
   npm install
   ```

4. **Start the Server**
   ```bash
   npm start
   ```
   Or for background execution (using pm2):
   ```bash
   sudo npm install -g pm2
   pm2 start server.js --name "gym-tracker"
   ```

5. **Access**
   Open `http://YOUR_SERVER_IP:3000` in your browser.

## Tech Stack
- **Backend**: Node.js + Express
- **Database**: SQLite (stored in `gym.db`)
- **Frontend**: Vanilla HTML/CSS/JS + Matter.js (CDN)