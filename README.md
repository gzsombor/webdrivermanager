# WebDriverManager [![][Logo]][GitHub Repository]

This piece of software is a small library aimed to automate the [Selenium Webdriver] binaries management within a Java project in runtime.

If you have ever used [Selenium Webdriver], you probably know that in order to use some browsers (for example **Chrome**, **Internet Explorer**, **Opera**, or **Microsoft Edge**) you need to download a binary which allows WebDriver to handle the browser. In addition, the absolute path to this binary must be set as Java variables, as follows:

	System.setProperty("webdriver.chrome.driver", "/absolute/path/to/binary/chromedriver");
	System.setProperty("webdriver.opera.driver", "/absolute/path/to/binary/operadriver");
	System.setProperty("webdriver.ie.driver", "C:/absolute/path/to/binary/IEDriverServer.exe");
	System.setProperty("webdriver.edge.driver", "C:/absolute/path/to/binary/MicrosoftWebDriver.exe");

This is quite annoying since it forces you to link directly this binary in your source code. In addition, you have to check manually when new versions of the binaries are released. This library comes to the rescue, performing in an automated way all this dirty job for you.

WebDriverManager is open source, released under the terms of [LGPL License].

## Usage

In order to use WebDriverManager in a Maven project, first add the following dependency to your `pom.xml`:

	<dependency>
		<groupId>io.github.bonigarcia</groupId>
		<artifactId>webdrivermanager</artifactId>
		<version>1.3.0</version>
	</dependency>

Then you can let WebDriverManager to do manage WebDriver binaries for your application/test. Take a look to this JUnit example which uses Chrome with Selenium WebDriver:

	public class ChromeTest {

		protected WebDriver driver;

		@BeforeClass
		public static void setupClass() {
			ChromeDriverManager.getInstance().setup();
		}

		@Before
		public void setupTest() {
			driver = new ChromeDriver();
		}

		@After
		public void teardown() {
			if (driver != null) {
				driver.quit();
			}
		}

		@Test
		public void test() {
			// Using Selenium WebDriver to carry out automated web testing
		}

	}

Notice that simple adding ``ChromeDriverManager.getInstance().setup();`` WebDriverManager does magic for you:

1. It checks the latest version of the WebDriver binary file
2. It downloads the binary WebDriver if it is not present in your system
3. It exports the required Java variable by Selenium WebDriver

So far, WebDriverManager supports **Chrome**, **Opera**, **Internet Explorer**, and **Microsoft Edge** as follows:

	ChromeDriverManager.getInstance().setup();
	InternetExplorerDriverManager.getInstance().setup();
	OperaDriverManager.getInstance().setup();
	EdgeDriverManager.getInstance().setup();

## Advanced

Configuration parameters for WebDriverManager are set in the ``application.properties`` file:

	wdm.targetPath=~/.m2/repository/webdriver
	wdm.override=false
	wdm.timeout=30

	wdm.chromeDriverUrl=http://chromedriver.storage.googleapis.com/
	wdm.chromeDriverExport=webdriver.chrome.driver
	wdm.chromeDriverVersion=LATEST

	wdm.operaDriverUrl=https://api.github.com/repos/operasoftware/operachromiumdriver/releases
	wdm.operaDriverExport=webdriver.opera.driver
	wdm.operaDriverVersion=LATEST

	wdm.internetExplorerDriverUrl=http://selenium-release.storage.googleapis.com/
	wdm.internetExplorerExport=webdriver.ie.driver
	wdm.internetExplorerVersion=LATEST

	wdm.edgeDriverUrl=https://www.microsoft.com/en-us/download/details.aspx?id=48212
	wdm.edgeExport=webdriver.edge.driver
	wdm.edgeVersion=LATEST

The variable ``wdm.targetPath`` is the default folder in which WebDriver binaries are going to be stored. Notice that by default the path of the Maven local repository is used. The URLs to check the latest version of Chrome, Opera, and Internet Explorer are set using the variables ``wdm.chromeDriverUrl``, ``wdm.operaDriverExport``, ``wdm.operaDriverUrl``, and ``wdm.edgeDriverUrl``. 

This properties can be overwritten with Java system properties. For example:

	System.setProperty("wdm.targetPath", "/my/custom/path/to/driver/binaries");

... or by command line, for example:

	-Dwdm.override=true

In addition, the usage of a architecture (32 or 64 bits) can be forced (except for Edge driver). By default, the suitable binary version for your system and architecture is downloaded and used. The architecture can be forced as follows:

	new ChromeDriverManager().setup(Architecture.x32);
	new ChromeDriverManager().setup(Architecture.x64);

	new InternetExplorerDriverManager().setup(Architecture.x32);
	new InternetExplorerDriverManager().setup(Architecture.x64);

	new OperaDriverManager().setup(Architecture.x32);
	new OperaDriverManager().setup(Architecture.x64);

By default, WebDriverManager downloads the latest version of the WebDriver binary. Concrete versions of WebDriver binaries can be forced, for instance: 

	ChromeDriverManager.getInstance().setup("2.20");

	InternetExplorerDriverManager.getInstance().setup("2.46");

	OperaDriverManager.getInstance().setup("0.2.0");
	
	EdgeDriverManager.getInstance().setup("8D0D08CF-790D-4586-B726-C6469A9ED49C");

This can also be done by changing the value of the variables ``wdm.chromeDriverVersion``, ``wdm.operaDriverVersion``,  ``wdm.internetExplorerVersion``, or  ``wdm.edgeVersion`` from its default value (``LATEST``) to a concrete version. For instance:

	-Dwdm.chromeDriverVersion=2.20

	-Dwdm.internetExplorerVersion=2.46

	-Dwdm.operaDriverVersion=0.2.0

	-Dwdm.edgeVersion=8D0D08CF-790D-4586-B726-C6469A9ED49C

## About

WebDriverManager (Copyright &copy; 2015) is a personal project of [Boni Garcia] licensed under [LGPL License]. Comments, questions and suggestions are always very welcome!

[Logo]: http://bonigarcia.github.io/img/webdrivermanager.png
[Selenium Webdriver]: http://docs.seleniumhq.org/projects/webdriver/
[LGPL License]: http://www.gnu.org/licenses/lgpl-2.1.html
[Boni Garcia]: http://bonigarcia.github.io/
[GitHub Repository]: https://github.com/bonigarcia/webdrivermanager
