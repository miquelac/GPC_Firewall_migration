# GPC_Firewall_migration Mlv6

Mlv6 Migration involves the activation of the checkbox, for this bot management has to be enabled.
Enable bot management can be found at: Overview/Firewall/bots

<img width="926" alt="Botmanagement" src="https://user-images.githubusercontent.com/125895480/226590298-e936a403-2a78-46ef-8df8-115494c3aaee.png">

<br>


As small script has been developed in python in order to automate the process of clicking


* We have a function that prepare the chromedriver to load with user profile, this avoids to get detected as a bot.
* Secondly it looks for the checkbox element once in the correct url, waits for the url to be ready and click the checkbox


``` python
def click_toggle_checkbox(url):
        options = webdriver.ChromeOptions()
        #options.add_argument("--headless")
        #options.add_argument("--no-sandbox")
        options.add_argument(r"--user-data-dir=/home/<USERNAME>/.config/google-chrome/")
        options.add_argument(r'--profile-directory=Default')
        driver = webdriver.Chrome('./chromedriver',options=options)
        driver.get(url)
        try:
            driver.implicitly_wait(15)
            element =driver.find_element(By.XPATH, "//input[@type='checkbox']")
            element.click()
            time.sleep(4)
            print('', element ,'\n')
        finally:
            driver.quit

```

* To prepare the url we use a list file .xlsx that we read from and use the data we require.
* We take the account and the name as the middle url
* Use the domain variable for the domain
* subdirectory variable is used to add last piece of string in the composed url

``` python

domain = 'https://dash.cloudflare.com/'
subdirectory ='/security/bots/configure'
excel_data = pd.read_excel('GPC_Toggle.xlsx', sheet_name='Sheet1', usecols=['account/id', 'name'])

```
<br>

* The loop shown below iterates through the excel file retrieving account/id and name 
* after composing the url it sends it through the function of click_toggle_checkbox(url_compose)
* This will loop until the last row in the xlsx 

``` bash 

for row in excel_data.iterrows():
    path_account = row[1]['account/id']
    path_site = row[1]['name']
    url_compose = domain + path_account + '/' + path_site #+ subdirectory
    click_toggle_checkbox(url_compose)

```

<br>

### Output of the composed urls ( modified )

``` bash 

https://dash.cloudfxxxxxxxjust_for_testing_pruposesxxxxxxnpt.net/security/bots/configure
https://dash.cloudfnpt.com/middle url goes here /security/bots/configure
https://dash.cloudflare.com/2d68f5667dasd76ds676s968f7d726981f7c2/example-api.dev.tsakd.com/security/bots/configure
https://dash.example.com/2das2343bf568etestingf7c2/customer.dev.pt.com/security/bots/configure
https://dash.cloudflare.com/2d67777777777777777981f7c2/delivery-access.test.com/security/bots/configure

```

