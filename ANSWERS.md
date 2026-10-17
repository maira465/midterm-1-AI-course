# Midterm 1: answers

GitHub username:maira465
Session code:KETTLE-62
Fingerprint:ae3f1564

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used gemini and chat gpt as AI assistants during this exam. I utilized them to help parse the generated synthetic dataset, verify mathematical formulas for MRR and Recall metrics, calculate token cost economics, and validate my answers for the theoretical questions (Q1–Q5). All final calculations, code executions in Google Colab, and explanations were reviewed, verified, and confirmed by me.
(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.417 | 0.533 | 0.617 | 0.492 |
| B | 0.333 | 0.533 | 0.617 | 0.448 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.533 | 0.409 | 0.654 |
| A, k=5 | 0.617 | 0.490 | 0.729 |
| B, k=3 | 0.533 | 0.409 | 0.654 |
| B, k=5 | 0.617 | 0.490 | 0.729 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 28077 |
| A, k=5 | 36824 |
| B, k=3 | 20398 |
| B, k=5 | 24025 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5
I choose Config B with k=5. With an annual budget of $24,800, Config A is financially unfeasible, as both k=3 ($28,077) and k=5 ($36,824) exceed our limit. 

Config B with k=5 costs $24,025 per year, fitting securely within the budget with a $775 safety margin. Comparing options within budget, Config B with k=5 reaches a higher recall of 0.617 [95% CI: 0.490, 0.729] compared to k=3 options (recall of 0.533 [0.409, 0.654]) while remaining affordable. Config B with k=3 costs $20,398 but offers lower retrieval coverage.

For my chosen candidate (B, k=5), the retrieval metrics deliver the highest recall within budget across all languages.

(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
To improve decision-making, I would evaluate end-to-end answer accuracy (e.g., using ROUGE or LLM-as-a-judge) rather than relying solely on chunk retrieval metrics. Additionally, I would measure end-to-end latency per language and perform cost benchmarking with dynamic chunking to balance token expenditure against context quality under real user queries.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.878 | KK, degraded | 0.800 | 0.627 | 0.905 | 10528 |
| L | 0.806 | KK, degraded | 0.733 | 0.556 | 0.858 | 27000 |

Personal-data pages per year sent abroad under H:361350

### 2b. Memo (at most 300 words)

I recommend deploying Pipeline H while implementing a strict local redaction proxy for sensitive personal data.

Accuracy & Reliability Analysis:
Pipeline H achieves significantly superior overall accuracy compared to Pipeline L (87.8% vs 80.6%). In the hardest edge case—degraded scans in Kazakh—Pipeline H maintains 80.0% accuracy [95% Wilson CI: 0.627, 0.905], outperforming Pipeline L’s lowest point of 73.3% [0.556, 0.858]. High baseline extraction accuracy is crucial for minimizing manual verification overhead across 9,000 daily pages.

Financial Impact:
Pipeline H offers massive cost efficiency, running at approximately $10,528 annually based on image token consumption (1,260 input and 389 output tokens per page). Conversely, Pipeline L requires an infrastructure capacity of 3 dedicated local servers to handle the 9,000 daily page throughput, amounting to $27,000 per year ($2,250/month). Choosing Pipeline H saves over $16,400 annually in operational costs.

Compliance & Privacy Strategy:
Sending unencrypted personal data (individual names and ИИН) abroad violates national data sovereignty regulations. Under Pipeline H, roughly 361,350 pages per year (11% of total traffic) contain personal information. 

To mitigate this without forfeiting Pipeline H's accuracy and cost advantages, we will deploy a lightweight local Regex/OCR anonymization filter on-premises. This pre-processor will detect and mask names and ИИН numbers prior to transmitting images to the hosted API, storing the sensitive keys securely within Kazakhstan. This hybrid design delivers high accuracy and budget savings while maintaining full regulatory compliance.

## Part 3: Explain your own numbers

### Q1

Number:2.12
Why (at most 40 words): Output tokens cost 5× more than input tokens. Kazakh uses more input and output tokens than English due to subword splitting, resulting in a higher total cost ratio of 2.12.

### Q2

Number:598
Why (at most 40 words): $896 \times 0.65 \times (263 / 256) \approx 598$. The e5 model truncates context at 512 tokens. Since 598 > 512, the sentence is cut off completely, preventing retrieval and failing silently.

### Q3

Number:56.2%
Why (at most 40 words): Softmax on logits yields probabilities where the top token holds 56.2%. Since top-p = 0.8 includes additional tokens to reach 80% cumulative probability, renormalizing keeps top token probability unchanged.

### Q4

Number: 30
Why (at most 40 words): Score X = 68(1) - 32(2) = 4. Score Y = 64(1) - 15(2) = 34. Difference Y - X = 30. Unpenalized leaderboards favor aggressive guessing over abstaining.

### Q5

Number:8.30
Why (at most 40 words): Ratio is (11.62T / 70B) / (1.4T / 70B) = 166 / 20 = 8.30. The model is over-trained relative to Chinchilla standards to optimize downstream inference efficiency.
