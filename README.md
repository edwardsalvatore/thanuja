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
<script type="text/javascript">
    function takeScreenshot(filename) {
        var button = document.getElementById('screenshotButton');
        button.disabled = true; // Disable the button during the process
        
        var WshShell = new ActiveXObject("WScript.Shell");
        WshShell.SendKeys("{PRTSC}");
        
        setTimeout(function() {
            WshShell.SendKeys("%{F4}");
            button.disabled = false; // Enable the button after the process is complete
        }, 1000); // Delay for 1 second to allow time for the screenshot to be taken
    }
</script>
</head>
<body>
    <h1>Screen Capture HTA</h1>
    <button id="screenshotButton" onclick="takeScreenshot('p1')">Take Screenshot</button>
</body>
</html>
