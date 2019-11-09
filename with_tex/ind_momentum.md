## Text-based Industry Reclassification and Industry Momentum

**Project Overview:** This is my graduate thesis at _Renmin University of China_, completed during the spring of 2019. I built a new industry classification for China's listed companies using product & services descriptions disclosed in annual/semi-annual reports. This classification performed well as compared to conventional classifications. Consistent with a behavioral finance perspective, the new industry momentum factor, calculated as the average return of industy peers, was proved to have much stronger predictive power of future stock returns than conventional industry momentum factor.

### 1. Motivation
Conventional industry classifications typically categorize a company into one industry. However, in China a large proportion of listed companies have their business spanned across several industries or sectors. Investors who rely on conventional classifications thus tend to ignore industry-level information relevant to a target company's secondary business, resulting in delayed incorporation of such information into stock prices — a phenomenon called **limited attention** in behavioral finance literature. Ideally, capturing the information that investors overlook would help better predict future stock returns.

### 2. Methodology: Text-based Industry Classification

Starting from the 2015 annual report (and the 2017 semi-annual report), the China Securities Regulatory Commission(henceforth CSRC) requires listed companies to disclose their business profiles. This section contains a large number of vocabulary related to the company's products and services. If the product & services descriptions of two companies are similar, they probably belong to the same industries. Based on this observation, my study uses text information to redo the industry classification. The specific steps are as follows:

- Webscraping: annual and semi-annual reports are automatically collected from [elangshen](http://www.elangshen.com/)  
- Text cleaning:
  - Extract product & services descriptions using regular expressions; rid off tables which are mostly irrelavant or boilerplates
  - Tokenize the text and keep nouns
  - Remove high frequency words and rare words, as they introduce noise
  - Remove reports that have few unique words
- Map text into vectors following the Bag-of-Words model: Denote the normalized vector of company <img src="/with_tex/tex/77a3b857d53fb44e33b53e4c8b68351a.svg?invert_in_darkmode&sanitize=true" align=middle width=5.663225699999989pt height=21.68300969999999pt/> at period <img src="/with_tex/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/> as <img src="/with_tex/tex/e4760d1dbb5d5c4786113ab1c3cea441.svg?invert_in_darkmode&sanitize=true" align=middle width=21.488890499999993pt height=14.15524440000002pt/>  
- Cacluate similarity scores between any two comany-period observations
  - Calculate text vector distances using cosine similarity: Denote the cosine similarity between comany <img src="/with_tex/tex/77a3b857d53fb44e33b53e4c8b68351a.svg?invert_in_darkmode&sanitize=true" align=middle width=5.663225699999989pt height=21.68300969999999pt/> and company <img src="/with_tex/tex/36b5afebdba34564d884d347484ac0c7.svg?invert_in_darkmode&sanitize=true" align=middle width=7.710416999999989pt height=21.68300969999999pt/> at period <img src="/with_tex/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/> as <img src="/with_tex/tex/db70e305afe798d9b40621d1e3b7b295.svg?invert_in_darkmode&sanitize=true" align=middle width=252.98774654999994pt height=24.65753399999998pt/>
  - Calculate similarity scores: <img src="/with_tex/tex/f463c5fbcacc564ba508b81e72fa8480.svg?invert_in_darkmode&sanitize=true" align=middle width=687.8497626pt height=24.65753399999998pt/>, where <img src="/with_tex/tex/e511736946f4f78b39e5df3aa9f18f68.svg?invert_in_darkmode&sanitize=true" align=middle width=88.14102164999998pt height=22.465723500000017pt/> is the set of companies at period <img src="/with_tex/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/>.
- Define **industry peers**:
  - For a target company <img src="/with_tex/tex/77a3b857d53fb44e33b53e4c8b68351a.svg?invert_in_darkmode&sanitize=true" align=middle width=5.663225699999989pt height=21.68300969999999pt/>, its industry peer at period <img src="/with_tex/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/> is defined as: <img src="/with_tex/tex/b622f4fba44d5d916cb52696d74a7180.svg?invert_in_darkmode&sanitize=true" align=middle width=667.3170801pt height=24.65753399999998pt/>
  - Use Shenwan Industry Classification (henceforth SWS Ind) to determine the value of <img src="/with_tex/tex/76bc21ee6c4fa0dd1b406deb46edd637.svg?invert_in_darkmode&sanitize=true" align=middle width=80.78226419999999pt height=22.831056599999986pt/>: set <img src="/with_tex/tex/76bc21ee6c4fa0dd1b406deb46edd637.svg?invert_in_darkmode&sanitize=true" align=middle width=80.78226419999999pt height=22.831056599999986pt/> in a way that at period <img src="/with_tex/tex/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.936097749999991pt height=20.221802699999984pt/>, the number of industry pairs defined by text is approximate to that defined by SWS Ind
  
  


### Analysis:
