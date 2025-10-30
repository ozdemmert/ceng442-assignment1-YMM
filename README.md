# CENG 442 – Azerbaijani Text Preprocessing + Word Embeddings

## Group Members
- Mert Özdemir
- Melike Çobankaya
- Yahya Çakıcı

## 1. Data & Goal

*Datasets:* Five Azerbaijani sentiment datasets with varying label schemes.

*Goal:* Create reproducible preprocessing pipeline and unify sentiment labels (Negative=0.0, Neutral=0.5, Positive=1.0). Keeping neutral as 0.5 preserves three-class polarity for nuanced analysis.

## 2. Preprocessing

Implemented Azerbaijani-aware pipeline with: emoji mapping, HTML cleaning, token replacement, hashtag splitting, mention handling, Azerbaijani lowercasing, punctuation normalization, number replacement, whitespace cleaning, and slang correction.

*Data Cleaning Stats:*
- Original: 122,431 rows
- After removing empties/duplicates: 62,964 rows (48.6% reduction)

*Examples:*
- Original: Salam! Bu, bir test mesajıdır. Qiymət 50 azn-dir. #YaxsiGunler 😊
- Cleaned: salam bu bir test mesajıdır qiymət <NUM> azn dir yaxşı gunler emo pos

## 3. Mini Challenges

- *Hashtag Split:* CamelCase detection and splitting
- *Emoji Mapping:* Dictionary-based conversion to tokens
- *Negation Scope:* 3 tokens after negators get _NEG suffix
- *Deasciify:* SLANG_MAP for common misspellings (e.g., cox→çox)
- *Stopwords:* Identified but not removed for sentiment context

## 4. Domain-Aware Additions

*Domain Detection:* Regex-based classification into:
- news (media mentions)
- social (@, #, RT, emojis) 
- reviews (price, rating terms)
- general (default)

*Review-Specific:* Prices→<PRICE>, stars→<STARS_X>, phrases→sentiment tokens

*Corpus Tagging:* Each sentence prepended with domain tag in final corpus.

## 5. Embeddings

*Training Settings:*
| Parameter | Value |
|-----------|-------|
| vector_size | 300 |
| window | 5 |
| min_count | 3 |
| sg | 1 |
| epochs | 10 |

*Results:*
- *Coverage:* 0.932-0.990 across datasets
- *Similarity:* W2V Separation=0.020, FT Separation=0.010
- *Neighbors:* W2V showed more coherent grouping of variants

## 6. Reproducibility

*Requirements:* Python 3.10+, pandas, gensim, openpyxl, regex

*Note:* You should download the **embeddings** folder in order to use pretrained models and add it to the folder where the code is located. **embeddings** link:

*Run:*
bash
pip install -r requirements.txt

Place Excel files in Excels/

Run main.ipynb

## 7. Conclusions

### Which model worked better?
Better Model: Word2Vec performed slightly better:

- Positive separation score vs FastText's negative

- More coherent morphological variant grouping

- Better for user-generated Azerbaijani text

### Next Steps 

- Azerbaijani lemmatizer integration

- Hyperparameter tuning

- Larger corpus pre-training