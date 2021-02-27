# JS interview tasks 

Here I collects some tasks that I met and solved by me. 
Usually, here is my first solution. 
In time I'll try to replace this solutions to the best I can

### ATM 
```
const limits = { 1000: 6; 500: 7; 100: 5; 50: 4 }

atm(1250, limits)       // '1x1000 2x100 1x50'
atm(1000000, limits)    // 'Error: Not enough money'
atm(2400, limits)       // '2x1000 4x100 2x50'
atm(6512, limits)       // 'Error: Incorrect value'
atm(50, limits)         // '1x50'
atm(5500, limits)       // '3x1000 5x500'

function atm(sum, limits) {
    const keys = Object.keys(limits).reverse().map(Number)
    const result = {}
    for(const key of keys) {
        const max = Math.max(limits[key], Math.floor(sum / key))
        sum -= max * key
        result[key] = max
    }
    if(sum > 0) {
        return 'Error: Not enough money'
    }
    for(const key of keys) {
        limits[key] -= result[key]
    }
    return Object.entries(result).reverse().map(([count, value]) => `${count}x${value}`).join(' ')
}
```

### Ordered count 

```
orderedCount('abracadabra') // [['a', 5], ['b', 2], ['r', 2], ['c', 1], ['d', 1]]

function orderedCount(str) {
  const array = str.split('')
  const obj = array.reduce((acc, value) => {
    acc[value] ? acc[value] += 1 : acc[value] = 1
    return acc
  }, {})
  const unique = [...new Set(array)]
  return unique.map(i => [i, obj[i]])  
}
```

### Dump (tree)

```
dump(object) //  => [3, 4, 0, 2, 5]

// with recursion
function dump(tree) {
  const result = []
  result.push(tree.value)
  if(tree.children !== undefined) { 
    for(let child of tree.children) {
      result.push(...dump(child))
    }
  }
  return result
}

const object = {
  value: 3,
  children: [
    {
      value: 4, 
      children: [
        {
          value: 0
        }, 
        {
          value:2
        }
      ]
    },
    {
      value: 5
    }
  ]
}
```

### Flatten

```
flatten([1, [2, [3, [4]]], 5]) // [1, 2, 3, 4, 5]

// with recursion
function flatten(arr) {
  const result = []
  for(let i of arr) {
    if(!Array.isArray(i)) {
      result.push(i)
    } else {
      result.push(...flatten(i))
    }
  }
  return result
}
```

### Compress string

```
compress('AAABBOPP') // '3A2BO2P'

function compress(str) {
  let result = ''
  let count = 0
  for(let i = 0; i < str.length; i++) {
    count++
      if(str[i] !== str[i + 1]) {
        result += (count > 1 ? count : '') + str[i]
        count = 0
      }
  }
  return result
}
```

### Sort and group array of operations

``` 
/*  
 * There is an array of operations
 * Need to sort it by date and group by year
 * result should be like this: 
  result = {
    '2017': [
      '07-31',
      '08-22'
    ],
    '2018': [
      '01-01',
      '02-22'
    ] 
  }
 *
*/

const operations = [
  { "date": "2017-07-31", "amount": "5422" },
  { "date": "2017-06-30", "amount": "5220" },
  { "date": "2017-05-31", "amount": "5365" },
  { "date": "2017-08-31", "amount": "5451" },
  { "date": "2017-09-30", "amount": "5303" },
  { "date": "2018-03-31", "amount": "5654" },
  { "date": "2017-10-31", "amount": "5509" },
  { "date": "2017-12-31", "amount": "5567" },
  { "date": "2018-01-31", "amount": "5597" },
  { "date": "2017-11-30", "amount": "5359" },
  { "date": "2018-02-28", "amount": "5082" },
  { "date": "2018-04-14", "amount": "2567" }
]

function sortOperations(operations) {
  const result = {}

  const years = operations.reduce((acc, value) => {
    const year = value.date.split('-')[0]
      if(!acc.includes(year)) {
        acc.push(year)
      }
      return acc
  }, [])

  const data = []
  for(let year of years) {
    let temp = operations.filter(op => op.date.split('-')[0] === year)
    result[year] = temp.map(op => op.date.slice(5)).sort()
  }
  return result
}
```

### Tricky question about var and context
``` 
// #1
// find out what is wrong and solve in maximum ways
var o = {
  x: 10,
  foo() {
    for(var i = 0; i < 10; i++) {
      setTimeout(function() {
        console.log(i + this.x)
      }, 7)
    }
  }
}    

// #2
var indexes = (function () {
  var arr = [];
  for (var i = 0; i <= 4; i++) {
    arr.push(() => i);
  }
  return arr;
})()
var index = indexes[2];

console.log(index());

```

### Draw a nesting
``` 
const data = [
  'a',
  ['b', 'c'],
  'd',
  ['e', 'f', ['g', 'h']],
  'i',
]

function draw(data) {
  // TODO
}
```

### Move arr elements on given step
if step is positive move right, else move left
```
move([1, 2, 3, 4, 5], 2) // [4, 5, 1, 2, 3]
move([1, 2, 3, 4, 5], -2) // [3, 4, 5, 1, 2]

function move(arr, step) {
  step = step % arr.length
  return arr.slice(-step).concat(arr.slice(0, -step))
}
```

### Get all letters and their count

```
getChars('Saratov') // 's:*, a:**, r:*, t:*, o:*, v:*'
getChars('Saint Petersburg') // 's:**, a:*, i:*, n:*, t:**, p:*, e:**, r:**, b:*, u:*, g:*'

function getChars(city) {
  const arr = city.replace(/[^a-zA-Zs]/g, "").split('').map(i => i.toLowerCase())
  const obj = {}
  for(let i of arr) {
    obj[i] ? obj[i] += 1 : obj[i] = 1
  }
  const unique = [...new Set(arr)]
  return unique.map((i, index) => `${i}:${'*'.repeat(obj[i])}${index !== unique.length - 1 ? ',' : ''}`).join(' ')
}
```

### Reverse array
If it starts with odd number reversed should start also with odd number
``` 
sort([1, 2, 3, 4, 5, 6, 7, 8]) // [7, 8, 5, 6, 3, 4, 1, 2]
sort([2, 3, 4, 5, 6]) // [6, 5, 4, 3, 2]
sort([2, 3, 4, 5, 6, 7]) // [6, 7, 4, 5, 2, 3]

function sort(arr: Array<number>): Array<number> {
    // if elements length is odd just reverse it 
    if(arr.length % 2 !== 0) {
        return arr.reverse()
    }
    const result = [...arr]
    for(let i = 0; i < arr.length / 2; i++) {
        const secondIndex = i % 2 === 0 
            ? result.length - 2 - i
            : result.length - i;
        [result[i], result[secondIndex]] = [result[secondIndex], result[i]]    
    }
    return result
}
```


