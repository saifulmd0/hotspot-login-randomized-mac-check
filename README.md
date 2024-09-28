# hotspot-login-randomized-mac-check
Check if the user has enabled the randomized mac address in their wifi settings

Add this code to your hotspot's login.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hotspot Login</title>
    <script>
        // Function to detect private (randomized) MAC addresses based on the second character
        function detectPrivateMac() {
            // Get the MAC address passed dynamically by MikroTik
            const macAddress = '$(mac)'; // MikroTik will replace $(mac) with the actual MAC address

            if (macAddress) {
                // Check the second character of the MAC address (after the first character in the first octet)
                const secondChar = macAddress.charAt(1).toUpperCase();

                // If the second character is 2, 6, A, or E, it's a randomized MAC address
                if (['2', '6', 'A', 'E'].includes(secondChar)) {
                    // If a randomized MAC address is detected, show a warning message
                    document.getElementById('error-message').style.display = 'block';
                    document.getElementById('login-form').style.display = 'none';
                }
            }
        }

        // Call the function when the page loads
        window.onload = detectPrivateMac;
    </script>
</head>
<body>
    <div id="error-message" style="display:none; color: red; text-align: center;">
        <h2>Randomized MAC Address Detected</h2>
        <p>To login to the network, please turn off Private MAC (Randomized MAC) in your device's Wi-Fi settings.</p>
        <p>Once you've disabled Private MAC, try logging in again.</p>
    </div>

    <div id="login-form" style="display: block;">
        <form action="login" method="post">
            <h2>Welcome to Our Wi-Fi</h2>
            <p>Please enter your username and password to login.</p>
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" required><br>
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" required><br>
            <input type="submit" value="Login">
        </form>
    </div>
</body>
</html>
```
