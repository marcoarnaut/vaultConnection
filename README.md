================== VaultConnection v1.00 ==================

Author: zxcvntx  
Tested versions: 1.18  

# How connect API with Vault?  
1. Download plugin
2. Setup it on your server
3. Start server
4. Copy secureKey and connectionKey
   from /plugins/VaultConnection/config.yml
5. Connected if you see "Connected to server"
6. Use it with scripts:
   Example:
   
     ```
    import requests

    SERVER_URL = "http://217.18.61.249:8086"
    CONNECTION_KEY = "2d16faf1-c776-4a96-ae91-06aedc9d91c9"
    SECURE_KEY = "3514a794-7fcc-4fc4-992a-0bb7d0fdeef0"
    
    def get_balance(player_name):
        try:
            response = requests.post(
                f"{SERVER_URL}/{CONNECTION_KEY}",
                json={
                    "secureKey": SECURE_KEY,
                    "action": "get",
                    "player": player_name
                },
                timeout=5
            )
            response.raise_for_status()
            return response.json().get('balance')
        except requests.exceptions.RequestException as e:
            print(f"Error getting balance: {e}")
            return None
    
    def set_balance(player_name, amount):
        try:
            response = requests.post(
                f"{SERVER_URL}/{CONNECTION_KEY}",
                json={
                    "secureKey": SECURE_KEY,
                    "action": "set",
                    "player": player_name,
                    "value": amount
                },
                timeout=5
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error setting balance: {e}")
            return None
        
    if __name__ == "__main__":
        ping_response = requests.get(f"{SERVER_URL}/ping", timeout=3)
        set_result = set_balance("zxcvntx", 4000)
        print("Set result:", set_result)
        balance = get_balance("zxcvntx")
        print("Current balance:", balance)
    ```
