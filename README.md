# My-selenium-script
Has code of framework that I have created

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public class SmartcomHomepageTest {

    private static WebDriver driver;
    private static WebDriverWait wait;

    private static final String BASE_URL = "https://smartcom.com/";
    private static final int TIMEOUT = 15;

    // ─────────────────────────────────────────────
    //  Setup & Teardown
    // ─────────────────────────────────────────────

    @BeforeAll
    public static void setUp() {
        // Automatically downloads & sets up ChromeDriver
        io.github.bonigarcia.wdm.WebDriverManager.chromedriver().setup();

        ChromeOptions options = new ChromeOptions();
        // options.addArguments("--headless");  // Uncomment to run without browser window
        options.addArguments("--start-maximized");
        options.addArguments("--disable-notifications");

        driver = new ChromeDriver(options);
        wait = new WebDriverWait(driver, Duration.ofSeconds(TIMEOUT));

        driver.get(BASE_URL);

        // Wait for page to fully load
        try { Thread.sleep(2000); } catch (InterruptedException e) { e.printStackTrace(); }
    }

    @AfterAll
    public static void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }

    // ─────────────────────────────────────────────
    //  1. Page Title & URL Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(1)
    @DisplayName("TC01 - Verify Page Title is not empty")
    public void testPageTitleNotEmpty() {
        String title = driver.getTitle();
        System.out.println("[Title] " + title);
        Assertions.assertFalse(title.isEmpty(), "Page title should not be empty");
    }

    @Test
    @Order(2)
    @DisplayName("TC02 - Verify Page Title contains relevant keywords")
    public void testPageTitleContainsKeywords() {
        String title = driver.getTitle().toLowerCase();
        boolean hasKeyword = title.contains("smartcom") || title.contains("wearable")
                          || title.contains("mobility") || title.contains("paas")
                          || title.contains("iot");
        Assertions.assertTrue(hasKeyword, "Page title should contain relevant keywords. Found: " + title);
    }

    @Test
    @Order(3)
    @DisplayName("TC03 - Verify Correct Page URL")
    public void testPageUrl() {
        String currentUrl = driver.getCurrentUrl();
        System.out.println("[URL] " + currentUrl);
        Assertions.assertTrue(currentUrl.contains("smartcom.com"),
                "URL should contain 'smartcom.com'. Found: " + currentUrl);
    }

    // ─────────────────────────────────────────────
    //  2. Navigation Links Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(4)
    @DisplayName("TC04 - Verify Home Navigation Link is visible")
    public void testNavHomeLink() {
        WebElement homeLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Home')]")));
        Assertions.assertTrue(homeLink.isDisplayed(), "Home nav link should be visible");
        System.out.println("[Nav] Home link href: " + homeLink.getAttribute("href"));
    }

    @Test
    @Order(5)
    @DisplayName("TC05 - Verify Solutions Navigation Link is visible")
    public void testNavSolutionsLink() {
        WebElement solutionsLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Solutions')]")));
        Assertions.assertTrue(solutionsLink.isDisplayed(), "Solutions nav link should be visible");
    }

    @Test
    @Order(6)
    @DisplayName("TC06 - Verify About Navigation Link is visible")
    public void testNavAboutLink() {
        WebElement aboutLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'About')]")));
        Assertions.assertTrue(aboutLink.isDisplayed(), "About nav link should be visible");
    }

    @Test
    @Order(7)
    @DisplayName("TC07 - Verify Contact Us Navigation Link is visible")
    public void testNavContactLink() {
        WebElement contactLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Contact')]")));
        Assertions.assertTrue(contactLink.isDisplayed(), "Contact nav link should be visible");
    }

    @Test
    @Order(8)
    @DisplayName("TC08 - Verify Blog Navigation Link is visible")
    public void testNavBlogLink() {
        WebElement blogLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Blog')]")));
        Assertions.assertTrue(blogLink.isDisplayed(), "Blog nav link should be visible");
    }

    @Test
    @Order(9)
    @DisplayName("TC09 - Verify Solutions dropdown contains Mobile Device Management")
    public void testSolutionsDropdownMDM() {
        WebElement solutionsLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Solutions')]")));
        solutionsLink.click();

        WebElement mdmLink = wait.until(ExpectedConditions.visibilityOfElementLocated(
                By.xpath("//a[contains(text(),'Mobile Device Management')]")));
        Assertions.assertTrue(mdmLink.isDisplayed(), "MDM dropdown link should be visible");

        // Click away to close dropdown
        driver.findElement(By.tagName("body")).click();
    }

    @Test
    @Order(10)
    @DisplayName("TC10 - Verify Solutions dropdown contains IoT Platform-as-a-Service")
    public void testSolutionsDropdownIoT() {
        WebElement solutionsLink = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//nav//a[contains(text(),'Solutions')]")));
        solutionsLink.click();

        WebElement iotLink = wait.until(ExpectedConditions.visibilityOfElementLocated(
                By.xpath("//a[contains(text(),'IoT Platform-as-a-Service')]")));
        Assertions.assertTrue(iotLink.isDisplayed(), "IoT PaaS dropdown link should be visible");

        // Click away to close dropdown
        driver.findElement(By.tagName("body")).click();
    }

    // ─────────────────────────────────────────────
    //  3. Hero / Banner Section Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(11)
    @DisplayName("TC11 - Verify Hero Heading H1 is present")
    public void testHeroHeading() {
        WebElement heading = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.tagName("h1")));
        String headingText = heading.getText().toLowerCase();
        System.out.println("[Hero H1] " + heading.getText());
        Assertions.assertFalse(headingText.isEmpty(), "Hero heading should not be empty");
        boolean hasKeyword = headingText.contains("paas") || headingText.contains("iot")
                          || headingText.contains("wearable") || headingText.contains("solutions")
                          || headingText.contains("family");
        Assertions.assertTrue(hasKeyword, "Hero heading should contain relevant keywords. Found: " + headingText);
    }

    @Test
    @Order(12)
    @DisplayName("TC12 - Verify Hero subheading mentions 'communicate' or 'securely'")
    public void testHeroSubheading() {
        WebElement subtext = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'communicate') or contains(text(),'securely')]")));
        Assertions.assertTrue(subtext.isDisplayed(), "Hero sub-heading should be visible");
        System.out.println("[Hero Sub] " + subtext.getText());
    }

    @Test
    @Order(13)
    @DisplayName("TC13 - Verify Hero banner image is present")
    public void testHeroBannerImage() {
        WebElement bannerImage = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//img[contains(@src,'Home-1A')]")));
        Assertions.assertTrue(bannerImage.isDisplayed(), "Hero banner image should be visible");
    }

    // ─────────────────────────────────────────────
    //  4. Our Advantages Section Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(14)
    @DisplayName("TC14 - Verify 'Our Advantages' section is present")
    public void testAdvantagesSectionPresent() {
        WebElement advantages = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Our Advantages')]")));
        Assertions.assertTrue(advantages.isDisplayed(), "'Our Advantages' section should be visible");
    }

    @Test
    @Order(15)
    @DisplayName("TC15 - Verify '30 Million end-users' stat is visible")
    public void test30MillionStat() {
        WebElement stat = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'30 Million')]")));
        Assertions.assertTrue(stat.isDisplayed(), "30 Million end-users stat should be visible");
    }

    @Test
    @Order(16)
    @DisplayName("TC16 - Verify 'Fast Integration' advantage card is present")
    public void testFastIntegrationCard() {
        WebElement card = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Fast') and contains(text(),'Integration')]")));
        Assertions.assertTrue(card.isDisplayed(), "'Fast Integration' card should be visible");
    }

    @Test
    @Order(17)
    @DisplayName("TC17 - Verify 'Proven Solutions' advantage card is present")
    public void testProvenSolutionsCard() {
        WebElement card = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Proven') and contains(text(),'Solutions')]")));
        Assertions.assertTrue(card.isDisplayed(), "'Proven Solutions' card should be visible");
    }

    @Test
    @Order(18)
    @DisplayName("TC18 - Verify 'Advanced Customization' advantage card is present")
    public void testAdvancedCustomizationCard() {
        WebElement card = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Advanced') and contains(text(),'Customization')]")));
        Assertions.assertTrue(card.isDisplayed(), "'Advanced Customization' card should be visible");
    }

    @Test
    @Order(19)
    @DisplayName("TC19 - Verify 'Security & Monitoring' advantage card is present")
    public void testSecurityMonitoringCard() {
        WebElement card = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Security') and contains(text(),'Monitoring')]")));
        Assertions.assertTrue(card.isDisplayed(), "'Security & Monitoring' card should be visible");
    }

    // ─────────────────────────────────────────────
    //  5. Core Solutions Section Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(20)
    @DisplayName("TC20 - Verify 'Our Core Solutions' section is present")
    public void testCoreSolutionsSection() {
        WebElement section = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'Core Solutions')]")));
        Assertions.assertTrue(section.isDisplayed(), "'Our Core Solutions' section should be visible");
    }

    @Test
    @Order(21)
    @DisplayName("TC21 - Verify Mobile Device Management solution card is present")
    public void testMDMSolutionCard() {
        List<WebElement> cards = driver.findElements(
                By.xpath("//h2[contains(text(),'Mobile Device Management')]"));
        Assertions.assertFalse(cards.isEmpty(), "MDM solution card should be present");
        Assertions.assertTrue(cards.get(0).isDisplayed(), "MDM solution card should be visible");
    }

    @Test
    @Order(22)
    @DisplayName("TC22 - Verify IoT Platform-as-a-Service solution card is present")
    public void testIoTPaaSCard() {
        List<WebElement> cards = driver.findElements(
                By.xpath("//h2[contains(text(),'IoT Platform')]"));
        Assertions.assertFalse(cards.isEmpty(), "IoT PaaS solution card should be present");
        Assertions.assertTrue(cards.get(0).isDisplayed(), "IoT PaaS solution card should be visible");
    }

    @Test
    @Order(23)
    @DisplayName("TC23 - Verify at least 2 'LEARN MORE' buttons are present")
    public void testLearnMoreButtons() {
        List<WebElement> buttons = driver.findElements(
                By.xpath("//a[contains(text(),'LEARN MORE')]"));
        System.out.println("[Buttons] LEARN MORE count: " + buttons.size());
        Assertions.assertTrue(buttons.size() >= 2,
                "Expected at least 2 LEARN MORE buttons, found: " + buttons.size());
    }

    @Test
    @Order(24)
    @DisplayName("TC24 - Verify 'LEARN MORE' for MDM navigates to correct page")
    public void testLearnMoreMDMNavigation() {
        WebElement learnMoreMDM = wait.until(ExpectedConditions.elementToBeClickable(
                By.xpath("(//a[contains(text(),'LEARN MORE')])[1]")));
        learnMoreMDM.click();

        wait.until(ExpectedConditions.urlContains("device-management"));
        String url = driver.getCurrentUrl();
        System.out.println("[Navigation] MDM URL: " + url);
        Assertions.assertTrue(url.contains("device-management"),
                "Should navigate to MDM page. Found URL: " + url);

        // Navigate back to homepage
        driver.navigate().back();
        wait.until(ExpectedConditions.urlToBe(BASE_URL));
    }

    // ─────────────────────────────────────────────
    //  6. Logo Test
    // ─────────────────────────────────────────────

    @Test
    @Order(25)
    @DisplayName("TC25 - Verify Smartcom logo is present")
    public void testLogoPresent() {
        WebElement logo = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//img[contains(@src,'logo') or contains(@src,'smartcom')]")));
        Assertions.assertTrue(logo.isDisplayed(), "Smartcom logo should be visible");
        System.out.println("[Logo] src: " + logo.getAttribute("src"));
    }

    @Test
    @Order(26)
    @DisplayName("TC26 - Verify Logo links back to Homepage")
    public void testLogoNavigatesToHome() {
        WebElement logoLink = wait.until(ExpectedConditions.elementToBeClickable(
                By.xpath("//a[contains(@href,'smartcom.com')]//img[contains(@src,'logo')]/..")));
        String href = logoLink.getAttribute("href");
        System.out.println("[Logo] href: " + href);
        Assertions.assertTrue(href.contains("smartcom.com"),
                "Logo should link to homepage. Found: " + href);
    }

    // ─────────────────────────────────────────────
    //  7. Footer Tests
    // ─────────────────────────────────────────────

    @Test
    @Order(27)
    @DisplayName("TC27 - Verify Footer contact email is present")
    public void testFooterEmail() {
        WebElement email = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[contains(@href,'mailto:contact@smartcom.com')]")));
        Assertions.assertTrue(email.isDisplayed(), "Contact email should be visible in footer");
        System.out.println("[Footer] Email: " + email.getText());
    }

    @Test
    @Order(28)
    @DisplayName("TC28 - Verify Footer US phone number is present")
    public void testFooterUSPhone() {
        WebElement phone = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[contains(@href,'tel:+14049070009')]")));
        Assertions.assertTrue(phone.isDisplayed(), "US phone number should be visible in footer");
    }

    @Test
    @Order(29)
    @DisplayName("TC29 - Verify Footer LinkedIn link is present")
    public void testFooterLinkedIn() {
        WebElement linkedin = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[contains(@href,'linkedin.com/company/smartcom')]")));
        Assertions.assertTrue(linkedin.isDisplayed(), "LinkedIn link should be visible in footer");
    }

    @Test
    @Order(30)
    @DisplayName("TC30 - Verify Footer Privacy Policy link is present")
    public void testFooterPrivacyPolicy() {
        WebElement privacy = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[contains(text(),'Privacy Policy')]")));
        Assertions.assertTrue(privacy.isDisplayed(), "Privacy Policy link should be visible in footer");
    }

    @Test
    @Order(31)
    @DisplayName("TC31 - Verify Footer Data Protection Agreement link is present")
    public void testFooterDataProtectionAgreement() {
        WebElement dpa = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[contains(text(),'Data Protection Agreement')]")));
        Assertions.assertTrue(dpa.isDisplayed(), "Data Protection Agreement link should be visible");
    }

    @Test
    @Order(32)
    @DisplayName("TC32 - Verify Footer copyright notice mentions SMARTCOM")
    public void testFooterCopyright() {
        WebElement copyright = wait.until(ExpectedConditions.presenceOfElementLocated(
                By.xpath("//*[contains(text(),'SMARTCOM') and contains(text(),'Copyright')]")));
        Assertions.assertTrue(copyright.isDisplayed(), "Copyright notice should be visible in footer");
        System.out.println("[Footer] Copyright: " + copyright.getText());
    }
}
