---
layout: post
title:  The real-time fraud detection
description: |
  The real-time fraud detection is about understand consumer transaction, deduct relevant knowledge and accumulate consumer profile as a set of feature vectors.
tags:
  - thoughts
  - analytics
  - FinTech
  - fraud
  - datalog
  - open search
  - open source
---

# The real-time fraud detection

Recently, I had a luxury to brainstorm my [real-time consumer engagement](/2015/03/17/real-time-consumer-engagement.html) with FinTech expert. We talked about methods of fraud detections and applicability of e-commerce techniques. Anyone can apply various statistical and machine learning approaches to study consumer behavior. One of the fundamental differences in a real-time data analysis is reaction on each individual transaction, which accumulates in time and impacts on the decision about consumer behavior.

We can rephrase that consumer behavior analysis in FinTech is all about knowing consumers transactions, getting to known consumer peers and emerging trends from both consumer, banking and other service providers.

A high resolution, real-time behavioral analysis is an opportunity to enhance traditional ETL analytics portfolio, which opens a new opportunities for Financial Intelligence and Crime Analysis.

The successful implementation of real-time behavior analysis depends on the ability to understand consumer transaction, deduct relevant knowledge and accumulate consumer profile as a set of feature vectors. These vectors formalises consumer behavior in the format suitable and understandable by machines. For example, consumer transactions are collection of immutable facts that depicts financial flows and relations between peers. Let’s consider a simple scenario


| Account | Statement | Amount | Destination |
|---|---|---|---|---|
| 8345 | Debit | 4342 | US |
| 3138 | Debit | 5852 | UK |
| ... | ...  | ... | ... |
| 8345 | Credit | 2389 | FI |


Each account is a node that has a finite set of vertices. Each vertex defines a flow of transactions between peers. Anyone can formalize these flows using statistical methods e.g. express it using appropriate statistical distribution. One remark here, TeleCom domain often uses Poisson distribution to abstract flows. Once appropriate distribution(s) is fixed for FinTech then definition for consumer behavior becomes routine process - collection of these distribution forms the genome. A similar consumer behavior reflects to similar genomes which is then later assessed using pattern matching techniques.


{: .lh-0 }
```
 +-----------> A +---------+
 +                         v

US      +----------------+ FI <-------+ C
        |
 ^      v                               ^
 |                                      |
 +----+ B +--------------> UK +---------+

```

The genome discovery is a continuous process. It accumulates knowledge over the time. Initially, we do not know anything about the behavior of consumers. Every transaction or behavioral signal gives us a clue for each link that is accumulated into the feature vector.


We can apply the real-time analysis technique to example transaction dataset. It requires provision on the architecture solution discussed in [previous post](/2015/03/17/real-time-consumer-engagement.html).

The proof of the concept draft the high level architecture solution discussed above. We are going to focus on three elements: part that builds feature vectors from transaction log, semantical storage and data access interface Datalog to deduct fraud. The proof of the concept is based on prior art open source project: [datalog](https://github.com/fogfish/datalog), [elasticlog](https://github.com/fogfish/elastic) and [hercule](https://github.com/fogfish/hercule).


## Genome schema

We are using naive approach to model distribution of financial traffic, in real life the feature vector consists of hundreds parameters. It calculates expectation (E), standard deviation (SD), total traffic volume and number of transactions (N). A table below shows the example of genome.

| Link | E | SD | Volume | N |
|---|---|---|---|---|---|
| A → FI | 6974 | 836 | 13457 | 5 |
| A ← US | 7542 | 928 | 23173 | 3 |
| B → US | 4231 | 652 | 17263 | 6 |
| B → UK | 8272 | 435 | 13232 | 3 |
| B ← FI | 1231 | 321 | 18625 | 2 |


The solution uses ElasticSearch for this solution (eventually we can use any BigData storage). However, Usage of Elastic has few benefits: we can scale it to store 1 bln genomes, it provides efficient real-time data analytics and there are open source tooling to simplify the data mining via datalog. We model the genome via set of documents, where each document depicts credit/debit flows between account and its counterpart in some countries. An example feature vector looks like

```javascript
{
  "account": "1007",
  "country": "SE",
  "id": "SE:1007",
  "credit": 
  {
    "e": 5071,
    "n": 3,
    "sd": 352.09799772222505,
    "vol": 15213
  },
  "debit": 
  {
    "sd": 240.41630560342617,
    "vol": 10702,
    "e": 5351,
    "n": 2
  }
}
```

The construction of genome is an elementary ETL activity, which is based on pure computations. The computation transforms logs of transactions to collection of feature vectors with consequent storage to Elastic.

## Scam analysis

We are using pattern matching technique to discover contradiction behavior of customers, once the real-time data collection is established. As part of these techniques, we have formulated a pattern (feature vectors) that corresponds to scam activities, e.g.
1. Traffic expectation is much higher than typical expectation in the group
2. High volume of the traffic 
3. High number of transactions

### Hypothesis No.1


Analysis of traffic expectation per transaction helps us to understand if average behavior of groups differs each other. Looking CDF (99th percentile) of expected behavior, we able to detect regions where some accounts has higher credit/debit volume than accounts in other areas. 

```
?- amount/3.

features:country(category 5 "key" "country").
features:debit("country", percentiles "99.0" "debit.e").
features:credit("country", percentiles "99.0" "credit.e").

amount(country, debit, credit) :-
   features:country(country),
   features:debit(country, debit),
   features:debit(country, credit).
```

Using pattern match technique we can discover accounts with expectations higher than the 99th percentile.

```
?- scam/4.

features:debit("country", percentiles "99.0" "debit.e").
features:account("country", "account", "debit.e", "debit.vol", "debit.n").

scam(account, debit, volume, transactions) :-
   features:debit(country, pth),
   features:account(country, account, debit, volume, transactions),
   country = "...",
   debit > pth.
```

```
?- scam/4.

features:credit("country", percentiles "99.0" "credit.e").
features:account("country", "account", "credit.e", "credit.vol", "credit.n").

scam(account, credit, volume, transactions) :-
   features:credit(country, pth),
   features:account(country, account, credit, volume, transactions),
   country = "...",
   debit > pth.
```

### Hypothesis No.2 and No.3

Analysis of the volume distribution per account helps us to identity outlines, who transfers large qualities. We run analysis independently for credit and debit. 

```
?- volume/2.

features:country(category 5 "key" "country").
features:debit("country", stats "debit.vol").

volume(country, debit) :-
   features:country(country),
   features:debit(country, debit).
```

```
?- volume/2.

features:country(category 5 "key" "country").
features:credit("country", stats "credit.vol").

volume(country, credit) :-
   features:country(country),
   features:credit(country, credit).
```

The outcome of the analysis give a clear hint to investigate accounts that transferred higher volume than other.

```
?- scam/5.

features:credit("country", "account", "credit.e", "credit.vol", "credit.n").

scam(country, account, e, vol, n) :-
   features:credit(country, account, e, vol, n), vol > ... .
```

```
?- scam/5.

features:debit("country", "account", "debit.e", "debit.vol", "debit.n").

scam(country, account, e, vol, n) :-
   features:debit(country, account, e, vol, n), vol > ... .
```

## Conclusion

The discussed technique is nothing more than traditional Event Sourcing patterns with consequent data enrichment and deduction of consumer behavioral. The solution can be implemented with what-so-ever storage techniques. The usage to graphs helps to model complex behavioral pattern and graphs databases helps efficiently pattern match your insides. Usage of datalog is artificial in this particular example, it was only done for my educational purpose.



