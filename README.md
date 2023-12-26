<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Server Status Checker</title>
</head>
<body>

<script>
function checkServerStatus(url) {
    fetch(url)
        .then(response => {
            if (!response.ok) {
                throw new Error(`Server down: ${url}`);
            }
        })
        .catch(error => {
            // Handle server down scenario
            alert(error.message);
        });
}

// Specify the URLs of the servers you want to monitor
const serverUrls = ['http://example.com', 'http://example2.com'];

// Set the interval for checking server status (in milliseconds)
const checkInterval = 5000; // 5 seconds

// Start checking server status in a loop
const intervalId = setInterval(() => {
    serverUrls.forEach(url => {
        checkServerStatus(url);
    });
}, checkInterval);

// To stop the loop, you can use clearInterval like this:
// clearInterval(intervalId);
</script>

</body>
</html>
