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

```
