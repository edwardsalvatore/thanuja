<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Server Status Checker</title>
</head>
<body>

<button onclick="startChecking()">Start Checking</button>
<button onclick="stopChecking()">Stop Checking</button>

<script>
let intervalId;

function checkServerStatus(url) {
    const xhr = new XMLHttpRequest();
    
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
            if (xhr.status === 200) {
                console.log(`Server is up: ${url}`);
            } else {
                console.error(`Server is down: ${url}`);
            }
        }
    };

    xhr.open('GET', url, true);
    xhr.send();
}

function startChecking() {
    // Specify the URLs of the servers you want to monitor
    const serverUrls = ['http://example.com', 'http://example2.com'];

    // Set the interval for checking server status (in milliseconds)
    const checkInterval = 5000; // 5 seconds

    // Start checking server status in a loop
    intervalId = setInterval(() => {
        serverUrls.forEach(url => {
            checkServerStatus(url);
        });
    }, checkInterval);

    console.log('Checking started.');
}

function stopChecking() {
    // Stop the loop
    clearInterval(intervalId);

    console.log('Checking stopped.');
}
</script>

</body>
</html>
