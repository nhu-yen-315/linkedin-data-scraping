![Web scraping LinkedIn jobs (1)](https://github.com/user-attachments/assets/14b1648e-0c37-430d-9bba-02b345aba4de)

# WEB SCRAPING LINKEDIN JOB POSTINGS WITH PYTHON AND SCRAPFLY
 
Authors: Mai Nguyễn Thanh Phúc, Huỳnh Như Yến <br>
Date: 12/6/2025  
Tools Used: Python, Scrapfly-web scraping service

---
## 📑 Table of Contents  
1. 📌 Background & Overview 
2. 📂 The structure of LinkedIn website
3. 🔑 Scrape URLs of job posts from the job search page <br>
   3.1 Workflows <br>
   3.2 Codes <br>
4. 📄 Scrape job details <br>
  4.1 Workflows <br>
  4.2 Codes <br>
5. 🔎 Data output
6. ⛳️ Summary

---
## 1. 📌 Background & Overview

### Objective:
### 📖 What is this project about? 
- The project aims to collect LinkedIn job postings related to the business administration undegraduate major. The project only focuses on jobs located in Vietnam.
- Data output will then be used in another project (click to this link) to identify the most demanded skills by employers.


### 👤 Who is this project for? 
The project will benefit:

✔️ Data scientists who want to learn the procedure of scraping LinkedIn jobs <br>
✔️ Human resource managers for CV round <br>
✔️ Data analyst managers for CV round
### 🎯Project Outcome:  
(Number of job postings collected)

---
## 2. 📂 The structure of LinkedIn website

- When we search a job title from the job search page, the result contains a list of jobs on the left-hand side and the detail of a job post on the right-hand side (see the picture).
![image](https://github.com/user-attachments/assets/deb9f26a-651f-40f2-8113-d44b8fc815b6) <br>

- After suspecting the left-hand side panel HTML, we see the location of URLs as below: 
![image](https://github.com/user-attachments/assets/4229493e-9d14-4d7f-a0d8-ab997838a62f) <br>

- After suspecting the right-hand side panel HTML, we see the location of a job detail as below: 
![image](https://github.com/user-attachments/assets/a94076bc-ceef-4b88-a6da-b7267e7b3ec4) <br>

It is worth noting that we can't see all job details at the same time. We need to click on a specific job listing on the left-hand side to see its detail on the right-hand side. <br>
👉 Hence, we need to scrape the URLs of job listings first. Then, we access to each URL to scrape the job detail. The scraping procedure will be divided into two parts: <br>
    Part 1: Scrape URLs of job posts from the job search page <br>
    Part 2: Scrape job details
     
--- 
## 🔑 3.  Scrape URLs of job posts from the job search page
In this project, we will use job title 'Data analyst' as an example.

### 3.1 Workflows
**Step 1:** Open "Jobs" section
- In the navigation bar, click on "Jobs" icon.
- The formatted URL is https://www.linkedin.com/jobs/
<img width="1300" alt="image" src="https://github.com/user-attachments/assets/ad56fd10-c453-4413-a444-8fd75c9808d7" /> <br>

**Step 2:** Fill and search a job title in the search box
- In the search box, fill in the job title which you want to find. We will use "Data analyst" as an example.
- From the appearing URL https://www.linkedin.com/jobs/search/?currentJobId=4244696811&geoId=104195383&keywords=data%20analyst&origin=JOB_SEARCH_PAGE_KEYWORD_AUTOCOMPLETE&refresh=true, we see that "keywords" is an important parameter for the job title. Other parameters are not crucial.
- Hence, the formatted URL after entering the job title: https://www.linkedin.com/jobs/search/?keywords={JobTitle}

<img width="1307" alt="image" src="https://github.com/user-attachments/assets/2bba0079-d675-497c-bf02-474466c6ec94" /> <br>

**Step 3:** Filter the job posts by "Location" 
- We can filter job posts by location by filling in the location box. We just want to get job posts in Vietnam so we fill in Vietnam.
- In the appearing URL https://www.linkedin.com/jobs/search/?currentJobId=4244696811&geoId=104195383&keywords=data%20analyst&origin=JOB_SEARCH_PAGE_LOCATION_AUTOCOMPLETE&refresh=true, we see that Vietnam is assigned to a location ID which is geoId=104195383. The ID number is assigned by LinkedIn. 
- The formatted URL after filtering location: https://www.linkedin.com/jobs/search/?geoId={IDnumber}&keywords={JobTitle}
<img width="1305" alt="image" src="https://github.com/user-attachments/assets/7b05f7ad-41f3-47f6-b940-f3379aaedde3" /> <br>

**Step 4:** Filter the job posts by "Experience level" 
- We can filter job posts by experience level. In this example, we want to collect data analyst jobs at internship and entry levels.
- In the appearing URL https://www.linkedin.com/jobs/search/?currentJobId=4247339188&f_E=1%2C2&geoId=104195383&keywords=data%20analyst&origin=JOB_SEARCH_PAGE_JOB_FILTER&refresh=true, we see that experience level is identified by f_E parameter. There are 6 experience levels including Internship, Entry level, Associate, Mid-Senior level, Director, Executive corresponding to number 1 to 6 respectively. If select only one experience level, for example Internship level, the syntax is f_E=1. If select two levels (Internship and Entry level), the syntax is f_E=1%2C2. Similarly, if select three levels (Internship, Entry level, Associate), the syntax is f_E=1%2C2%3C3. 
- The formatted URL after filtering experience level: https://www.linkedin.com/jobs/search/?f_E={ExperienceID}&geoId={IDnumber}&keywords={JobTitle}
<img width="1312" alt="image" src="https://github.com/user-attachments/assets/e90a80d1-0539-4643-8f01-c41b3678a866" /> <br>


**Step 5:** Figure out how to jump to the next page with combined filters
- Scroll down the job list section, we see the number of pages. Each page contains 25 jobs. Hence, it is necessary to figure out how to jump to the next page.
<img width="676" alt="image" src="https://github.com/user-attachments/assets/f62af2cb-84eb-45fb-ba95-3a01d2181d48" />

- The URL of the second page is https://www.linkedin.com/jobs/search/?currentJobId=4239373647&f_E=1%2C2&geoId=104195383&keywords=data%20analyst&origin=JOB_SEARCH_PAGE_JOB_FILTER&refresh=true&start=25. start=25 specifies the second page starting with the job number 26 which has index 25. So the nth page is specified in URL as start={25*(n-1)}.

- The formatted URL to turn to nth page: https://www.linkedin.com/jobs/search/?f_E={ExperienceID}&geoId={IDnumber}&keywords={JobTitle}&start={25*(n-1)}
<img width="1305" alt="image" src="https://github.com/user-attachments/assets/eae07d09-8924-4eba-856c-44d9149691c2" />

### 3.2 Codes

#### Module 1: Import infrastructure from Scrapfly
- To get the key, we need to register an account with Scrapfly. Here is the sign-up link https://scrapfly.io/register. 
```python
import json
import asyncio
from loguru import logger as log
from scrapfly import ScrapeConfig, ScrapflyClient, ScrapeApiResponse
from lxml import html
from urllib.parse import urlencode, quote_plus

# Initialize Scrapfly client with your API key
SCRAPFLY = ScrapflyClient(key="scp-live-29eb1fea76d9446eac9f5ba0027653fc")

# Base configuration for scraping
BASE_CONFIG = {
    "asp": True,
    "country": "US",
    "headers": {
        "Accept-Language": "en-US,en;q=0.5"
    }
}
```

#### Module 2: Parse job urls and the total of results from a job search page
- The input is a response returned by Scrapfly after it sent the URL request to LinkedIn server.
- The response is a dictionary which has the key "content" storing HTML of a search page.
- We need to access the value of "content" key to parse the job URLs and the total number of results.
```python
async def parse_job_search(response: ScrapeApiResponse):
        # Get the HTML content which is assigned to the key "content" in the response dictionary
        content = response.scrape_result['content']
        
        # Parse the HTML content
        tree = html.fromstring(content)
        log.info(f"tree detail: {tree}")

        # Extract job URLs which have href attributes
        job_urls = tree.xpath('//a[contains(@class, "base-card__full-link")]/@href')
        urls = []
        for url in job_urls:
            urls.append(url)

        # Extract the number of total results
        total_results = tree.xpath("//span[@class='results-context-header__job-count']/text()")
        total_results = int(total_results[0].strip()) if total_results else 0

        # Store job URLs and the total number of results in a dictionary    
        result = {"urls": urls, "total_results": total_results}
        return result
```

#### Module 3: Parse urls from all search pages 
- The input is the job title which we want to find.
- The output is a list of job urls related to that job title.
```python
async def scrape_job_search(keyword: str, max_pages: int = None):
    
    def form_urls_params(keyword): #keyword is the job title which we want to search
        # form the job search URL params
        params = {'keywords': quote_plus(keyword)}
        return urlencode(params)

    # Get the response of the first page
    first_page_url = "https://www.linkedin.com/jobs/search?f_E=1%2C2&geoId=104195383&" + form_urls_params(keyword)
    first_page_response = await SCRAPFLY.async_scrape(ScrapeConfig(first_page_url, **BASE_CONFIG, render_js=True))
    
    # Scrape URLs of job listings in the first page and total results
    first_page_data = await parse_job_search(first_page_response)
    urls = first_page_data['urls']
    total_results = first_page_data['total_results']

    # Calculate the number of pages to scrape
    if max_pages and max_pages * 25 < total_results:
        total_results = max_pages * 25
    log.info(f'Scraped the first job page, {total_results // 25 - 1} more pages')

    # Scrape the remaining pages concurrently
    other_pages_url = "https://www.linkedin.com/jobs-guest/jobs/api/seeMoreJobPostings/search?f_E=1%2C2&geoId=104195383&"
    to_scrape = [ScrapeConfig(other_pages_url + form_urls_params(keyword) + f"&start={index}", **BASE_CONFIG, render_js=True)
                 for index in range(25, total_results + 25, 25)]
    
    async for response in SCRAPFLY.concurrent_scrape(to_scrape):
        if response.status_code == 200:
            result = await parse_job_search(response)  # Await this call
            page_urls = result['urls']
            urls.extend(page_urls)
            log.debug(f"Scraped {len(page_data)} jobs from this page. Total jobs collected: {len(urls)}")
        else:
            log.error(f"Failed to scrape: Status code {response.status_code}")
            return None

    log.success(f'Scraped {len(urls)} jobs from LinkedIn job search')
    return urls # Return a list of urls 
```
#### Module 4: Run the code
- We want to search for jobs related to keyword "financial risk management"; hence, we input that keyword to parameter "keyword" in function scrape_job_search.
- The result is saved to a JSON file and downloaded to the computer.
```python
async def run():
    job_search_data = await scrape_job_search(
        keyword="financial risk management", max_pages = 10
    )
    print(job_search_data)
    # Save the data to a JSON file
    with open("u2_financial_risk_management.json", "w", encoding="utf-8") as file:
        json.dump(job_search_data, file, indent=2, ensure_ascii=False)

# Directly use `await` in the interactive shell
await run()
```
#### Output
We obtain a list of URLs for the job title financial risk management.
<img width="1062" alt="image" src="https://github.com/user-attachments/assets/e4959d13-6924-47c7-862c-3ecd97fe5d6e" />

---
### 4. 📄 Scrape job details
After scraping URLs of job listings, we will send each URL to LinkedIn server via Scrapfly to get its detail information.
#### 4.1 Workflows 

#### 4.2 Codes 
#### Module 5: Input the list of URLs from the computer
- In this example, we will scrape details of financial risk management jobs for URLs we scraped in section 3. 
```python
file_path = '/Users/nhuyenhuynh/u2_financial_risk_management.json'  
with open(file_path, 'r', encoding='utf-8') as file:
    json_data = json.load(file)
urls = [item for item in json_data]
```

#### Module 6: Save parsed job details 
```python
# Function to save the descriptions to a .txt or .json file
def save_descriptions_to_file(descriptions):
    file_path_json = '/Users/nhuyenhuynh/d2_financial_risk_management.json'

    try:
        # Optionally save as .json file (formatted JSON)
        with open(file_path_json, 'w', encoding='utf-8') as file:
            json.dump(descriptions, file, ensure_ascii=False, indent=4)
        print(f"Descriptions saved to {file_path_json}")

    except Exception as e:
        print(f"Error saving descriptions to file: {e}")
```

#### Module 7: Scrape job details from URLs
- The job detail is stored in the selector //script[@type="application/ld+json".
- All job details are stored in a file.
```python
async def scrape_urls():
    to_scrape = [
        ScrapeConfig(url, **BASE_CONFIG) for url in urls
    ]
    
    # Initialize a list to store the cleaned descriptions
    descriptions = []

    async for response in SCRAPFLY.concurrent_scrape(to_scrape):
        try:
            if response.status_code == 200:
                # Get the HTML content
                content = response.scrape_result['content']
                
                # Parse the HTML content
                tree = html.fromstring(content)
                
                # Extract the JSON content from the <script> tag
                json_script = tree.xpath('//script[@type="application/ld+json"]/text()')
                
                # If the script is found and contains valid JSON, parse it
                if json_script:
                    try:
                        job_posting_data = json.loads(json_script[0])  # Parse the first matching script tag
                        description = job_posting_data.get("description", "")
                        
                        # Decode HTML entities (e.g., &lt;br&gt; to <br>)
                        description_cleaned = unescape(description)
                        
                        # Remove any remaining HTML tags
                        description_no_tags = re.sub(r'<.*?>', ' ', description_cleaned)
                        
                        # Append the cleaned description to the list
                        descriptions.append(description_no_tags)
                        
                
                    except json.JSONDecodeError as e:
                        print(f"Failed to decode JSON: {e}")
                else:
                    print("No <script> tag with type 'application/ld+json' found")
                
            else:
                log.error(f"Failed to scrape: Status code {response.status_code}")
                return None
                
        except Exception as e:
            log.error(f"Error processing response: {str(e)}")
            return None

    # Save the descriptions to a file after scraping is complete
    save_descriptions_to_file(descriptions)

    # Return descriptions if needed
    return 'finish the scraping'
```
#### Module 8: Run the code
```python
if __name__ == "__main__":
    if not asyncio.get_event_loop().is_running():
        result = asyncio.run(scrape_urls())
    else:
        result = await scrape_urls()
```
