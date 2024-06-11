<script type="text/javascript">
    var counter = 1; // Counter to keep track of screenshots

    function takeScreenshot() {
        var WshShell = new ActiveXObject("WScript.Shell");
        WshShell.SendKeys("{PRTSC}");

        setTimeout(function() {
            WshShell.SendKeys("%{F4}");
            saveScreenshot('p' + counter);
            counter++;
        }, 1000); // Delay for 1 second to allow time for the screenshot to be taken
    }

    function saveScreenshot(filename) {
        var WshShell = new ActiveXObject("WScript.Shell");
        WshShell.SendKeys("^{ESC}"); // Minimize all windows to show desktop

        var oIE = new ActiveXObject("InternetExplorer.Application");
        oIE.Visible = false;
        oIE.Navigate("about:blank");
        while (oIE.Busy) {
            WScript.Sleep(100);
        }

        var oShell = new ActiveXObject("WScript.Shell");
        oShell.SendKeys("^n");
        WScript.Sleep(500);
        oShell.SendKeys("^w");
        WScript.Sleep(500);
        oShell.SendKeys("^s");
        WScript.Sleep(500);
        oShell.SendKeys(filename);
        WScript.Sleep(500);
        oShell.SendKeys("{ENTER}");
        WScript.Sleep(1000);

        // Specify directory to save screenshots (modify the path as needed)
        var path = "C:\\Screenshots\\" + filename + ".png";
        oIE.Document.execCommand("SaveAs", false, path);
        WScript.Sleep(500);

        oIE.Quit();
    }
</script>
