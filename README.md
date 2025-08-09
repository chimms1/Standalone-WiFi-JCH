# Automated Wi-Fi Jammer and Password Cracker

This project is an automated Wi-Fi security testing tool that runs on a Raspberry Pi. It continuously jams a target Wi-Fi network and attempts to crack its WPA/WPA2 password using a dictionary attack.

## Disclaimer

This tool is intended for educational and authorized security testing purposes only. Unauthorized use of this tool to attack Wi-Fi networks is illegal. The authors are not responsible for any misuse of this software.

## Features

* **Automated Deauthentication:** Continuously sends deauthentication packets to disrupt the target Wi-Fi network.
* **WPA/WPA2 Handshake Capture:** Automatically captures WPA/WPA2 handshakes after deauthenticating a client.
* **Dictionary-Based Password Cracking:** Uses `aircrack-ng` with a provided wordlist to crack the captured handshake.
* **Autonomous Operation:** Designed to run on a Raspberry Pi from boot, powered by a power bank for portable use.
* **Systemd Service:** Includes a systemd service file for easy setup to run on boot.

## Hardware Requirements

* Raspberry Pi 4 Model B (1GB RAM)
* Monitor Mode Capable Wi-Fi Adapter
* Power Bank (e.g., 10000 mAh) for portable operation

## Software Requirements

* Kali Linux for Raspberry Pi
* Aircrack-ng Suite (pre-installed on Kali Linux)
* Python 3

## Setup and Installation

1.  **Flash Kali Linux:** Install Kali Linux on an SD card and boot your Raspberry Pi.
2.  **Clone the Repository:**
    ```bash
    git clone https://github.com/chimms1/Standalone-WiFi-JCH.git
    cd Standalone-WiFi-JCH
    ```
3.  **Configure the Target Network:**
    * Edit the `wifi_config.txt` file and enter the SSID of the target Wi-Fi network.
4.  **Install the Systemd Service:**
    * Copy the `hacking.service` file to the systemd directory:
        ```bash
        sudo cp systemd/hacking.service /etc/systemd/system/
        ```
    * Enable the service to start on boot:
        ```bash
        sudo systemctl enable hacking.service
        ```
    * Start the service immediately:
        ```bash
        sudo systemctl start hacking.service
        ```

## Usage

Once the setup is complete, the script will run automatically on boot. The Raspberry Pi can be powered by a power bank for portable and autonomous operation.

The script will perform the following cycle:
1.  Jam the target Wi-Fi network for 15 seconds.
2.  Attempt to capture a handshake and crack the password for 1 minute.

The results of the password cracking attempts will be logged in the `cracked_passwords.txt` file in the `scripts` directory.

## How It Works

The `minimalfinal0.py` script orchestrates the attack using the Aircrack-ng suite:
1.  **`airmon-ng`:** Puts the Wi-Fi adapter into monitor mode.
2.  **`airodump-ng`:** Scans for the target network to get its BSSID and channel.
3.  **`aireplay-ng`:** Sends deauthentication packets to clients on the network.
4.  **`airodump-ng`:** Captures the WPA/WPA2 handshake when a client reconnects.
5.  **`aircrack-ng`:** Attempts to crack the captured handshake using the `rockyou.txt` wordlist.

## Future Improvements

* Implement proper exception handling for a more robust script.
* Add support for multiple wordlists.
* Incorporate a web interface for easier configuration and monitoring.
* Add functionality to automatically hop channels to find the target network.
