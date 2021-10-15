# FCC_Intermediate_Algorithm

### We'll pass you an array of two numbers. Return the sum of those two numbers plus the sum of all the numbers between them. The lowest number will not always come first.
```js
function sumAll(arr) {
  let start=arr[0]
  let end=arr[arr.length-1]

  let max=start>end?start:end
  let min=start>end?end:start

  let sum=0

  let cal=(bigNum,smallNum)=>{
    sum+=bigNum
    bigNum--
    return bigNum>=smallNum?cal(bigNum,smallNum):sum
  }
  
  return cal(max,min)
}

sumAll([1, 4]);
```

### Compare two arrays and return a new array with any items only found in one of the two given arrays, but not both. In other words, return the symmetric difference of the two arrays.
```js
function diffArray(arr1, arr2) {
  var newArr = [];
  let obj={}
  let arr3=arr1.concat(arr2)
  arr3.forEach(item=>{
    if(item in obj){
      obj[item]++
    }else{
      obj[item]=1
    }
  })
  Object.keys(obj).forEach(key=>{
    if(obj[key]==1) isNaN(Number(key))?newArr.push(key):newArr.push(Number(key))
  })
  return newArr;
}

diffArray([1, 2, 3, 5], [1, 2, 3, 4, 5]);
```

### You will be provided with an initial array (the first argument in the destroyer function), followed by one or more arguments. Remove all elements from the initial array that are of the same value as these arguments.
```js
function destroyer(arr) {
  let args=Array.from(arguments)
  args.shift()
  return arr.filter(item=>args.indexOf(item)==-1)
}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);
```

### Make a function that looks through an array of objects (first argument) and returns an array of all objects that have matching name and value pairs (second argument). Each name and value pair of the source object has to be present in the object from the collection if it is to be included in the returned array.
```js
function whatIsInAName(collection, source) {
  var arr = [];
  // Only change code below this line
  collection.forEach((obj,index)=>{
    let bool=Object.keys(source).every(key=>obj.hasOwnProperty(key)&&obj[key]==source[key])
    if(bool) arr.push(obj)
  })

  // Only change code above this line
  return arr;
}

whatIsInAName([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });
```

### Convert a string to spinal case. Spinal case is all-lowercase-words-joined-by-dashes.
```js
function spinalCase(str) {
  let arr=str.split('')
  arr.forEach((item,index)=>{
    if(item==' ') arr[index]='-'
    if(item=='_') arr[index]='-'
    if(item.match(/^.*[A-Z]+.*$/)&&index!=0&&arr[index-1].match(/^.*[a-z]+.*$/)){
      arr.splice(index,0,'-')
    }
  })
  return arr.join('').toLowerCase()
}

spinalCase('This Is Spinal Tap')
```

### Pig Latin is a way of altering English Words. The rules are as follows:
- If a word begins with a consonant, take the first consonant or consonant cluster, move it to the end of the word, and add ay to it.
- If a word begins with a vowel, just add way at the end.
```js
function translatePigLatin(str) {
  let firstChar=str.charAt(0)
  let isVowel=(char)=>{
    if(char=='a'||char=='e'||char=='i'||char=='o'||char=='u') return true
    return false
  }
  let strToArr=(s)=>s.split('')
  let allConsonant=(s)=>strToArr(s).every(item=>!isVowel(item))

  let transfer=(arr)=>{
    let firstChar=arr.shift()
    arr.push(firstChar)
    return isVowel(arr[0])?arr:transfer(arr)
  }
  let headToTail=(s=>{
    if(s.length<2) return s
    let arr=strToArr(s)
    return transfer(arr).join('')+'ay'
  })

  if(!isVowel(firstChar)){
    if(allConsonant(str)) return str+'ay'
    return headToTail(str)
  }
  return str+'way';
}

translatePigLatin("consonant")
```

### Search and Replace
- Perform a search and replace on the sentence using the arguments provided and return the new sentence.
- First argument is the sentence to perform the search and replace on.
- Second argument is the word that you will be replacing (before).
- Third argument is what you will be replacing the second argument with (after).
```js
function myReplace(str, before, after) {
  let isUpperCase=before.charAt(0).match(/^.*[A-Z]+.*$/)
  let head=after.charAt(0)
  let body=after.substr(1)

  if(isUpperCase&&!head.match(/^.*[A-Z]+.*$/)){
    after=head.toUpperCase()+body
  }else if(!isUpperCase&&head.match(/^.*[A-Z]+.*$/)){
    after=head.toLowerCase()+body
  }

  let len=before.length
  let index=str.indexOf(before)
  return str.substr(0,index)+after+str.substr(index+len);
}

myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");
```

### DNA Pairing
- The DNA strand is missing the pairing element. Take each character, get its pair, and return the results as a 2d array.
- Base pairs are a pair of AT and CG. Match the missing element to the provided character.
- Return the provided character as the first element in each array.
- For example, for the input GCG, return [["G", "C"], ["C","G"], ["G", "C"]]
```js
function pairElement(str) {
  let DNA=[]
  let RNA=str.split('')

  RNA.forEach(item=>{
    switch(item){
      case 'A':
        DNA.push(['A','T'])
        break
      case 'T':
        DNA.push(['T','A'])
        break
      case 'C':
        DNA.push(['C','G'])
        break
      default:
        DNA.push(['G','C'])
    }
  })
  return DNA;
}

pairElement("GCG");
```

