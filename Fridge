import datetime
from decimal import Decimal

DATE_FORMAT = '%Y-%m-%d'


def add(items, title, amount, expiration_date=None):
    if expiration_date is not None:
        expiration_date = datetime.datetime.strptime(str(expiration_date), DATE_FORMAT).date()
    if title in items:
        items[title].append({'amount': Decimal(f'{amount}'), 'expiration_date': expiration_date})
    else:
        items[title] = [{'amount': Decimal(amount), 'expiration_date': expiration_date}]    


def add_by_note(items, note):
    str = note.split()
    last_part = str[-1]
    if last_part and len(str) > 2 and len(last_part) == 10:
        expiration_date = datetime.datetime.strptime(last_part, DATE_FORMAT).date()
        title = ' '.join(str[:- 2])
        add(items, title, Decimal(str[-2]), expiration_date)
    else:
        title = ' '.join(str[:- 1])
        add(items, title, Decimal(str[-1]), None)
  

def find(items, needle):
    output = []
    for item in items:
        if str.lower(needle) in str.lower(item):
            output.append(item)
    return output


def amount(items, needle):
    items_amount = 0
    needle_number = needle.lower()
    for key, value in items.items():
        if needle_number in key.lower():
            items_amount += sum([item['amount'] for item in value])
    return Decimal(items_amount)


def expire(items, in_advance_days=0):
    expired_items = []
    date_to_expire = datetime.date.today() + datetime.timedelta(in_advance_days)
    for item, details in items.items():
        amount_expire = Decimal('0')
        for k in details:
            if k['expiration_date'] is not None and date_to_expire >= k['expiration_date']:
                amount_expire += k['amount']
        if amount_expire > Decimal('0'):
            expired_items.append(tuple((item, amount_expire)))
    return expired_items
