![Web scraping LinkedIn jobs (1)](https://github.com/user-attachments/assets/14b1648e-0c37-430d-9bba-02b345aba4de)

# WEB SCRAPING LINKEDIN JOB POSTINGS WITH PYTHON AND SCRAPFLY
 
Authors: Mai Nguyễn Thanh Phúc, Huỳnh Như Yến <br>
Date: 12/6/2025  
Tools Used: Python, Scrapfly-web scraping service

---
## 📑 Table of Contents  
1. 📌 Background & Overview 
2. 📂 The structure of LinkedIn website
3. 🧠 Workflows <br>
   3.1 Scrape URLs of job posts from the job search page <br>
   3.2 Scrape job descriptions from job pages
5. 📊 Modules
6. 🔎 Data output
7. ⛳️ Summary

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


--- 
## 3. 🧠 Workflows
We will collect job postings by searching a specific job title. There are two main steps. The first step is to scrape a list of job URLS via the job search page. The second step is to scrape job details of each job link via the job page. 
In this project, we will use job title 'Data analyst' as an example.

### 3.1 Scrape URLs of job posts from the job search page
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



### 3.2 Scrape job descriptions from job pages 
**Step 1:** 
