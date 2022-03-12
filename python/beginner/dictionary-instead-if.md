# Pythonic way to use Dictionary

here is an example

    sampleDict = {}
    print(sampleDict)
    print(sampleDict.get('minutes', 0))
    sampleDict.setdefault('hours', 10)
    print(sampleDict)
    print(sampleDict.get('hours', 0))

output

    {}
    0
    {'hours': 10}
    10

Manipulate it

    def dosomethingWorse(season):
        if season == 'Winter':
            holiday = 'cold'
        elif season == 'Spring':
            holiday = 'good'
        elif season == 'Summer':
            holiday = 'hot'
        elif season == 'Fall':
            holiday = 'awesome'
        else:
            holiday = 'I dunno'
        print(holiday)

    def dosomethingBetter(season):
        holiday = {
            'Winter': 'cold',
            'Spring': 'good',
            'Summer': 'hot',
            'Fall':   'awesome'
        }.get(season, 'I dunno')
        print(holiday)

    dosomethingWorse('hello')
    dosomethingBetter('hello')

output

    I dunno
    I dunno