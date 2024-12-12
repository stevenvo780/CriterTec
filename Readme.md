# Deploying and Using the Godot Build

This guide explains how to deploy and use the Godot project compiled and uploaded, either to Firebase Hosting or a custom Ubuntu server with flexibility for other tools.

## Prerequisites

### For Firebase Hosting:
1. **Node.js and npm** installed on your system.
   - Download and install Node.js from [https://nodejs.org/](https://nodejs.org/).
2. **Firebase CLI** installed globally.
   - Run `npm install -g firebase-tools` to install the Firebase CLI.
3. **Firebase Project** set up.
   - Create a Firebase project in the [Firebase Console](https://console.firebase.google.com/).

### For Ubuntu Server:
1. **Node.js and npm** installed.
2. **NGINX** installed for serving static files.
3. **Docker** installed (optional).

## Project Structure

The project directory structure is as follows:

```
.
├── firebase.json
└── public
    ├── index.apple-touch-icon.png
    ├── index.audio.worklet.js
    ├── index.html
    ├── index.icon.png
    ├── index.js
    ├── index.pck
    ├── index.png
    └── index.wasm
```

- `firebase.json`: Firebase configuration file.
- `public/`: Directory containing the compiled Godot project files.

## Deployment Options

### Option 1: Firebase Hosting

1. **Login to Firebase**:
   Open a terminal and run:
   ```bash
   firebase login
   ```
   Follow the instructions to authenticate with your Firebase account.

2. **Initialize Firebase Hosting**:
   Run the following command in the project directory:
   ```bash
   firebase init hosting
   ```
   - Select your Firebase project.
   - Set `public` as the hosting directory.
   - Choose "No" for configuring as a single-page app unless required.

3. **Deploy the Project**:
   Deploy the compiled Godot build to Firebase Hosting with:
   ```bash
   firebase deploy
   ```

4. **Access the Hosted Game**:
   Once deployed, Firebase will provide a hosting URL. Open the provided URL in your browser to access the game.

### Option 2: Ubuntu Server with NGINX

1. **Set Up NGINX**:
   Install NGINX:
   ```bash
   sudo apt update && sudo apt install nginx -y
   ```

2. **Upload Files**:
   Copy the contents of the `public` directory to `/var/www/html`:
   ```bash
   sudo cp -r public/* /var/www/html/
   ```

3. **Configure NGINX**:
   Create or edit an NGINX configuration file:
   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```
   Replace the content with:
   ```nginx
   server {
       listen 80;
       server_name _;

       root /var/www/html;
       index index.html;

       location / {
           try_files $uri /index.html;
       }
   }
   ```
   Save and exit.

4. **Restart NGINX**:
   ```bash
   sudo systemctl restart nginx
   ```

5. **Access the Game**:
   Open your server's IP address in a browser to view the game.

### Option 3: Using Docker

1. **Create Dockerfile**:
   In the project root, create a `Dockerfile`:
   ```dockerfile
   FROM nginx:alpine

   COPY public /usr/share/nginx/html

   EXPOSE 80

   CMD ["nginx", "-g", "daemon off;"]
   ```

2. **Build Docker Image**:
   Build the Docker image:
   ```bash
   docker build -t godot-server .
   ```

3. **Run Docker Container**:
   Run the container:
   ```bash
   docker run -d -p 80:80 godot-server
   ```

4. **Access the Game**:
   Open `http://localhost` in your browser to view the game.

## Running Locally

### Firebase Hosting Emulator
To test the Godot build locally before deploying:

1. Start the Firebase Hosting emulator:
   ```bash
   firebase emulators:start
   ```

2. Open `http://localhost:5000` in your browser to view the game.

### NGINX or Docker
For NGINX, copy files to a local NGINX server as described above. For Docker, run the container locally.

## Notes

- Ensure that all Godot build files (`index.html`, `index.js`, `index.pck`, `index.wasm`, etc.) are placed in the `public/` directory.
- If you encounter any issues with Firebase, check the documentation at [https://firebase.google.com/docs/hosting](https://firebase.google.com/docs/hosting).
- For Docker-related issues, refer to the [Docker documentation](https://docs.docker.com/).

Enjoy your Godot project hosted on Firebase, NGINX, or Docker!