### Find the missing letter in the passed letter range and return it
```js
function fearNotLetter(str) {
  let standard='abcdefghijklmnopqrstuvwxyz'
  if(str==standard) return undefined
  
  let basic=standard.indexOf(str.charAt(0))
  let count=1
  let target=null

  while(count<=str.length){
    let index=standard.indexOf(str.charAt(count))
    if(index!=basic+1){
      target=basic+1
      break
    }else{
      basic=index
    }
    count++
  }
  
  return target==null?undefined:standard.charAt(target);
}

fearNotLetter("abce");
```

### Sorted Union
- Write a function that takes two or more arrays and returns a new array of unique values in the order of the original provided arrays.
- In other words, all values present from all arrays should be included in their original order, but with no duplicates in the final array.
- The unique numbers should be sorted by their original order, but the final array should not be sorted in numerical order.
```js
function uniteUnique(arr) {
  let restArr=Array.from(arguments)
  restArr.shift()
  restArr.forEach(item=>{
    item.forEach(jtem=>{
      if(arr.indexOf(jtem)==-1){
        arr.push(jtem)
      }
    })
  })
  return arr;
}

uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);
```

### Convert the characters &, <, >, " (double quote), and ' (apostrophe), in a string to their corresponding HTML entities.
```js
function convertHTML(str) {
  let arr=str.split('')
  arr.forEach((item,index)=>{
    switch(item){
      case '&':
        arr[index]='&amp;'
        break
      case '<':
        arr[index]='&lt;'
        break
      case '>':
        arr[index]='&gt;'
        break
      case '"':
        arr[index]='&quot;'
        break
      case "'":
        arr[index]='&apos;'
        break
      default:
        break
    }
  })
  return arr.join('')
}

convertHTML("Dolce & Gabbana");
```

### Sum All Odd Fibonacci Numbers
- Given a positive integer num, return the sum of all odd Fibonacci numbers that are less than or equal to num.
- The first two numbers in the Fibonacci sequence are 1 and 1. Every additional number in the sequence is the sum of the two previous numbers. The first six numbers of the Fibonacci sequence are 1, 1, 2, 3, 5 and 8.
- For example, sumFibs(10) should return 10 because all odd Fibonacci numbers less than or equal to 10 are 1, 1, 3, and 5.
```js
function sumFibs(num) {
  if(num==1) return 2

  let createFibs=(fibs=[1,1],num)=>{
    let len=fibs.length
    let pre1=fibs[len-2]
    let pre2=fibs[len-1]
    let sum=pre1+pre2
    if(sum>num){
      return fibs
    }else{
      fibs.push(sum)
      return createFibs(fibs,num)
    }
  }

  let fibs=createFibs([1,1],num)
  let sum=0
  fibs.forEach(item=>{
    if(item%2!=0) sum+=item
  })
  return sum;
}

sumFibs(4);
```

### Sum All Primes
- A prime number is a whole number greater than 1 with exactly two divisors: 1 and itself. For example, 2 is a prime number because it is only divisible by 1 and 2. In contrast, 4 is not prime since it is divisible by 1, 2 and 4.
- Rewrite sumPrimes so it returns the sum of all prime numbers that are less than or equal to num.
```js
function sumPrimes(num) {
  let isPrime=(num,divisor)=>{
    if(!divisor) divisor=num-1
    if(num%divisor==0){
      return divisor!=1?false:true
    }else{
      divisor--
      return isPrime(num,divisor)
    }
  }

  let createPrimeArr=(num)=>{
    let arr=[]
    while(num>0){
      if(isPrime(num)){
        arr.push(num)
      }
      num-- 
    }
    return arr
  }

  let sum=0
  createPrimeArr(num).forEach(item=>sum+=item)
  return sum
}

sumPrimes(10);
```

### Smallest Common Multiple
- Find the smallest common multiple of the provided parameters that can be evenly divided by both, as well as by all sequential numbers in the range between these parameters.
- The range will be an array of two numbers that will not necessarily be in numerical order.
```js
function smallestCommons(arr) {
  let head=arr[0]
  let tail=arr[1]
  let max=head>tail?head:tail
  let min=head>tail?tail:head

  let createArr=(min,max)=>{
    let arr=[]
    while(min<=max){
      arr.push(min)
      min++
    }
    return arr
  }
  
  let newArr=createArr(min,max)
  let common=max

  let findCommonMultiple=()=>{
    let bool=newArr.every(item=>common%item==0)
    if(bool){
      return common
    }else{
      common++
      return findCommonMultiple
    }
  }
  
  //解决栈溢出
  let excuteFunc=(func)=>{
    let value=func()
    while(typeof value=='function'){
      value=value()
    }
    return value
  }

  return excuteFunc(findCommonMultiple)
}


smallestCommons([1,5]);
```

