## Text-based Industry Reclassification and Industry Momentum

**Project Overview:** This is my graduate thesis at _Renmin University of China_, completed during the spring of 2019. I built a new industry classification for China's listed companies using product & services descriptions disclosed in annual/semi-annual reports. This classification performed well as compared to conventional classifications. Consistent with a behavioral finance perspective, the new industry momentum factor, calculated as the average return of industy peers, was proved to have much stronger predictive power of future stock returns than conventional industry momentum factor.

### 1. Motivation
Conventional industry classifications typically categorize a company into one industry. However, in China a large proportion of listed companies have their business spanned across several industries or sectors. Investors who rely on conventional classifications thus tend to ignore industry-level information relevant to a focal company's secondary business, resulting in delayed incorporation of such information into stock prices — a phenomenon called **limited attention** in behavioral finance literature. Ideally, capturing the information that investors overlook would help better predict future stock returns.

### 2. Methodology: Text-based Industry Classification

My idea of using text data for industry reclassification originated from my internship at NLP tech startup. However, when I did my graduate thesis, Hoberg and Phillips (2016) already proposed a solution that had been widely accepted in academia. Hence, the following steps are based on their paper, with some steps adapted to Chinese language.

Starting from the 2015 annual report (and the 2017 semi-annual report), the China Securities Regulatory Commission(henceforth CSRC) requires listed companies to disclose their business profiles. This section contains a large number of vocabulary related to the company's products and services. If the product & services descriptions of two companies are similar, they probably belong to the same industries. Based on this observation, my study uses text information to redo the industry classification. The specific steps are as follows:

