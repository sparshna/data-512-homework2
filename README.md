# Data-512 Homework 2: Considering Bias in Data

## Project Overview
This project explores bias in data using Wikipedia articles about politicians from various countries. The goal is to analyze the coverage and quality of Wikipedia articles on political figures relative to country populations. The project combines datasets of Wikipedia articles and country populations, retrieves article quality estimates using the ORES machine learning tool, and performs an analysis of how article coverage and quality vary across countries and regions. The final output includes tables ranking countries and regions by article coverage and high-quality content, as well as a reflection on biases observed in the data and their implications for data science projects.

## Research Implications

- The analysis of Wikipedia articles by country and region reveals interesting insights into the coverage and quality of information on the platform. The findings suggest that there are biases in the data, with certain countries and regions having higher coverage and quality of articles compared to others. These biases can be attributed to factors such as language, geography, culture, and politics, which influence the availability and accuracy of information on Wikipedia. Addressing these biases is crucial to ensure a more balanced and inclusive representation of global knowledge on the platform.
- The results highlight the need for more comprehensive and diverse data sources to overcome the limitations of Wikipedia as a data source. While Wikipedia provides valuable information, its coverage and quality may not be representative of all countries and regions worldwide. By incorporating data from multiple sources and ensuring data integrity and diversity, researchers can mitigate biases and enhance the reliability of their analyses and models.
- One surprising finding was the high number of articles per capita in countries like Tuvalu and Monaco, which have very small populations. While these countries have a high number of articles per capita, the metric may not accurately reflect the coverage and quality of information due to their small population sizes. This underscores the importance of considering population size and other contextual factors when interpreting data and drawing conclusions from it.

### QUESTION:
- What biases did you expect to find in the data (before you started working with it), and why?
### ANSWER:
I expected to find biases in the data due to the following reasons:
- 1. Language bias: Wikipedia is primarily available in English, which may lead to overrepresentation of English-speaking countries and underrepresentation of non-English-speaking countries.
- 2. Geographic bias: Countries with higher internet penetration rates and better access to technology may have more articles on Wikipedia compared to countries with lower internet penetration rates.
- 3. Cultural bias: Countries with a strong tradition of documenting historical and cultural information may have more articles on Wikipedia compared to countries with less emphasis on documentation.
- 4. Political bias: Countries with more political stability and freedom of speech may have more articles on Wikipedia compared to countries with less political stability and freedom of speech.
     
### --------

### QUESTION:
- What might your results suggest about (English) Wikipedia as a data source?
### ANSWER:
The results suggest that (English) Wikipedia may have biases in terms of coverage and quality of articles. The top countries by coverage and quality are mostly English-speaking countries or countries with high internet penetration rates. This indicates that Wikipedia may not be a representative source of information for all countries and regions globally. There is a need to address these biases to ensure a more balanced and inclusive representation of information on Wikipedia.

### --------

### QUESTION:
- What might your results suggest about the internet and global society in general?
### ANSWER:
The results suggest that the internet and global society may have biases in terms of information representation and accessibility. Countries with better internet infrastructure and higher literacy rates are more likely to have higher coverage and quality of articles on Wikipedia. This indicates that there is a digital divide between countries with access to information and those without. Addressing these biases is essential to promote a more equitable and inclusive representation of information on the internet and in global society.

### --------

### QUESTION:
- Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?
### ANSWER:
A realistic situation where using Wikipedia data to train a model or conduct hypothesis-driven research might lead to biased or misleading results is in a project focused on analyzing global political representation or historical coverage. For instance, if a model were trained to predict the global prominence of political figures based on Wikipedia articles, it might overestimate the importance of politicians from countries with higher internet access, more active Wikipedia communities, or dominant English-speaking populations, while underestimating figures from regions with less representation. This would lead to skewed results that misrepresent global political influence, disproportionately favoring countries with higher Wikipedia article coverage. The same issue could arise in business decision-making contexts, such as market analysis, where models might mistakenly assume higher public interest or prominence in certain regions based on the number of articles or article quality.

### --------

## Data Description and Provenance

### Data Source
The dataset used for this analysis combines information about Wikipedia articles of politicians and country populations. The two main datasets are provided as CSV files:
- `politicians_by_country.AUG.2024.csv`: This dataset contains a list of Wikipedia articles about politicians from various countries. It was generated by crawling the Wikipedia Category by nationality.
- `population_by_country_AUG.2024.csv`: This dataset contains population data for each country and was sourced from the World Population Data Sheet, published by the Population Reference Bureau.
- [Wikipedia Politicians Dataset](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality): The list of Wikipedia pages was extracted by crawling relevant categories in Wikipedia to identify articles about politicians. This data is likely a subset and may include inconsistencies or irrelevant pages.
- [World Population Data](https://www.prb.org/international/indicator/population/table/): The population dataset provides country-level and regional population estimates for the analysis of article coverage.


### API Usage
The article quality data was retrieved using the Wikimedia ORES API, a machine learning system that estimates the quality of Wikipedia articles. Each article in the politicians dataset was assigned a quality rating (FA, GA, B, C, Start, Stub) based on the most current revision ID of the page.

The ORES API was queried as follows:
- The current revision ID of each Wikipedia article was obtained using the MediaWiki API Info request.
- The ORES API was used to predict the quality class of each article based on its revision ID.

The API endpoint used for data collection was:

```python
ORES_API_ENDPOINT = 'https://ores.wikimedia.org/v3/scores/{project}/?revids={revid}&models=wp10'
```

Attribution: The data from Wikipedia and ORES is licensed under CC-BY by the Wikimedia Foundation.

#### IMPORTING ACCESS TOKEN

To use the ORES API, you need an access token to authenticate your requests. Here’s how you can create and manage the access token for your project:

Steps to Generate ORES API Access Token:
- **Create a Wikimedia Developer Account**:
Visit the Wikimedia API Developer Portal and create an account.
After registering, log in and go to the Wikimedia OAuth page.
- **Request an OAuth Access Token**:
Once logged in, apply for an OAuth access token by following the instructions provided. You may need to describe your project and the intended use of the ORES API.
After submitting your request, you will be provided with a token that allows you to authenticate your API requests.
- **Storing the Token**:
Best practice is to store the API token securely. You can either set it as an environment variable in your development environment or use a secret manager.
In a local environment, you can store the token as follows (example for Unix-based systems):
```bash
export ORES_API_KEY='your-access-token'
```
- **Using the Token in Code**:
In your code, you can access the token using the `os` module in Python:
```python
import os
ORES_API_KEY = os.getenv('ORES_API_KEY')
```

Then, include the token in the headers when making API requests to authenticate:
```python
headers = {
    'Authorization': f'Bearer {ORES_API_KEY}'
    'User-Agent': "<your_uw_netid@uw.edu>, University of Washington, MSDS DATA 512 - AUTUMN 2024"
}
```

## Files in the Repository

- [Code Notebook](homework.ipynb): Jupyter Notebook containing the full code for data collection, API requests, data cleaning, analysis, and visualization. The notebook includes steps for fetching Wikipedia article revision IDs, retrieving ORES article quality predictions, merging datasets, and calculating various coverage metrics for the analysis.
- [politicians_by_country.AUG.2024.csv](politicians_by_country.AUG.2024.csv): The dataset containing Wikipedia articles about politicians categorized by their nationality. This file was used as the base to extract article titles for subsequent quality analysis.
- [population_by_country_AUG.2024.csv](population_by_country_AUG.2024.csv): The dataset containing population data for countries and regions. This file was used to calculate articles-per-capita and high-quality-articles-per-capita for the analysis.
- [wp_politicians_by_country.csv](wp_politicians_by_country.csv): A merged dataset that includes country, region, population, article title, revision ID, and ORES quality score. This dataset is the result of combining the politician articles data and population data after quality predictions were fetched via the ORES API.
- [wp_countries-no_match.txt](wp_countries-no_match.txt): A text file listing the countries that appeared in the politicians dataset but did not have a match in the population dataset.
- [missing_revision_ids.txt](missing_revision_ids.txt): A log file containing articles for which revision IDs could not be retrieved during the API request process, resulting in missing quality scores from the ORES API.
- [README.md](README.md): A detailed description of the project, including the data sources, methodologies used, and key findings from the analysis. The file also contains reflections on potential biases in the dataset and the limitations of Wikipedia as a data source.
- [LICENSE](LICENSE): The license file specifying the terms under which this project can be used, modified, and distributed. 

## Libraries Used
The following Python libraries were used for this project:

- `json`: For handling and saving data in JSON format.
- `pandas`: For processing and analyzing the data.
- `tqdm`: To display progress bars when iterating through API requests.
- `os`: For handling file operations.
- `time`: To control API request timing to avoid exceeding the rate limit.
- `requests`: To make API calls to the Wikimedia Pageviews API.

## How to Reproduce the Analysis

To reproduce the analysis, follow these steps:
1. Clone the repository:
```bash
git clone <repository-url>
cd <repository-directory>
```

2. Install the required Python packages:
Install using pip:
```bash
pip install json tqdm pandas requests
```

3. Data Collection: 
Partial data had been already provided for the assignment (`population_by_country.csv`,`politicians_by_country.csv`). The remaining data is collected through the ORES data of Wikimedia API. You can run the provided code in `homework.ipynb` to fetch data.

4. Run the Jupyter Notebook:
Open and run `homework.ipynb`  in a Jupyter environment to execute the analysis and generate the visualizations.

## Analysis
The analysis includes the tables present within the Python notebook under Step 4 and Step 5:

## License Information

The code for this project is licensed under the MIT License (see the LICENSE file). The data obtained from the Wikimedia Pageviews API is licensed under CC-BY as per Wikimedia Foundation's Terms of Use.

