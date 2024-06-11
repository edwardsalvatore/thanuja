<!DOCTYPE html>
<html>
<head>
<title>Screen Capture HTA</title>
<HTA:APPLICATION ID="oMyApp" 
    APPLICATIONNAME="ScreenCaptureHTA"
    BORDER="thin"
    CAPTION="yes"
    SHOWINTASKBAR="yes"
    SINGLEINSTANCE="yes"
    SYSMENU="yes"
    WINDOWSTATE="normal">
<script type="text/javascript" src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
<script type="text/javascript">
    function takeScreenshot(filename) {
        var button = document.getElementById('screenshotButton');
        button.disabled = true; // Disable the button during the process
        
        html2canvas(document.body).then(function(canvas) {
            var link = document.createElement("a");
            document.body.appendChild(link);
            link.download = filename + ".png";
            link.href = canvas.toDataURL();
            link.click();
            
            button.disabled = false; // Enable the button after the process is complete
        });
    }
</script>
</head>
<body>
    <h1>Screen Capture HTA</h1>
    <button id="screenshotButton" onclick="takeScreenshot('p1')">Take Screenshot</button>
</body>
</html>
