// ... existing imports

import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class UrlCheckerController {

    @GetMapping("/")
    public String home(Model model) {
        return "index";
    }

    @GetMapping("/check")
    public String checkUrls(@RequestParam(required = false) String url, Model model) {
        if (url == null) {
            model.addAttribute("result", "URL attribute is not specified.");
        } else {
            String[] urls = {url};

            StringBuilder result = new StringBuilder();
            for (String u : urls) {
                try {
                    int statusCode = UrlCheckerUtil.checkUrlStatus(u);
                    result.append(u).append(": ").append(statusCode).append("<br>");
                } catch (Exception e) {
                    result.append(u).append(": ").append("Error - ").append(e.getMessage()).append("<br>");
                }
            }

            model.addAttribute("result", result.toString());
        }
        return "result";
    }
}
---------------------------
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.h2.console.enabled=true
spring.datasource.platform=h2
