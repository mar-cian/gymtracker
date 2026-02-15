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

## Production Deployment (Cloudflare Tunnel)

Since you are using Cloudflare Tunnel, you don't need to open ports on your router or manage SSL certificates on the server. Cloudflare handles all of that.

### 1. Simplify Nginx (Optional but recommended)
If you still want to use Nginx as a reverse proxy (e.g. for potential caching or log management), use the provided `nginx.conf`.
It simply listens on port 80 and forwards to your app.
```bash
sudo cp nginx.conf /etc/nginx/sites-available/giani.cc
sudo ln -sf /etc/nginx/sites-available/giani.cc /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default  # Remove default if present
sudo systemctl restart nginx
```
*Note: You can also skip Nginx entirely and point the Tunnel directly to `http://localhost:3000`.*

### 2. Install Cloudflared
On your Debian server:
```bash
# Add Cloudflare GPG key
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null

# Add Repo
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared bookworm main' | sudo tee /etc/apt/sources.list.d/cloudflared.list

# Install
sudo apt-get update && sudo apt-get install cloudflared
```

### 3. Authenticate & Create Tunnel
```bash
# Login (opens a URL, authorize with your giani.cc account)
cloudflared tunnel login

# Create a tunnel (replace NAME with whatever you want, e.g., 'gym-server')
cloudflared tunnel create gym-server
```
*Save the Tunnel ID shown in the output!*

### 4. Configure Tunnel
Create a configuration file `~/.cloudflared/config.yml`:
```yaml
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json

ingress:
  # If using Nginx on port 80:
  - hostname: giani.cc
    service: http://localhost:80
  # OR if connecting directly to Node.js app:
  # - hostname: giani.cc
  #   service: http://localhost:3000
    
  # Catch-all
  - service: http_status:404
```

### 5. Route DNS
Point your domain to the tunnel:
```bash
cloudflared tunnel route dns gym-server giani.cc
```

### 6. Run as Service
Install it as a system service so it starts automatically:
```bash
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

### 7. Node.js App Management
Don't forget to keep your app running:
```bash
pm2 start server.js --name "gym-tracker"
pm2 save
pm2 startup
```