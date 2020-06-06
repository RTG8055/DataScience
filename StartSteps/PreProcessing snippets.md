### Convert to Date
 - today = datetime.datetime(2017, 10, 20)
 - datetime.strptime(date_string, format)
 - train['cnv_date'] = pd.to_datetime(train.DATE)
 - train['day_of_week'] = train.cnv_date.apply(lambda x: x.dayofweek)
 - train['weekend_or_not'] = train.day_of_week.apply(lambda x: 1 if x==6 or x==0 else 0)


https://www.journaldev.com/23365/python-string-to-datetime-strptime
