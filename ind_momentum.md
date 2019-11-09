## Text-based Industry Reclassification and Industry Momentum

**Project Overview:** This is my graduate thesis at _Renmin University of China_, completed during the spring of 2019. I scraped annual and semi-annual reports of China's listed companies from websites, extracted the Product & Description chapter from Management Discussion & Analysis, from which I further built a new industry classification. This classification performed well as compared to conventional classifications. Consistent with a behavioral finance perspective, the new industry momentum factor, calculated as the average return of industy peers, was proved to have much stronger predictive power of future stock returns than conventional industry momentum factor.

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
- Map text into vectors following the Bag-of-Words model: Denote the vector of company <img src="/tex/77a3b857d53fb44e33b53e4c8b68351a.svg?invert_in_darkmode&sanitize=true" align=middle width=5.663225699999989pt height=21.68300969999999pt/> at period <img src="/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/> as <img src="/tex/e4760d1dbb5d5c4786113ab1c3cea441.svg?invert_in_darkmode&sanitize=true" align=middle width=21.488890499999993pt height=14.15524440000002pt/>  
- <img src="/tex/010f345562008e85da4abd0ab364c0a0.svg?invert_in_darkmode&sanitize=true" align=middle width=258.56990444999997pt height=24.65753399999998pt/>
<img src="/tex/b832ded8243f420a0af52ca8abbc67dd.svg?invert_in_darkmode&sanitize=true" align=middle width=729.5360985pt height=24.65753399999998pt/> 
