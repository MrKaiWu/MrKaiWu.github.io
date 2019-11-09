## Text-based Industry Reclassification and Industry Momentum

**Project Overview:** This is my graduate thesis at _Renmin University of China_, completed during the spring of 2019. I built a new industry classification for China's listed companies using product & services descriptions disclosed in annual/semi-annual reports. This classification performed well as compared to conventional classifications. Consistent with a behavioral finance perspective, the new industry momentum factor, calculated as the average return of industy peers, was proved to have much stronger predictive power of future stock returns than conventional industry momentum factor.

### Motivation
Conventional industry classifications typically categorize a company into one industry. However, in China a large proportion of listed companies have their business spanned across several industries or sectors. Investors who rely on conventional classifications thus tend to ignore industry-level information relevant to a target company's secondary business, resulting in delayed incorporation of such information into stock prices — a phenomenon called **limited attention** in behavioral finance literature. Ideally, capturing the information that investors overlook would help better predict future stock returns.

### Methodology: Text-based Industry Classification

Starting from the 2015 annual report (and the 2017 semi-annual report), the China Securities Regulatory Commission(henceforth CSRC) requires listed companies to disclose their business profiles. This section contains a large number of vocabulary related to the company's products and services. If the product & services descriptions of two companies are similar, they probably belong to the same industries. Based on this observation, my study uses text information to redo the industry classification. The specific steps are as follows:

- Webscraping: annual and semi-annual reports are automatically collected from elangshen(http://www.elangshen.com/)  
- Text cleaning:
  - Extract product & services descriptions using regular expressions; rid off tables which are mostly irrelavant or boilerplates
  - Tokenize the text and keep nouns
  - Remove high frequency words and rare words, as they introduce noise
  - Remove reports that have few unique words
- Map text into vectors following the Bag-of-Words model: Denote the normalized vector of company $f$ at period $t$ as $v_{f,t}$  
- Cacluate similarity scores between any two comany-period observations
  - Calculate text vector distances using cosine similarity: Denote the cosine similarity between comany $f$ and company $j$ at period $t$ as $CosineSimilarity(f, j)_t = v_{f,j} * v_{j,t}$
  - Calculate similarity scores: $Similarity(f,j)_t = Cosinesimilarity(f, j)_t - Median(\{CosineSimilarity(f, k)_t, k \in Companies_t\})$ 
- Define industry peers:

### Analysis:

