# TheTaskOne
First Task. A hello world application on ubuntu server and on ubuntu local machine with spring boot.

   After installing necessary programs, packages and setting needed plugins, I started a new project in IntelliJ IDEA. I chose there
"spring initializr" project because I wanted to have some options set up for me at first, like some dependencies. Then, I chose
Web applications only. 

   Later on, I had POM file and a main application class to manipulate.
 
   Firstly, my main class only was including:
 
 public static void main(String[] args) {
		SpringApplication.run(TheTaskOneApplication.class, args);
	}
	
    I wanted to saw if it was working with this class and i did not know starting my application from tomcat. I was running my 
application at first, without running tomcat. It was starting tomcat. So, after realized it, from edit configurations, I added 
a TomCat server. I changed dependencies, I added a new dependency to be able to use TomCat:

<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>
		
		I also defined 8081 port for tomcat to use. When I ran this tomcat server, it was invoking my application and it was opening 
localhost:8081 page in browser.

   After this, i added controller:
   
   @RestController
class GreetingController {

	@RequestMapping("/hello/{name}")
	String hello(@PathVariable String name) {
		return "Hello, welcome " + name + "!";
	}
}

  Then, I could see some output if i navigate to localhost:8081/hello/xxxx; giving xxxx for example.
  
  Later on, I created an ubuntu machine in virtual machine. I changed network adapter of it to bridged from NAT to be able to 
get its ip address. After getting ip address, i could connect and communicate to it with SSH from command line.

  But creating a war file and putting that war file to that remote ubuntu and making it deploy was not easy for me. From
project structure, I created a new Artifact, web application exploded, and gave a name ending with extension of .war. I changed 
the packaging to war from jar in POM.xml:

	<packaging>war</packaging>
	
	And in edit configurations, in deployment tab, I added this artifact i had created. So, when project ran, it was creating
this war file.

  With SCP, i copied this war file to home of remote server. Then I moved it to webapps folder of tomcat. Tomcat deployed it.
And i put the ip address of remote server(getting from ifconfig) to the browser and :8081 to see my output that i had seen in my local.
I thought it had to be 8081 port but it worked with 8080, default port. Because i had not changed the port of the server.

  I could see the output of remote ubuntu server from my ubuntu by navigating
  
  ipaddressofremote:8080/hello/xxxx
  
  And, after those, I added a new controller to handle error pages with giving responses to them:
  
@RestController
class IndexController implements ErrorController {

	private static final String PATH = "/error";

	@RequestMapping(value = PATH)
	public String error() {
		return "You wanted to go to invalid URL. Please check again.";
	}

	@Override
	public String getErrorPath() {
		return PATH;
	}
}

When URL is not continuing with /hello after port address, it gives this output. Also this works in remote ubuntu.
  
	
