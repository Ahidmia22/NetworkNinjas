#!/usr/bin/env python3
import os
import sys
import subprocess

# ========================
# NetworkNinjas Main Launcher
# ========================

BANNER = r"""
_      _____ _____  _      ____  ____  _  __ _      _  _         _  ____  ____  
/ \  /|/  __//__ __\/ \  /|/  _ \/  __\/ |/ // \  /|/ \/ \  /|   / |/  _ \/ ___\ 
| |\ |||  \    / \  | |  ||| / \||  \/||   / | |\ ||| || |\ ||   | || / \||    \ 
| | \|||  /_   | |  | |/\||| \_/||    /|   \ | | \||| || | \||/\_| || |-||\___ | 
\_/  \|\____\  \_/  \_/  \|\____/\_/\_\\_|\_\\_/  \|\_/\_/  \|\____/\_/ \|\____/
"""

# ========================
# ফোল্ডারের লিস্ট 1 থেকে 140
# ========================
FOLDERS = [
    "01_🛡_SQL_Injection", "02_🔑_Admin_Panel_Finder", "03_🖥_RAT_Creator",
    "04_🔗_Link_Grabber", "05_📸_Instagram_Hack", "06_📘_Facebook_Hack",
    "07_🌐_IP_Tracker", "08_🖥_Website_Cloner", "09_📡_Ping_Test",
    "10_🔍_Port_Scanner", "11_📜_Whois_Lookup", "12_🛠_DNS_Resolver",
    "13_📧_Email_Tracker", "14_☁️_Cloudflare_Bypass", "15_🖥_Domain_Availability",
    "16_💻_XSS_Tester", "17_🛡_SQL_Scanner", "18_🔑_Admin_Login_Brute",
    "19_🔎_Google_Dorker", "20_🌐_Html_Sniffer", "21_⌨️_JS_Keylogger",
    "22_🍪_Cookie_Stealer", "23_📶_Mac_Address_Finder", "24_🖼_Image_Metadata_Extractor",
    "25_🗂_File_Hasher", "26_📋_Clipboard_Hacker", "27_🛡_ARP_Spoofer",
    "28_📡_WiFi_Hacking", "29_📄_PDF_Exploit", "30_💻_USB_Infector",
    "31_🛡_Malware_Scanner", "32_📦_Packet_Analyzer", "33_📡_Sniffer",
    "34_⌨️_Keylogger", "35_📷_Cam_Hack", "36_🎧_Audio_Hack",
    "37_📱_Mobile_Trace", "38_🌐_Network_Mapper", "39_🔵_Bluetooth_Attack",
    "40_⚡_Hydra", "41_📱_WhatsApp_Bomber", "42_📩_SMS_Bomber",
    "43_📧_Email_Bomber", "44_📞_Phone_Number_Scanner", "45_💳_Credit_Card_Checker",
    "46_💰_Paypal_Checker", "47_📸_Instagram_Cracker", "48_📘_Facebook_Cracker",
    "49_🌐_Netcut_Tool", "50_📡_Network_Jammer", "51_📶_WiFi_Deauth_Attack",
    "52_🛡_MITM_Attack", "53_💻_Reverse_Shell", "54_⚡_DDoS_Attack",
    "55_🌐_Web_Shell", "56_🦠_Virus_Maker", "57_📄_PDF_Virus",
    "58_🖼_PNG_Virus", "59_📱_Apk_Backdoor", "60_🎭_Phishing_Page_Maker",
    "61_⌨️_Keylogger_Premium", "62_🔑_SSH_Force_Login", "63_🖥_RDP_Brute_Force",
    "64_🎭_Spoofed_Social_Media", "65_🛠_Exploit_DB_Searcher", "66_📜_Wordlist_Generator",
    "67_🛡_Metasploit_Auto_Exploit", "68_📷_Camera_Hack_Advance", "69_🎧_Audio_Stream_Listener",
    "70_🗂_File_Binder", "71_📍_GPS_Spoofer", "72_📱_WhatsApp_Spy",
    "73_💬_Termux_Chat_Sniffer", "74_🎭_Social_Engineering_Toolkit", "75_📢_Fake_Update_Attacker",
    "76_🔑_Session_Hijacker", "77_📶_WiFi_Password_Sniffer", "78_🔗_QR_Code_Phisher",
    "79_🎭_Scam_Page_Maker", "80_🗂_Data_Extractor_From_Target",
    "81_💻_Remote_File_Transfer", "82_🛡_Anti_Trace_Tool", "83_🔥_Firewall_Bypasser",
    "84_📧_Mail_Spoofer", "85_📋_Clipboard_Snatcher", "86_💻_Online_Termux_Control",
    "87_🛠_C2_Command_Tool", "88_📦_Custom_Payload_Creator", "89_🛡_FUD_Encoder",
    "90_💻_Shellcode_Injector", "91_🛠_Payload_Crypter", "92_💻_USB_Auto_Execute",
    "93_🔑_Password_Dump_Grabber", "94_🌐_DarkWeb_Searcher", "95_📡_VPN_Hack_Script",
    "96_💻_Bios_Password_Extractor", "97_⚡_BruteForce_Cloud", "98_📱_Remote_Mobile_Hack",
    "99_🌐_Tor_Analyzer", "100_🕵️_Deep_Web_Scanner", "101_🛡_Rootkit_Creator",
    "102_📱_Premium_RAT_Advance", "103_🌐_DarkWeb_URL_Scanner", "104_🕵️_DeepWeb_OSINT_Search",
    "105_🌐_Tor_Network_Monitor", "106_💻_Anonymous_File_Uploader", "107_🌐_Onion_Proxy_Checker",
    "108_🌐_Tor_VPN_Chain", "109_🌐_ExitNode_Checker", "110_🛒_Dark_Market_Tracker",
    "111_💬_Anonymous_Chatroom_Access", "112_🌐_ZeroNet_Access", "113_🛡_Onion_DDoS_Protector_Tool",
    "114_🗑_MetaData_Remover", "115_📶_Mac_Address_Changer", "116_📡_VPN_Spoof_Checker",
    "117_🌐_Browser_Fingerprint_Spoofer", "118_🛠_Private_Search_Engine", "119_🔓_Premium_Unlock",
    "120_🗑_Ads_Removed", "121_🖥_jadx_AI_Auto_Pattern_Detector", "122_📱_Sign_Apk",
    "123_🛠_Advanced_Patch_System_Java", "124_🔑_Extra_Premium_Feature_1", "125_🔑_Extra_Premium_Feature_2",
    "126_🔑_Extra_Premium_Feature_3", "127_🔑_Extra_Premium_Feature_4", "128_🔑_Extra_Premium_Feature_5",
    "129_🛡_Advanced_Exploit_Feature_1", "130_🛡_Advanced_Exploit_Feature_2",
    "131_🛡_Advanced_Exploit_Feature_3", "132_🛡_Advanced_Exploit_Feature_4",
    "133_🛡_Advanced_Exploit_Feature_5", "134_📱_Ultimate_Payload_Creator",
    "135_🛡_AI_Patch_Detector", "136_🔒_System_Protector", "137_🛠_Advanced_Injector",
    "138_💻_Encrypted_Communication", "139_🌐_Global_Network_Scanner", "140_⚡_NextGen_Hacking_Tool"
]

