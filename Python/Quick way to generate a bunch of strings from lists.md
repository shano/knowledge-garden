
#til #blog #tech/python
Tidy way of generating a bunch of strings

```python
	expected = [
        f'{region}:{account}:stack/system-stack-{stack}'
        for region in ['eu-west-1', 'eu-west-2']
        for account in ['1234', '5678']
        for stack in ['TestStack1', 'TestStack2']
    ]
```