### Drop it
- Given the array arr, iterate through and remove each element starting from the first element (the 0 index) until the function func returns true when the iterated element is passed through it.
- Then return the rest of the array once the condition is satisfied, otherwise, arr should be returned as an empty array.
```js
function dropElements(arr, func) {
  if(arr.length==0) return []
  if(func(arr[0])){
    return arr
  }else{
    arr.shift()
    return dropElements(arr,func)
  }
}

dropElements([1, 2, 3], function(n) {return n < 3; });
```

### Flatten a nested array. You must account for varying levels of nesting.
```js
function steamrollArray(arr) {
  let newArr=[]
  let flat=(arr)=>{
    arr.forEach(item=>{
      if(item instanceof Array){
        flat(item)
      }else{
        newArr.push(item)
      }
    })
  }
  
  flat(arr)
  
  return newArr
}

steamrollArray([1, [2], [3, [[4]]]]);
```

### Return an English translated sentence of the passed binary string.
```js
function binaryAgent(str) {
  let target=[]
  let source=str.split(' ')
  source.forEach(code=>{
    let asciiCode=parseInt(code,2)
    let char=String.fromCharCode(asciiCode)
    target.push(char)
  })
  return target.join('')
}

binaryAgent("01000001 01110010 01100101 01101110 00100111 01110100 00100000 01100010 01101111 01101110 01100110 01101001 01110010 01100101 01110011 00100000 01100110 01110101 01101110 00100001 00111111");
```

### Everything Be True
- Check if the predicate (second argument) is truthy on all elements of a collection (first argument).
- In other words, you are given an array collection of objects. The predicate pre will be an object property and you need to return true if its value is truthy. Otherwise, return false.
```js
function truthCheck(collection, pre) {
  let bool=collection.every(obj=>{
    return obj.hasOwnProperty(pre)&&Boolean(obj[pre])
  })
  return bool
}

truthCheck([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}, {"user": "Laa-Laa", "sex": "female"}, {"user": "Po", "sex": "female"}], "sex");
```

### Create a function that sums two arguments together. If only one argument is provided, then return a function that expects one argument and returns the sum.
```js
function addTogether() {
  let args=Array.from(arguments)
  let len=args.length
  let arg0=args[0]

  let isNum=(num)=>(typeof num=='number')

  if(len==1){
    if(!isNum(arg0)) return undefined
    return function(){
      let args=arguments[0]
      if(!isNum(args)) return undefined
      return arg0+args
    }
  }else if(len==2){
    let arg1=args[1]
    if(isNum(arg0)&&isNum(arg1)){
      return arg0+arg1
    }
    return undefined
  }
}

addTogether(2,3);
```

### Fill in the object constructor with the following methods below:
```js
getFirstName()
getLastName()
getFullName()
setFirstName(first)
setLastName(last)
setFullName(firstAndLast)
```

```js
var Person = function(firstAndLast) {
  // Only change code below this line
  let name=firstAndLast.split(' ')
  let first=name[0]
  let last=name[1]

  this.getFirstName=()=>{
    return first
  }
  this.setFirstName=(newName)=>{
    first=newName
  }

  this.getLastName=()=>{
    return last
  }
  this.setLastName=(newName)=>{
    last=newName
  }

  this.setFullName=(newName)=>{
    name=newName.split(' ')
    first=name[0]
    last=name[1]
  }
  // Complete the method below and implement the others similarly
  this.getFullName = function() {
    return first+' '+last
  };
  return firstAndLast;
};

var bob = new Person('Bob Ross');
bob.getFullName();
```

### Map the Debris
- Return a new array that transforms the elements' average altitude into their orbital periods (in seconds).
- The array will contain objects in the format {name: 'name', avgAlt: avgAlt}.
- You can read about orbital periods on Wikipedia.
- The values should be rounded to the nearest whole number. The body being orbited is Earth.
- The radius of the earth is 6367.4447 kilometers, and the GM value of earth is 398600.4418 km3s-2.
```js
function orbitalPeriod(arr) {
  var GM = 398600.4418;
  var earthRadius = 6367.4447;
  //公式 T = 2π * (a^3 / μ)^(-1/2) 中的 a 在题目中等于 avgAlt + earthRadius，μ值就是GM
  
  let result=[]
  arr.forEach(obj=>{
    let name=obj.name
    let orbitalPeriod = Math.round(Math.sqrt(Math.pow((earthRadius + obj.avgAlt), 3) / GM) * 2 * Math.PI)
    result.push({name,orbitalPeriod})
  })

  return result
}

orbitalPeriod([{name : "sputnik", avgAlt : 35873.5553}]);
```

### Palindrome Checker
```js
function palindrome(str) {
  let arr=str.toUpperCase().split('')
  let newArr=[]

  arr=arr.filter(item=>item!=' ')

  arr.forEach(item=>{
    if(item.match(/^.*[A-Z]+.*$/)||!isNaN(Number(item))){
      newArr.push(item)
    }
  })
  
  let len=newArr.length
  if(len%2==1) newArr.splice(Math.floor(len/2),1)
  return newArr.join('')===newArr.reverse().join('')
}



palindrome("_$#:, eye");
```

### Roman Numeral Converter
```js

```
