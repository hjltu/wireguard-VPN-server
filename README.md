# wireguard-VPN-server
I hope this little project will help you to aviod sensorship of some shit county like russian pederation



## Installation and Running Instructions

### 1. Install Docker and Docker Compose
If you haven't installed Docker and Docker Compose, follow these steps:

#### For Ubuntu:
```bash
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
```

#### For other systems:
Visit the [official Docker documentation](https://docs.docker.com/get-docker/) and [Docker Compose installation guide](https://docs.docker.com/compose/install/).

### 2. Create the Docker Compose File
1. Create a new directory for your setup:
   ```bash
   mkdir ~/wg-easy-setup
   cd ~/wg-easy-setup
   ```

2. Create the `docker-compose.yml` file:
   ```bash
   nano docker-compose.yml
   ```
   
   Paste the `docker-compose.yml` content into this file and save it.

### 3. Set Up Password Hash
Generate a password hash for secure access to the web interface. You can use the following command:
```bash
openssl passwd -6 your_password_here
```
Replace `your_password_here` with the password you want to use. Copy this hash and place it in the `PASSWORD_HASH` field in `docker-compose.yml`.

### 3.1 Or alternative way to Set Up Password Hash
Generate a password hash for secure access to the web interface. You can use the following command:
```bash
docker run -it --rm ghcr.io/wg-easy/wg-easy wgpw YOUR_PASSWORD
```
Replace `YOUR_PASSWORD` with the password you want to use. Copy this hash and place it in the `PASSWORD_HASH` field in `docker-compose.yml`.
If your hash contains $ signs, you must double them (e.g., $$)

### 4. Start the Docker Containers
Run the following command to start your containers:
```bash
docker-compose up -d
```
Check how its run:
```bash
docker compose logs -f wg-easy
```

### 5. Access the Web Interface
Open your web browser and navigate to:
```
http://<your-server-ip>:8822
```

Log in using the password you set up using the generated password hash.

### 6. Setting Up Nginx Proxy Manager
Access the manager at http://<your-ip>:81.
Create a Proxy Host.
Domain Name: e.g., vpn.yourdomain.com.
Forward Hostname: Use the container name wg-easy (if both are on the same Docker network) or your server's local IP.
Forward Port: 8822.
SSL: Use the built-in "Request a new SSL Certificate" to enable HTTPS via Let's Encrypt.
Remove Public Port: Once your proxy is working, you can remove the 8822:8822

### 7. Configure WireGuard Clients
Once you're logged in, follow these steps to add a client:
1. Go to the "Clients" section in the web interface.
2. Click on "Add Client" and fill in the necessary details (name, DNS settings, etc.).
3. Once created, download the configuration file for the client.

### 8. Install WireGuard on Client Devices
Depending on the operating system of your client devices:

#### For Windows:
- Download and install the WireGuard client from [the official website](https://www.wireguard.com/install/).
- Import the downloaded configuration file.

#### For Linux:
```bash
sudo apt install wireguard
```
Then, use the configuration file to set it up:
```bash
sudo mv ~/path/to/client.conf /etc/wireguard/
sudo wg-quick up client.conf
```

#### For Android/iOS:
- Download the WireGuard app from the [Google Play Store](https://play.google.com/store/apps/details?id=com.wireguard.android) or [Apple App Store](https://apps.apple.com/us/app/wire
