# Midterm 1: answers

GitHub username:maira465
Session code:KETTLE-62
Fingerprint:ae3f1564

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used gemini and chat got as AI assistants during this exam. I utilized them to help parse the generated synthetic dataset, verify mathematical formulas for MRR and Recall metrics, calculate token cost economics, and validate my answers for the theoretical questions (Q1–Q5). All final calculations, code executions in Google Colab, and explanations were reviewed, verified, and confirmed by me.
(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.400|0.650 |0.733 | 0.542|
| B | 0.467| 0.717| 0.817| 0.601|

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.650 | 0.524 | 0.758 |
| A, k=5 | 0.733 | 0.610 | 0.829 |
| B, k=3 | 0.717 | 0.592 | 0.816 |
| B, k=5 | 0.817 | 0.701 | 0.894 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 24072 |
| A, k=5 | 28491 |
| B, k=3 | 22467 |
| B, k=5 | 25806 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=3

I choose Config B with k=3. Although B with k=5 reaches a higher overall recall@5 (0.817 [0.701, 0.894]), its annual cost of $25,806 exceeds our strict $24,800 budget. 

Config B with k=3 costs $22,467 per year, comfortably fitting within the budget while saving over $2,300 for contingencies. Comparing the k=3 options, Config B clearly outperforms Config A (overall recall@3 of 0.717 [0.592, 0.816] vs 0.650 [0.524, 0.758]) while being $1,605 cheaper due to more compact chunk size.

For my chosen candidate (B, k=3), the breakdown of recall@3 by language is:
- EN: 0.800
- RU: 0.700
- KK: 0.650

This selection maximizes retrieval accuracy and preserves balanced multi-language coverage without overshooting the operational financial limit.

(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
To improve decision-making, I would evaluate end-to-end answer accuracy rather than relying solely on chunk retrieval metrics. Additionally, I would measure end-to-end latency per language and perform cost benchmarking with dynamic chunking to balance token expenditure against context quality under real user queries.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.872 | KK, degraded | 0.800 | 0.627 | 0.909 | 10531 |
| L | 0.800 | KK, degraded | 0.733 | 0.556 | 0.858 | 27000 |

Personal-data pages per year sent abroad under H:361350

### 2b. Memo (at most 300 words)

I recommend deploying Pipeline H while implementing a strict local redaction proxy for sensitive personal data.

Accuracy & Reliability Analysis:
Pipeline H achieves significantly superior overall accuracy compared to Pipeline L (87.2% vs 80.0%). In the hardest edge case—degraded scans in Kazakh—Pipeline H maintains 80.0% accuracy [95% Wilson CI: 0.627, 0.909], outperforming Pipeline L’s lowest point of 73.3% [0.556, 0.858]. High baseline extraction accuracy is crucial for minimizing manual verification overhead across 9,000 daily pages.

Financial Impact:
Pipeline H offers massive cost efficiency, running at approximately $10,531 annually based on image token consumption (1,260 input and 389 output tokens per page). Conversely, Pipeline L requires an infrastructure capacity of 3 dedicated local servers to handle the 9,000 daily page throughput, amounting to $27,000 per year ($2,250/month). Choosing Pipeline H saves over $16,400 annually in operational costs.

Compliance & Privacy Strategy:
Sending unencrypted personal data (individual names and ИИН) abroad violates national data sovereignty regulations. Under Pipeline H, roughly 361,350 pages per year (11% of total traffic) contain personal information. 

To mitigate this without forfeiting Pipeline H's accuracy and cost advantages, we will deploy a lightweight local Regex/OCR anonymization filter on-premises. This pre-processor will detect and mask names and ИИН numbers prior to transmitting images to the hosted API, storing the sensitive keys securely within Kazakhstan. This hybrid design delivers high accuracy and budget savings while maintaining full regulatory compliance.

## Part 3: Explain your own numbers

### Q1

Number:1.93
Why (at most 40 words): Output tokens cost 5× more than input tokens. Although Kazakh requires 2.25× more input tokens due to subword tokenization, English has a higher proportion of output tokens relative to inputs, reducing the bill ratio below the raw token ratio.

### Q2

Number:379
Why (at most 40 words): 896×0.65×(263/256)≈379. The e5 model truncates context at 512 tokens. Since 896×(263/256)≈920>512, the sentence is truncated. Retrieval fails silently with no matching chunk retrieved.

### Q3

Number:60.3%
Why (at most 40 words): Softmax on logits [3.9, 3.0, 2.5, 1.6, 0.3] yields probabilities [0.603, 0.245, 0.149, 0.060, 0.016]. The top token alone holds 60.3% probability, which already satisfies top-p = 0.8 without truncation or probability recalculation.

### Q4

Number:
Why (at most 40 words):

### Q5

Number:8.30
Why (at most 40 words): Ratio is (11.62T / 70B) / (1.4T / 70B) = 166 / 20 = 8.30. The model is over-trained relative to Chinchilla compute-optimal standards. Chinchilla noted models like GPT-3 trade training efficiency for cheaper downstream inference.
