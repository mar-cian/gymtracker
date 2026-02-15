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

## Production Deployment (HTTPS with giani.cc)

To run this on your Debian server with the domain `giani.cc`:

1.  **Install Nginx and Certbot**
    ```bash
    sudo apt update
    sudo apt install -y nginx certbot python3-certbot-nginx
    ```

2.  **Setup Nginx Configuration**
    Copy the provided `nginx.conf` to the Nginx sites directory:
    ```bash
    sudo cp nginx.conf /etc/nginx/sites-available/giani.cc
    sudo ln -s /etc/nginx/sites-available/giani.cc /etc/nginx/sites-enabled/
    sudo nginx -t  # Test configuration
    sudo systemctl restart nginx
    ```

3.  **Enable HTTPS (SSL)**
    Run Certbot to automatically obtain and configure SSL certificates:
    ```bash
    sudo certbot --nginx -d giani.cc -d www.giani.cc
    ```
    Follow the prompts. Certbot will automatically update your Nginx config to force HTTPS.

4.  **Keep App Running**
    Use `pm2` to keep the Node.js app running in the background:
    ```bash
    sudo npm install -g pm2
    pm2 start server.js --name "gym-tracker"
    pm2 save
    pm2 startup
    ```