package com.example.urlchecker;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UrlCheckerController {

    @GetMapping("/")
    public String home(Model model) {
        return "index";
    }

    @GetMapping("/check")
    public String checkUrls(Model model) {
        String[] urls = {"http://example.com", "http://example2.com"};

        StringBuilder result = new StringBuilder();
        for (String url : urls) {
            try {
                int statusCode = UrlCheckerUtil.checkUrlStatus(url);
                result.append(url).append(": ").append(statusCode).append("<br>");
            } catch (Exception e) {
                result.append(url).append(": ").append("Error - ").append(e.getMessage()).append("<br>");
            }
        }

        model.addAttribute("result", result.toString());
        return "result";
    }
}
-----------------------
package com.example.urlchecker;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RequestCallback;
import org.springframework.web.client.ResponseExtractor;
import org.springframework.web.client.RestTemplate;

public class UrlCheckerUtil {

    public static int checkUrlStatus(String url) {
        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<Void> responseEntity = restTemplate.execute(url, null, null, new ResponseExtractor<ResponseEntity<Void>>() {
            @Override
            public ResponseEntity<Void> extractData(org.springframework.http.client.ClientHttpResponse clientHttpResponse) {
                return new ResponseEntity<>(clientHttpResponse.getStatusCode());
            }
        });
        return responseEntity.getStatusCodeValue();
    }
}
-------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>URL Checker</title>
</head>
<body>
    <h1>URL Checker</h1>
    <button onclick="checkUrls()">Check URLs</button>

    <script>
        function checkUrls() {
            window.location.href = "/check";
        }
    </script>
</body>
</html>
-------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>URL Checker Results</title>
</head>
<body>
    <h1>URL Checker Results</h1>
    <div th:utext="${result}"></div>
    <br>
    <a href="/">Go back</a>
</body>
</html>