# ========================
# ফোল্ডার তৈরি এবং খালি main.py
# ========================
def setup_folders(base_path):
    for folder in FOLDERS:
        folder_path = os.path.join(base_path, folder)
        os.makedirs(folder_path, exist_ok=True)
        main_py = os.path.join(folder_path, "main.py")
        if not os.path.isfile(main_py):
            with open(main_py, "w") as f:
                f.write("#!/usr/bin/env python3\n# This is the main.py for {}\n".format(folder))
            os.chmod(main_py, 0o755)

# ========================
# ফিচার রান করা
# ========================
def run_feature(choice, base_path):
    if not choice.isdigit():
        print("[!] Invalid input.")
        return
    index = int(choice) - 1
    if index < 0 or index >= len(FOLDERS):
        print("[!] Invalid choice.")
        return
    folder = FOLDERS[index]
    main_py = os.path.join(base_path, folder, "main.py")
    if not os.path.isfile(main_py):
        print(f"[!] {folder}/main.py not found.")
        return
    # Run the feature's main.py
    subprocess.run(["python3", main_py])

# ========================
# Main launcher
# ========================
def main():
    base_path = os.path.dirname(os.path.abspath(__file__))
    setup_folders(base_path)

    print(BANNER)
    print("=== NetworkNinjas Launcher ===\n")

    # Print numbered folder list, 2 per line
    for i, folder in enumerate(FOLDERS, start=1):
        print(f"{i}. {folder:<40}", end="")
        if i % 2 == 0:
            print()
    if len(FOLDERS) % 2 != 0:
        print()
    print()

    choice = input("Enter your choice (number): ").strip()
    run_feature(choice, base_path)

if __name__ == "__main__":
    main()