```python
comments            = sorted(question_comments + answer_comments, 
                                     key = lambda i: i['pub_date'],
                                     reverse=True
                                     )
```

참고

* https://stackoverflow.com/questions/652291/sorting-a-list-of-dictionary-values-by-date-in-python

* https://www.geeksforgeeks.org/ways-sort-list-dictionaries-values-python-using-lambda-function/