- Webscraping: annual and semi-annual reports are automatically collected from [elangshen](http://www.elangshen.com/)  
- Text cleaning:
  - Extract product & services descriptions using regular expressions; remove tables for they are mostly either irrelavant or boilerplates
  - Tokenize the text and keep nouns
  - Remove high frequency words and rare words, as they introduce noise
  - Remove reports that have few unique words
- Map text into vectors following the Bag-of-Words model: Denote the normalized vector of company $i$ at period $t$ as $v_{i,t}$  
- Cacluate similarity scores between any two comany-period observations
  - Calculate text vector distances using cosine similarity: Denote the cosine similarity between comany $i$ and company $j$ at period $t$ as $CosineSimilarity(i, j)_t = v_{i,j} * v_{j,t}$
  - Calculate similarity scores: $Similarity(i,j)_t = Cosinesimilarity(i, j)_t - Median(\{k \in Companies_t: CosineSimilarity(i, k)_t})$, where $Companies_t$ is the set of companies at period $t$.
- Define **industry peers**:
  - For a focal company $i$, its industry peer at period $t$ is defined as: $IndustryPeer_{i, t} = \{j \in Companies_t: Min(Similarity(i, j)_t, Similarity(j, i)_t) > Threshold_t\}$
  - Use Shenwan Industry Classification (henceforth SWS Ind) to determine the value of $Threshold_t$: set $Threshold_t$ in a way that at period $t$, the number of industry pairs defined by text is approximate to that defined by SWS Ind

The definition of industry peers here makes it more like a network than a classification. In fact, the classification proposed by Hoberg and Phillips (2016) was named Text-based Network Industries (TNIC for short).

SWS Ind is the most popular industry classification among finance practioners in China. This study thus uses SWS Ind as the major benchmark. CSRC has also published industry classification (denoted as CSRC Ind for short), which is the default choice in academic research. But as you will see later, CSRC Ind performs worse than SWS Ind.

When working on this thesis, the annual report for fiscal year 2018 was not completely availble. Consequently, I gathered five periods of reports that had product & services information disclosed. Industry reclassification, namely the process described above on how to define industry peers, was conducted on Apr 30 (Aug 31) when most annual(semi-annual) reports had been made public. To further avoid look-ahead bias, the text for companies that failed to issue reports before the expected date was not updated.

**Table 1** number of companies with at least one industry peer:

| 2015A | 2016A | 2017H | 2017A | 2018H |
|:-----:|------:|-------|-------|-------|
|  2687 |  3035 | 3248  | 3431  | 3501  |

As shown in Table 1, text-based industry covers more than 90% of the entire A-share universe.

**Table 2** descriptive statistics: number of industry peers for a focal company

|      | 2015A | 2016A | 2017H | 2017A | 2018H |
|------|-------|-------|-------|-------|-------|
| mean | 51.95 | 57.62 | 61.61 | 65.91 | 67.03 |
| std  | 36.97 | 43.72 | 43.89 | 52.96 | 52.41 |
| min  | 1     | 1     | 1     | 1     | 1     |
| 25%  | 23    | 23    | 27    | 23    | 26    |
| 50%  | 43    | 46    | 51    | 51    | 53    |
| 75%  | 72    | 81    | 86    | 95    | 94    |
| max  | 215   | 225   | 264   | 284   | 292   |

Table 2 shows the distribution of the number of industry peers a focal company has under the text-based industry classification. For comparison, Table 3 lists the same statistics when using SWS Ind, for the same universe of stocks. Obviously, the order statistics are of comparable scale, which results directly from using SWS Ind as a benchmark.

**Table 3** descriptive statistics: number of industry peers for a focal company defined by SWS Ind

|      | 2015A | 2016A | 2017H | 2017A | 2018H |
|------|-------|-------|-------|-------|-------|
| mean | 54.54 | 60.90 | 62.91 | 67.03 | 67.43 |
| std  | 43.02 | 47.41 | 47.93 | 51.40 | 51.77 |
| min  | 0     | 0     | 0     | 0     | 0     |
| 25%  | 22    | 27    | 29    | 29    | 31    |
| 50%  | 41    | 44    | 47    | 49    | 50    |
| 75%  | 69    | 81    | 87    | 96    | 100   |
| max  | 159   | 177   | 177   | 193   | 194   |


### 3. Classification Performance
#### 3.1 The SWS Inds of Industry Peers

The table below shows two randomly selected focal companies and the SWS Inds corresponding to their industry peers. In both cases, the majority of peer firms have their SWS Inds highly consistent with the focal company. For example, among the 178 peers of Renfu Medicine, which is a pharmaceutical manufacturer, 98 are from the traditional Chinese medicine industry, the pharmaceutical production industry and the biological products industry. It is of course not by coincidence. Comapnies with closely related Shenwan Industries tend to use similar vocabulary when describing their businesses, which is captured by my text-based industry classification.

**Table 4** 

| Stock Name and Ticker  | SWS Ind         | Frequencies of SWS Inds among Industry Peers |
|--------------------------|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Phoenix Media 601928.SH  | cultural media           | cultural media [36], computer applications [17], internet media [10], communication equipment [6], marketing communications [4], general retail [3], computer equipment [3], chemicals [2], professional retail [2], commercial property management [2], real estate development [1], optical optoelectronics [1], hotel [1], clothing home textiles [1], textile manufacturing [1], household light industry [1], papermaking [1], packaging and printing [1], audio-visual equipment [1], food processing [1], comprehensive [1]                                                                                                                     |
| Renfu Medicine 600079.SH | pharmaceutical production | traditional Chinese medicine [44], pharmaceutical production [40], biological products [14], securities [13], banks [9], pharmaceutical business [8], medical services [6], chemicals [5], medical devices [5] , comprehensive [5], animal health [3], special equipment [3], clothing home textile [2], infrastructure [2], food processing [2], chemical fiber [2], real estate development [2], cultural media [1], semiconductor [1], automotive parts [1], park development [1], cement manufacturing [1], water [1], high and low voltage equipment [1], trade [1], motor [1], plastic [1], coal mining [1], metal products [1], internet media [1] |

#### 3.2 High Frequency Words in Product & Services Descriptions of Industry Peers

Below is another interesting table showing two randomly selected focal companies and the top 30 high-frequency words in the product & services descriptions of their industry peers. To some extent, we can guess the industry of the focal company simply by reading the most frequent words used by its industry peers. For example, in the case of Northeast Securities almost all words are directly related to finance. This finding further demonstrates the feasibility of text-based industry classification.

**Table5**

| Stock Name and Ticker          | SWS Ind    | Top 30 High-frequency Words in the Product & Services Descriptions of Industry Peers|
|--------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Northeast Securities 000686.SZ | securities | banks [84], securities [82], financing [81], funds [79], transactions [71], supervision [69], bonds [66], income [65], finance [64], credit [63], Consultation [58], wealth [58], transformation [56], private placement [55], entity [54], compliance [54], RMB [54], feature [53], stock [53], consultant [51] brokerage [51], futures [51], regulation [50], individual [49], asset management [48], underwriting [48], structure [46], insurance [46], equity [45], investors [45]             |
| Hainan Mining 601969.SH        | mining     | reserves [30], minerals [29], mining [29], mines [28], metals [27], mining [26], mines [26], smelting [24], grades [23], yields [23], Mining area [23], exchange [23], ore [22], mining [22], raw materials [21], exploration [21], copper [20], use [20], quantity [20], geology [20] , mineral processing [20], trade [19], solid [19], non-ferrous metals [18], exploration [18], securities [17], utilization [17], production capacity [17], production capacity [16], chemical industry [16] |

_Note: duplicated words are due to translation_

#### 3.3 Identify the Most Related SWS Ind Pairs

Here comes my favorite part. Although we can somehow know the degree of association between any two SWS Inds simply according to their names, from now on we pretend to have no prior knowledge about these industries at all. Instead, by assuming that text-based industry classifcation is a good classification, I demonstrate a way to pick out the most related SWS Ind pairs.

As we see in **3.1**, industry peers of a focal company come from different SWS Inds. Intuitively, the more related two SWS Inds are, the more likely that they appear simultaneously. There are at least two supporting arguments. First, as mentioned, companies from related Shenwan Industries tend to share similar vocabulary in their reports, and are thus more likely to appear as the peers of the same focal company. Second, for a focal company with diversified businesses, concentric diversification is more likely to happen, in which the company develops products or services closely related with its current core business. If text-based classifcation captures the diversification of the focal company, then different but highly related SWS Inds are more likely to coappear. Based on this observation, I implemented the following steps to quantify the degree of associations between two SWS Ind pairs.

- Caculate the Coappear Score: for a given SWS Ind pair $a$ and $b$, the Coappear Score is equal to the sum of the multiplications of frequencies, iterated over all focal companies' industry peers. $Coappear_{a, b} = \sum_{Peers_i} Freq_{i, a} \times Freq_{i,b}$, where $Peers_i$ represents the industry peers for focal company $i$, and $Freq_{i, a}$ represents the frequency of companies belonging to SWS Ind $a$ that are meanwhile industry peers of company $i$.
- Use Monte Carlo Simulation to calculate the part of Coappear Score attributed to SWS Ind distribution: a high Coappear Score does not necessarily means that two SWS Inds are highly correlated. Chances are that they frequently coappear because they both contain a large number of companies. If assigning industry peers randomly, then the Pseudo Coappear Score should only reflect the total number of companies in each industry, and is thus able to explain the part of Coappear Score purely attributed to SWS Ind distribution. I thus implemented a Monte Carlo Simulation of 100 rounds, where at each round companies are randomly assigned to be other focal companies' industry peers, keeping for each company the nunmber of industry peers as well as the times of being industry peers fixed. The Pseudo Coappear Score for SWS Inds $a$ and $b$ is calculated as $PseudoCoappear_{a, b} = 0.01 \times \sum_{trial_0} ^{trial_{100}} TrialCoappear_{a, b, i}$, where $TrialCoappear_{a, b, i}$ is the Coappear Score between $a$ and $b$ in $trial_i$.
- Calculate the Normalized Coappear Score as the final score: $NormalizedCoappear_{a,b} = \dfrac{Coappear_{a,b}}{PseudoCoappear_{a, b}}$ 

There are 5,356 potential SWS Ind pairs. Listed below are the top 50 SWS Ind pairs ranked by Normalized Coappear Score. The result is pretty impressive: we identify highly related industry pairs without using prior knowledge. The result further lends credence to text-based industry classification.

**Table 6** Top 50 Most Related SWS Ind Pairs

|                                                      |                                                                |                                               |                                                       |
|------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------|-------------------------------------------------------|
| 1. airport & air freight                             | 2. port & rail transport                                       | 3. port & shipping                            | 4. food & beverage manufacturing                      |
| 5. livestock and poultry farming & feed              | 6. tourism & attractions                                       | 7. oil exploitation & Mining service          | 8. shipping & rail transport                          |
| 9. airport & port                                    | 10. insurance & securities                                     | 11. rare metal & gold                         | 12. fishery & livestock and poultry farming           |
| 13. animal health & livestock and poultry farming    | 14. airport & shipping                                         | 15. ground equipment & aerospace equipment    | 16. securities & bank                                 |
| 17. other mining & gold                              | 18. livestock and poultry farming & crop farming               | 19. diversified finance & securities          | 20. insurance & bank                                  |
| 21. bus & airport                                    | 22. water & environmental engineering and   services           | 23. attractions & hotel                       | 24. animal health & feed                              |
| 25. other mining & rare metal                        | 26. agricultural integration & crop farming                    | 27. insurance & diversified finance           | 28. diversified finance & bank                        |
| 29. airport & rail transport                         | 30. traditional Chinese medicine & chemical and pharmaceutical | 31. infrastructure & housing construction     | 32. industrial metal & gold                           |
| 33. chemical and pharmaceutical & biological product | 34. bus & rail transport                                       | 35. tourism & hotel                           | 36. traditional Chinese medicine & biological product |
| 37. bus & air freight                                | 38. ground equipment & aviation equipment                      | 39. fishery & feed                            | 40. industrial metal & rare metal                     |
| 41. coal mining & rail transport                     | 42. park development & real estate development                 | 43. garden engineering & water                | 44. general retail & clothing                         |
| 45. fishery & crop farming                           | 46. aerospace equipment & aviation equipment                   | 47. Internet media & marketing communications |                                                       |
| 48. animal health & fishery                          | 49. general retail & commercial property management            | 50. medical service & biological product      |                                                       |

#### 3.4 Explanatory Power of Industry-level Variables

Companies in the same industry should also exhibit similar patterns in a wide variety of variables, including valuation multiples (PE ratio, book to market ratio), profitability (return on asset, return on equity), operating efficiency (asset turnover ratio), growth potential (annual sales growth), debt capacity (debt ratio), R&D expense ratio and analyst forecasted growth. If a classification is indeed well-defined, then an industry-level variable aggregated from the composite companies should have great explanatory power on company-level variables, hence a relatively high regression R-squared.

I used the following company-wise regression to compare CSRC Ind, SWS Ind and text-based industry classification. $variable$ can be any of the variables mentioned above, and $variable_{ind_{i,t}}$ is the corresponding industry average excluding company $i$:
$variable_{i,t} = \alpha + \beta \times variable_{ind_{i,t}}+ \gamma \times time\_dummies + \epsilon_{i,t}$

Table 7 summarizes all the adjusted R-squared obtained from the regressions. SWS Ind and TNIC both win 4 battles, and tie in one. CSRC Ind fails to win even one battle. Notably, although the SWS Ind is the most popular one to financial analysts, it falls behind text-based industry classification in the test where analyst forecasted growth is the variable of interest. Such a result seems to indicate that analysts will identify comparable companies based on their own understanding, rather than fully relying on established industry classifications.

**Table 7** Regress Company-level Variable on Industry-level Variable: R-squared results

| Variables       | CSRC Ind | SWS Ind | TNIC   | # Samples |
|-----------------|----------|---------|--------|-----------|
| _EP_              | 18.09%   | **18.25%**  | 11.17% | 9139      |
| _BM_              | 26.55%   | **27.80%**  | **27.80%** | 9139      |
| _ROA_             | 8.71%    | 8.38%   | **12.38%** | 9139      |
| _ROE_             | 6.53%    | 6.38%   | **7.42%**  | 9113      |
| _TURNOVER_        | 27.21%   | **29.83%**  | 27.93% | 9139      |
| _GROWTH_          | 5.68%    | **6.00%**   | 3.63%  | 9139      |
| _DEBT_            | 22.72%   | 24.20%  | **27.61%** | 9139      |
| _RD EXPENSE_      | 41.98%   | **44.20%**  | 43.08% | 9139      |
| _FORECAST GROWTH_ | 5.68%    | 7.00%   | **8.61%**  | 6314      |

_Note: 1. EP takes the reciprocal of PE ratio to account for negative values; 2. Bold format indicates the best among three_


### 4. Text-based Industry Momentum

I have demonstrated above that text-based industry classification is reliable. Obviously, it can capture some of the industry characteristics which is not directly reflected in a conventional classification scheme. To illustrate, a real estate company with food manufacturing taking up 25% percent of its yearly revenue is categorized to real estate industry, but its text-based industry peers will include many food manufactuers. Investors, subjected to limited attention from a behavioral finance perspective, tend to ignore this fact.

The momentum effect is an usual market phenomenon by which asset prices follow an upward or downward trend for a long time. As a result, the past return of the asset, usually named momentum factor, can predict future return. Likewise, industry momentum is defined as the industry-level past return. Formally, we can define the momentum of company $i$ during period $t$ as the average stock return of its industry peers during period $t$. $IndReturn_{i, t} = \dfrac{1}{N}\(sum_{m} return_{m, t}\)$ where $m \in Peer_{i, t}$. Here I extend the concept of $Peer_{i, t}$ to conventional classifications so that two comapnies belonging to the same industry are automatically industry peers.

As industry momentum can proxy for industry-level information, and the text-based industry classification defines industry associations that investors overlook, the text-based industry momentum will contain relevant industry-level information that slowly affects stock prices, resulting in a stronger momentum effect. This section presents the investigation.

#### 4.1 Fama-Macbeth Cross-sectional Regression


$$ return_{i, t} = \alpha + \beta_1 \times IndReturn_{i, t-1} + \beta_2 \times return_{i, t-1} +  
<br>
\beta_3 \times log(MV_{i, t-1}) + \beta_4 \times log(BM_{i, t-1}) + \epsilon_{i, t}$$


#### 4.2 Portfolio Backtest


