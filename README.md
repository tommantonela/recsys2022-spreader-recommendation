# Repository for the RecSys 2022 paper: *Do recommender systems make social media more susceptible to misinformation spreaders?*

Recommender systems are central to online information consumption and user-decision processes, as they help users find relevant information and establish newsocial relationships. However, recommenders could also (unintendedly) help propagate misinformation and increase the social influence of the spreading it. In this context, we study the impact of friend recommender systems on the social influence of misinformation spreaders on Twitter. To this end, we applied several user recommenders to a COVID-19 misinformation data collection. Then, we explore what-if scenarios to simulate changes in user misinformation spreading behaviour as an effect of the interactions in the recommended network. Our study shows that recommenders can indeed affect how misinformation spreaders interact with other users and influence them.

## Data

Evaluation was based on [FibVid](https://doi.org/10.1016/j.tele.2021.10168), a COVID-related misinformation dataset. The collection is based on news claims appearing in Politifact and Snopes. From each news claim, the authors extracted keywords that were then searched on Twitter to retrieve the associated content. Tweets were retrieved using the Faking it! tool4. The complete collection comprised 772 COVID-related news claims and 161,838 relevant tweets shared during 2020. From these claims, ``26%`` and ``74%`` were labeled as authentic and fake content, respectively, according to the Politifact and Snopes label of the associated news claim. Tweets’ labels were used to determine whether users were fake news spreaders. To this end, we computed a user score based on the proportion of shared tweets associated with fake news. Then, users were deemed as spreaders if the proportion of shared fake content was higher than a certain threshold. For the purpose of the evaluation, we adopted a conservative definition of spreaders, setting the threshold to ``0.5``.

Note: As per Twitter TOS, the shared graphs only include user and tweet ids involved in the interactions. No user information was included in the analysis.

Note2: The original data collection is available at [Zenodo](https://doi.org/10.5281/zenodo.4441377).

Note 3: The [``Faking it!``](https://github.com/knife982000/FakingIt) can be used to rehydrate the tweets collection based on the ids in the graphs.

The ``feature_sets`` folder includes the hand-crafted features used as part of the model.

#### Tweet graph

[NetworkX](https://networkx.org/) was used for supporting the graph.

##### Node attributes

* ``id``. Tweet ID.
* ``user_id``. User id.
* ``created_at``. ``datetime.datetime``.
* ``group``. Whether the tweet was tagged as fake or authentic. A ``nan`` value indicates that the tweet was not tagged.

Example of node with attributes:

```
(1248723016214663168, {'user_id': 3058777662, 'created_at': datetime.datetime(2020, 4, 10, 21, 22, 21), 'group':0.0})
```

##### Edge attributes

* ``source_id``. Tweet ID.
* ``target_id``. Tweet ID.
* ``{relation_type : datetime.datetime}``. For example, ``parent`` (a quote) or ``replies``.

Example of edge with attributes:
```
(1248723016214663168, 1248711880878608384, {'parent': datetime.datetime(2020, 4, 10, 21, 22, 21)})
```

#### User graph

[NetworkX](https://networkx.org/) was used for supporting the graph.

##### Node attributes

* ``id``. User ID.
* ``score_graph``. % of tweets in the graph that are fake.
* ``tweets``. Optional. It is a list of tuples, where each tuple represents ``(tweet_id, datetime.datetime)``. These tweets do not imply any relation with other users, or the users with whom they are relating do not belong to the graph.

Example of node with attributes:

```
(66873, {'score_graph': 0.0, 'tweets': [(1341337268280320002, datetime.datetime(2020, 12, 22, 10, 58, 20)), (1280272042747707393, datetime.datetime(2020, 7, 6, 22, 46, 55)), (1280280935041302529, datetime.datetime(2020, 7, 6, 23, 22, 16)), (1280279501289795584, datetime.datetime(2020, 7, 6, 23, 16, 34)), (1280279395673018369, datetime.datetime(2020, 7, 6, 23, 16, 9)), (1280279202542039040, datetime.datetime(2020, 7, 6, 23, 15, 22)), (1280284222901583874, datetime.datetime(2020, 7, 6, 23, 35, 19))]})
```

##### Edge attributes

* ``source_id``. User ID.
* ``target_id``. User ID.
* ``weight``. Number of interactions between the users.
* ``min_date``, ``max_date``, ``datetime.datetime``. The first and last date of user interactions.
* ``replies``, ``mentions``, ... List of ``tweet_id`` for each type of interaction.
* ``replies_date``, ``mentions_date``, ... List of ``datetime.datetime`` for each type of interactions.

Example of edge with attributes:
```
(1048911, 239509917, {'replies_date': [datetime.datetime(2020, 3, 12, 20, 39, 59)], 'replies': [1238203103310229506], 'mentions_date': [datetime.datetime(2020, 3, 12, 20, 39, 59)], 'mentions': [1238203103310229506], 'weight': 2, 'min_date': datetime.datetime(2020, 3, 12, 20, 39, 59), 'max_date': datetime.datetime(2020, 3, 12, 20, 39, 59)})
```


## Contact info:

* [Antonela Tommasel](https://tommantonela.github.io) (antonela.tommasel@isistan.unicen.edu.ar)

The repository is licenced under the Apache License V2.0. 
