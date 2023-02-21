package hellfun;

import java.net.MalformedURLException;

import org.apache.commons.validator.UrlValidator;

public class check {

	public static void main(String[] args) {
		try {
			boolean hey=isValidURL("https://training210223igal200ilt-ap-south-1"
					+ "-1088228.saviyntcloud.com/ECM/login/auth");
		} catch (MalformedURLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	@SuppressWarnings("deprecation")
	static boolean isValidURL(String url) throws MalformedURLException {
	    UrlValidator validator = new UrlValidator();
	    return validator.isValid(url);
	}
}

	<dependency>
    <groupId>commons-validator</groupId>
    <artifactId>commons-validator</artifactId>
    <version>1.7</version>
