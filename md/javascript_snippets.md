# Сниппеты (утилиты)

[На главную](../README.md)

### Содержание

- [Число](#number)
- [Строка](#string)
- [Массив](#array)
- [Объект](#object)
- [Функция](#function)
- [DOM](#dom)
- [Разное](#diff)

## <a name="number"></a> Число

```js
/**
 * Проблема чисел с плавающей точкой
 */
0.1 + 0.2 === 0.3 // false

const isEqual = (x, y) => x - y < Number.EPSILON

isEqual(0.1 + 0.2, 0.3) // true


// --- / --- / --- / --- //
/**
 * Четность/нечетность числа
 */
// 1
const isEven = (n) => n % 2 === 0
// 2
const _isEven = (n) => !(n & 1)

isEven(1) // false
_isEven(4) // true

// 1
const isOdd = (n) => n % 2 !== 0
// 2
const _isOdd = (n) => !!(n & 1)

isOdd(1) // true
_isOdd(4) // false


// --- / --- / --- / --- //
/**
 * Является ли число степенью числа 2?
 */
const isPowerOfTwo = (n) => !!n && (n & (n - 1)) === 0

isPowerOfTwo(3) // false
isPowerOfTwo(8) // true


// --- / --- / --- / --- //
/**
 * Сумма чисел, входящих в число
 */
const sum = (n) => (n * (n + 1)) / 2

sum(100) // 5050


// --- / --- / --- / --- //
/**
 * Делится ли одно число на другое без остатка?
 */
const isDivisible = (x, y) => x % y === 0

isDivisible(4, 2) // true
isDivisible(5, 3) // false


// --- / --- / --- / --- //
/**
 * Находится ли число в заданном диапазоне?
 */
const isInRange = (n, start, end = null) => {
  if (end && start > end) [end, start] = [start, end]
  return end === null ? n >= 0 && n < start : n >= start && n < end
}

isInRange(2, 0, 10) // true
isInRange(2, 3, 9) // false


// --- / --- / --- / --- //
/**
 * Округление числа до указанного количества знаков после запятой
 */
const round = (n, d = 0) => +`${Math.round(`${n}e${d}`)}e-${d}`

round(1.005, 2) // 1.01


// --- / --- / --- / --- //
/**
 * Сохранение знака числа
 */
const saveSign = (x) => (!Math.sign(x) ? -x : x)

saveSign(-2) // -2


// --- / --- / --- / --- //
/**
 * Частное и остаток
 */
const divMod = (x, y) => [~~(x / y), x % y]

divMod(4, 2) // [2, 0]
divMod(5, 3) // [1, 2]


// --- / --- / --- / --- //
/**
 * Ближайшая степень второго числа
 */
const nthRoot = (x, y) => Math.pow(x, 1 / y)

nthRoot(32, 5) // 2


// --- / --- / --- / --- //
/**
 * Натуральные (простые) числа
 */
// 1
const primes = (num) => {
  let arr = Array.from({ length: num - 1 }).map((_, i) => i + 2),
    root = ~~Math.sqrt(num),
    nums = Array.from({ length: root - 1 }).map((_, i) => i + 2)

  nums.forEach((x) => (arr = arr.filter((y) => y % x !== 0 || y === x)))
  return arr
}

primes(10) // [2, 3, 5, 7]

// Решето Эратосфена
const _primes = (num) => {
  const arr = new Array(num + 1)
  arr.fill(true)
  arr[0] = arr[1] = false

  for (let i = 2; i <= Math.sqrt(num); i++) {
    for (let j = 2; i * j <= num; j++) {
      arr[i * j] = false
    }
  }

  return arr.reduce(
    (primes, isPrime, prime) => (isPrime ? primes.concat(prime) : primes),
    []
  )
}

_primes(10) // [2, 3, 5, 7]


// --- / --- / --- / --- //
/**
 * Сложение и возведение в степень
 */
const sumPower = (end, power = 2, start = 1) =>
  Array(end + 1 - start)
    .fill(0)
    .map((_, i) => (i + start) ** power)
    .reduce((a, c) => a + c, 0)

sumPower(10) // 385
sumPower(10, 3) // 3025
sumPower(10, 3, 5) // 2925


// --- / --- / --- / --- //
/**
 * Арифметическая прогрессия
 */
const arithmeticProgress = (n, max) =>
  Array.from({ length: Math.ceil(max / n) }, (_, i) => (i + 1) * n)

arithmeticProgress(5, 25)
// [5, 10, 15, 20, 25]


// --- / --- / --- / --- //
/**
 * Геометрическая прогрессия
 */
const geometricProgress = (end, start = 1, step = 2) =>
  Array.from({ length: ~~(Math.log(end / start) / Math.log(step)) + 1 }).map(
    (_, i) => start * step ** i
  )

geometricProgress(256)
// [1, 2, 4, 8, 16, 32, 64, 128, 256]
geometricProgress(256, 1, 4)
// [1, 4, 16, 64, 256]


// --- / --- / --- / --- //
/**
 * Наибольший общий делитель
 */
const gcd = (...arr) => {
  const _gcd = (x, y) => (!y ? x : gcd(y, x % y))
  return [...arr].reduce((a, b) => _gcd(a, b))
}

gcd(8, 36) // 4
gcd(...[12, 8, 32]) // 4


// --- / --- / --- / --- //
/**
 * Наименьший общий множитель
 */
const lcm = (...args) => {
  const gcd = (x, y) => (!y ? x : gcd(y, x % y))
  const _lcm = (x, y) => (x * y) / gcd(x, y)
  return [...args].reduce((a, b) => _lcm(a, b))
}

lcm(12, 7) // 84
lcm(...[1, 3, 4, 5]) // 60


// --- / --- / --- / --- //
/**
 * Преобразование числа в массив
 */
const digitize = (n) => [...`${Math.abs(n)}`].map((i) => parseInt(i))

digitize(123) // [1, 2, 3]


// --- / --- / --- / --- //
/**
 * Преобразование арабских чисел в римские
 */
const toRomanNum = (n) => {
  const lookup = [
    ['M', 1000],
    ['CM', 900],
    ['D', 500],
    ['CD', 400],
    ['C', 100],
    ['XC', 90],
    ['L', 50],
    ['XL', 40],
    ['X', 10],
    ['IX', 9],
    ['V', 5],
    ['IV', 4],
    ['I', 1]
  ]
  return lookup.reduce((a, [k, v]) => {
    a += k.repeat(~~(n / v))
    n = n % v
    return a
  }, '')
}

toRomanNumeral(3) // III
toRomanNumeral(11) // XI


// --- / --- / --- / --- //
/**
 * Инверсия числа с сохранением знака
 */
const reverseNum = (n) =>
  parseFloat([...`${n}`].reverse().join('')) * Math.sign(n)

reverseNum(981) // 189
reverseNum(-500) // -5
reverseNum(73.6) // 6.37
reverseNum(-5.23) // -32.5


// --- / --- / --- / --- //
/**
 * Среднее значение
 */
const average = (...nums) => nums.reduce((a, c) => a + c, 0) / nums.length

average(...[1, 2, 3]) // 2
average(1, 2, 3) // 2


// --- / --- / --- / --- //
/**
 * Числа Фибоначчи
 */
const fib = (n) => {
  if (n < 0) return 'invalid'
  if (n <= 1) return n

  let a = 1,
    b = 1

  for (let i = 3; i <= n; i++) {
    let c = a + b
    a = b
    b = c
  }

  return b
}

fib(10) // 55


// --- / --- / --- / --- //
/**
 * Факториал
 */
const fact = (n) => {
  if (n < 0) return 'invalid'
  if (n <= 1) return 1

  return n * fact(n - 1)
}

fact(5) // 120


// --- / --- / --- / --- //
/**
 * Случайное целое число в заданном диапазоне
 */
const getRandomInt = (min, max) => ~~(min + Math.random() * (max - min + 1))


// --- / --- / --- / --- //
/**
 * Случайное положительное число (целое или дробное)
 * в заданном диапазоне
 */
const getRandomPosiviteNum = (min, max, p) => {
  if (min < 0 || min <= max) return 'invalid'
  const n = min + Math.random() * (max - min + 1)
  return !p ? ~~n : n.toFixed(p)
}


// --- / --- / --- / --- //
/**
 * Преобразование радиан в градусы и обратно
 */
const radToDeg = (rad) => (rad * 180) / Math.PI

const degToRad = (deg) => (deg * Math.PI) / 180


// --- / --- / --- / --- //
/**
 * Форматирование миллисекунд
 */
const formatMs = (ms) => {
  if (ms < 0) ms = -ms
  const time = {
    day: ~~(ms / 86400000),
    hour: ~~(ms / 3600000) % 24,
    minute: ~~(ms / 60000) % 60,
    second: ~~(ms / 1000) % 60,
    millisecond: ~~ms % 1000
  }
  return Object.entries(time)
    .filter((val) => val[1] !== 0)
    .map(([k, v]) => `${v} ${k}${v !== 1 ? 's' : ''}`)
    .join(', ')
}

formatMs(1001) // 1 second, 1 millisecond
formatMs(34325055574) // 397 days, 6 hours, 44 minutes, 15 seconds, 574 milliseconds


// --- / --- / --- / --- //
/**
 * Форматирование байтов
 * (латиница)
 */
const formatBytes = (num, precision = 3, addSpace = true) => {
  const UNITS = ['B', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']
  const space = addSpace ? ' ' : ''
  const sign = num < 0 ? -num : num

  if (Math.abs(num) < 1) return num + space + UNITS[0]

  const exponent = Math.min(~~(Math.log10(sign) / 3), UNITS.length - 1)
  const _num = +(sign / 1000 ** exponent).toPrecision(precision)

  return (num < 0 && '-') + _num + space + UNITS[exponent]
}

formatBytes(1000) // '1 KB'
formatBytes(-27145424323.5821, 5) // '-27.145 GB'
formatBytes(123456789, 3, false) // '123MB'


// --- / --- / --- / --- //
/**
 * Маскировка чисел
 */
const mask = (num, n = 4, mask = '*') =>
  `${num}`.slice(-n).padStart(`${num}`.length, mask)

mask(1234567890) // '******7890'
mask(1234567890, 3) // '*******890'
mask(1234567890, -4, '$') // '$$$$567890'


// --- / --- / --- / --- //
/**
 * Расстояние между двумя точками
 */
const distance = (x1, y1, x2, y2) => Math.hypot(x2 - x1, y2 - y1)

distance(0, 0, 10, 10) // 14.142135623730951


// --- / --- / --- / --- //
/**
 * Расстояние Хэмминга
 */
const hammingDistance = (num1, num2) =>
  ((num1 ^ num2).toString(2).match(/1/g) || '').length

hammingDistance(2, 3) // 1


// --- / --- / --- / --- //
/**
 * Биномиальный коэффициент
 */
const binomCoef = (n, k) => {
  if (Number.isNaN(n) || Number.isNaN(k)) return NaN
  if (k < 0 || k > n) return 0
  if (k === 0 || k === n) return 1
  if (k === 1 || k === n - 1) return n
  if (n - k < k) k = n - k
  let res = n
  for (let j = 2; j <= k; j++) res *= (n - j + 1) / j
  return Math.round(res)
}

binomCoef(8, 2) // 28
```

## <a name="string"></a> Строка

```js
/**
 * Инверсия слова
 */
// 1
const reverseWord = (str) => [...str].reverse().join('')

reverseWord('foo') // oof

// 2
const _reverseWord = (str) => {
  let res = ''
  for (const char of str) res = char + res
  return res
}

_reverseWord('bar') // rab

// 3
const __reverse = (str) => [...str].reduce((res, char) => char + res)

__reverse('baz') // zab

// --- / --- / --- / --- //
/**
 * Инверсия строки
 */
// только слов в строке
const reverseWords = (str) => str.split(' ').reverse().join(' ')

reverseWords('hello world') // world hello

// слов в строке и самих слов
const _reverseWords = (str) =>
  str
    .split(' ')
    .map((w) => [...w].reverse().join(''))
    .join(' ')

_reverseWords('hello world') // olleh dlrow

// --- / --- / --- / --- //
/**
 * "Капитализация" строки
 */
// одного слова
// 1
const capitilizeWord = (str) => `${str[0].toUpperCase()}${str.slice(1)}`

capitilizeWord('foo') // Foo

// 2
const _capitilizeWord = ([first, ...rest]) =>
  first.toUpperCase() + rest.join('').toLowerCase()

_capitilizeWord('bAr') // Bar

// дополнительный функционал
const __capitilizeWord = ([first, ...rest], lower = false) =>
  first.toUpperCase() + (lower ? rest.join('').toLowerCase() : rest.join(''))

__capitilizeWord('fooBar') // 'FooBar'
__capitilizeWord('fooBar', true) // 'Foobar'

// строки
// 1
const capitilizeWords = (str) =>
  str
    .split(' ')
    .map((w) => `${w[0].toUpperCase()}${w.slice(1)}`)
    .join(' ')

capitilizeWords('foo bar') // Foo Bar

// 2
const _capitalizeWords = (str) =>
  str.replace(/\b[a-zа-яё]/g, (w) => w.toUpperCase())

_capitalizeWords('hello world') // 'Hello World'

// --- / --- / --- / --- //
/**
 * Сокращение строки
 */
// n - количество букв без ...
const truncateWord = (str, n) =>
  str.length > n ? str.slice(0, n > 3 ? n : 3) + '...' : str

truncateWord('boomerang', 4) // boom...

// n - количество букв с ...
const _truncateWord = (str, n) =>
  str.length > n ? str.substr(0, n - 3) + '...' : str

_truncateWord('JavaScript', 7) // Java...

// n - количество слов
const truncateWords = (str, n) => str.split(' ').slice(0, n).join(' ')

truncateWords('JavaScript is awesome!', 1) // JavaScript

// дополнительный функционал
const _truncateWords = (str, max, end = '...') => {
  if (str.length <= max) return str
  const last = str.slice(0, max - end.length + 1).lastIndexOf(' ')
  return str.slice(0, last > 0 ? last : max - end.length) + end
}

_truncateWords('short', 10) // 'short'
_truncateWords('not so short', 10) // 'not so...'
_truncateWords('trying a thing', 10) // 'trying...'
_truncateWords('javascripting', 10) // 'javascr...'

// --- / --- / --- / --- //
/**
 * Определение количества вхождений подстроки в строку
 */
const countSub = (str, sub) => {
  let c = 0,
    i = 0
  while (true) {
    const r = str.indexOf(sub, i)
    if (r !== -1) [c, i] = [c + 1, r + 1]
    else return c
  }
}

countSub('tiktok tok tok tik tik', 'tik') // 3

// --- / --- / --- / --- //
/**
 * (латиница)
 */
const toTitleCase = (str) =>
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map((i) => `${i[0].toUpperCase()}${i.slice(1)}`)
    .join(' ')

toTitleCase('hello world') // Hello World
toTitleCase('hello-world') // Hello World

// --- / --- / --- / --- //
/**
 * (латиница)
 */
const toCamelCase = (str) => {
  let s =
    str &&
    str
      .match(
        /[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g
      )
      .map((c) => `${c[0].toUpperCase()}${c.slice(1).toLowerCase()}`)
      .join('')

  return `${s[0].toLowerCase()}${s.slice(1)}`
}

toCamelCase('hello world') // helloWorld
toCamelCase('hello-world') // helloWorld

// --- / --- / --- / --- //
/**
 * (латиница)
 */
const toSnakeCase = (str) =>
  str &&
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map((c) => c.toLowerCase())
    .join('_')

toSnakeCase('helloWorld') // hello_world
toSnakeCase('hello-world') // hello_world

// --- / --- / --- / --- //
/**
 * (латиница)
 */
const toKebabCase = (str) =>
  str &&
  str
    .match(/[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g)
    .map((l) => l.toLowerCase())
    .join('-')

toKebabCase('helloWorld') // hello-world
toKebabCase('hello world') // hello-world

// --- / --- / --- / --- //
/**
 * Являются ли строки анаграммами?
 * (латиница и кириллица)
 * https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0
 */
// 1
const isAnagrams = (x, y) => {
  const fn = (str) =>
    str
      .toLowerCase()
      .replace(/[^a-zа-яё0-9]/gi, '')
      .split('')
      .sort()
      .join('')
  return fn(x) === fn(y)
}

isAnagrams('воз', 'зов') // true

// 2
const sort = (str) => [...str.replace(/\W/g, '').toLowerCase()].sort().join('')

const _isAnagrams = (a, b) => sort(a) === sort(b)

_isAnagrams('listen', 'silent') // true

// 3
const __isAnagrams = (x, y) =>
  x.length === y.length &&
  [...x.toLowerCase()].every((c) => y.toLowerCase().includes(c))

__isAnagrams('the eyes', 'they see') // true

// --- / --- / --- / --- //
/**
 * Является ли строка палиндромом?
 * (латиница и кириллица)
 * https://ru.wikipedia.org/wiki/%D0%9F%D0%B0%D0%BB%D0%B8%D0%BD%D0%B4%D1%80%D0%BE%D0%BC
 */
// 1
const isPalindrome = (str) =>
  [...str.toLowerCase().replace(/\W/g, '')].every(
    (c, i) => c === str[str.length - 1 - i]
  )

isPalindrome('А роза упала на лапу Азора') // true

// 2
const _isPalindrome = (str) => (
  (str = str.toLowerCase().replace(/\W/g, '')),
  str === [...str].reverse().join('')
)

_isPalindrome('Borrow or rob') // true

// --- / --- / --- / --- //
/**
 * Все возможные варианты строки
 */
const permutStr = (str) => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str]

  return str
    .split('')
    .reduce(
      (a, l, i) =>
        a.concat(permutStr(str.slice(0, i) + str.slice(i + 1)).map((v) => l + v)),
      []
    )
}

permutStr('abc') // ['abc', 'acb', 'bac', 'bca', 'cab', 'cba']

// --- / --- / --- / --- //
/**
 * Преобразование строки в массив
 */
const strToWords = (str, p = /[^a-zа-яё-]+/i) => str.split(p).filter(Boolean)

strToWords('Я люблю JavaScript!') // ['Я', 'люблю', 'JavaScript']
strToWords('JavaScript & TypeScript') // ['JavaScript', 'TypeScript']


// --- / --- / --- / --- //
/**
 * Чувствительная к языку сортировка строки
 */
const sortStr = (str) => [...str].sort((a, b) => a.localeCompare(b)).join('')

sortStr('cabbage') // aabbceg


// --- / --- / --- / --- //
/**
 * Единственное -> множественное число
 * (латиница)
 */
const pluralize = (val, word, plural = word + 's') => {
  const p = (num, word, plural = word + 's') =>
    [1, -1].includes(Number(num)) ? word : plural
  if (typeof val === 'object') return (num, word) => p(num, word, val[word])
  return p(val, word, plural)
}

pluralize(0, 'apple') // 'apples'
pluralize(1, 'apple') // 'apple'
pluralize(2, 'apple') // 'apples'
pluralize(2, 'person', 'people') // 'people'


// --- / --- / --- / --- //
/**
 * Полный хеш строки
 */
const sdbm = (str) =>
  [...str].reduce(
    (hash, cur) =>
      (hash = cur.charCodeAt(0) + (hash << 6) + (hash << 16) - hash),
    0
  )

sdbm('name') // -3521204949


// --- / --- / --- / --- //
/**
 * Перенос строки по количеству указанных символов
 */
const wordWrap = (str, max, br = '\n') =>
  str.replace(
    new RegExp(`(?![^\\n]{1,${max}}$)([^\\n]{1,${max}})\\s`, 'g'),
    '$1' + br
  )

wordWrap(
  'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce tempus.',
  32
)
/*
  Lorem ipsum dolor sit amet,
  consectetur adipiscing elit.
  Fusce tempus.
*/


// --- / --- / --- / --- //
/**
 * Удаление всех лишних пробелов
 */
const noSpace = (str) => str.trim().replace(/\s{2,}/g, ' ')

noSpace(' hello   world  ') // 'hello world'


// --- / --- / --- / --- //
/**
 * Размер строки в байтах
 */
const getByteSize = (str) => new Blob([str]).size

byteSize('😀') // 4
byteSize('Hello World') // 11


// --- / --- / --- / --- //
/**
 * Единичное и максимальное количество вхождений символа в строке
 */
// единичное вхождение
const singleOccur = (str) =>
  [...str].find((c) => str.indexOf(c) === str.lastIndexOf(c))

singleOccur('bbaccc') // a

// символ с максимальным числом вхождений
const maxOccur = (str) => {
  const obj = {}

  for (const char of str) obj[char] = obj[char] + 1 || 1

  return Object.keys(obj).find(
    (c) => obj[c] === Math.max(...Object.values(obj))
  )
}

maxOccur('bbaccc') // c


// --- / --- / --- / --- //
/**
 * Количество гласных букв в слове
 * (латиница и кириллица)
 */
const vowels = (str) => str.match(/[aeiouyауоиэыяюеё]/gi)?.length || 0

vowels('foo bAr привет народ') // 7


// --- / --- / --- / --- //
/**
 * Определение наличия слов в строке
 */
// 1
const ransomNote = (note, magazine) => {
  const hash = {}

  magazine.split(' ').forEach((word) => {
    if (!hash[word]) hash[word] = 0
    hash[word]++
  })

  let possible = true

  note.split(' ').forEach((word) => {
    if (hash[word] && hash[word] > 0) {
      hash[word]--
    } else possible = false
  })

  return possible
}

const words = 'The quick brown fox jumps over the lazy dog'

ransomNote('quick fox', words) // true
ransomNote('big dog', words) // false

// 2
const _ransomNote = (note, magazine) =>
  note
    .split(' ')
    .every((word) =>
      magazine.includes(word) ? (magazine = magazine.replace(word, '')) : false
    )

_ransomNote('lazy dog', words) // true
_ransomNote('red fox', words) // false
```

## <a name="array"></a> Массив

```js
/**
 * Извлечение последнего элемента
 */
// 1
const lastItem = (arr, len = arr.length) => (arr && len ? arr[len - 1] : undefined)

const arr = [1, 2, 3, 4]
lastItem(arr) // 4

// 2
const nthArg = (n) => (...args) => args.slice(n)[0]

const _lastItem = nthArg(-1)
_lastItem(...arr) // 4


// --- / --- / --- / --- //
/**
 * Извлечение i-того элемента
 */
const nthItem = (arr, n = 0) =>
  (n === -1 ? arr.slice(n) : arr.slice(n, n + 1))[0]

nthItem([1, 2, 3], 1) // 2
nthItem(['a', 'b', 'c'], -3) // a


// --- / --- / --- / --- //
/**
 * Извлечение каждого i-того элемента
 */
const getEveryNth = (arr, n) => arr.filter((_, i) => i % n === n - 1)
getEveryNth([1, 2, 3, 4, 5, 6], 2) // [2, 4, 6]


// --- / --- / --- / --- //
/**
 * Создание массива
 */
// 1
const createArr = (n) => Array.from({ length: n }, (_, i) => i)

createArr(5) // [0, 1, 2, 3, 4]

// 2
const _createArr = (n, fn) => Array.from({ length: n }, (_, i) => fn(i))

_createArr(10, () => +Math.random().toFixed(2))
// [0.84, 0.21, 0.43, 0.24, 0.94, 0.2, 0.91, 0.44, 0.38, 0.28]

// дополнительный функционал
// 1
const initArrWithRange = (end, start = 0, step = 1) =>
  Array.from(
    { length: ~~((end - start + 1) / step) },
    (_, i) => i * step + start
  )

initArrWithRange(5) // [0, 1, 2, 3, 4, 5]
initArrWithRange(7, 3) // [3, 4, 5, 6, 7]
initArrWithRange(9, 0, 2) // [0, 2, 4, 6, 8]

// 2
const _initArrWithRange = (min, max, n = 1) =>
  Array.from({ length: n }, () => ~~(Math.random() * (max - min + 1)) + min)

_initArrWithRange(12, 35, 10)
// [ 34, 14, 27, 17, 30, 27, 20, 26, 21, 14 ]


// --- / --- / --- / --- //
/**
 * Получение всех индексов элемента
 */
const allIndexes = (arr, val) =>
  arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), [])

allIndexes([1, 2, 3, 1, 2, 3], 1) // [0, 3]
allIndexes([1, 2, 3], 4) // []


// --- / --- / --- / --- //
/**
 * Сумма двух чисел
 */
const twoSum = (arr, sum) => {
  const pairs = []

  for (const part1 of arr) {
    const part2 = sum - part1

    if (arr.includes(part2)) {
      pairs.push([part1, part2])
      arr.splice(arr.indexOf(part2, 1))
    }
  }

  return pairs
}

twoSum([1, 2, 2, 3, 4], 4) // [ [1, 3], [2, 2] ]


// --- / --- / --- / --- //
/**
 * Все возможные варианты массива
 */
const permutArr = (arr) => {
  if (arr.length <= 2) return arr.length === 2 ? [arr, [arr[1], arr[0]]] : arr
  return arr.reduce(
    (acc, cur, i) =>
      acc.concat(
        permutArr([...arr.slice(0, i), ...arr.slice(i + 1)]).map((v) => [
          cur,
          ...v
        ])
      ),
    []
  )
}

permutArr([1, 2, 3])
// [ [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1] ]


// --- / --- / --- / --- //
/**
 * Определение самого длинного элемента массива
 */
const longestItem = (...args) =>
  args.reduce((a, b) => (b.length > a.length ? b : a))

longestItem('a', 'abc', 'ab') // abc
longestItem(...['abc', 'a', 'ab'], 'abcd') // abcd
longestItem([1, 2, 3], [1, 2], [1, 2, 3, 4]) // [1, 2, 3, 4]


// --- / --- / --- / --- //
/**
 * Сравнение массивов
 */
const areEqual = (a, b) => {
  if (a.length !== b.length) return false
  const _a = a.sort(),
    _b = b.sort()
  return _a.every((v, i) => v === _b[i])
}

const x = [1, 3, 5, 7, 9]
const y = [5, 3, 1, 9, 7]

areEqual(x, y) // true


// --- / --- / --- / --- //
/**
 * Является ли второй массив подмассивом (подмножеством) первого?
 */
const isSubset = (x, y) => {
  const _x = new Set(x),
    _y = new Set(y)
  return [..._x].every((i) => _y.has(i))
}

isSubset([1, 2, 3, 4], [2, 4]) // true


// --- / --- / --- / --- //
/**
 * Можно ли объединить два массива без дубликатов?
 */
const isJoin = (a, b) => {
  const _a = new Set(a),
    _b = new Set(b)
  return [..._a].every((i) => !_b.has(i))
}

isJoin([1, 2], [3, 4]) // true
isJoin([1, 2], [1, 3]) // false


// --- / --- / --- / --- //
/**
 * Сдвиг на указанное количество элементов
 */
const offset = (arr, off) => [...arr.slice(off), ...arr.slice(0, off)]

offset([1, 2, 3, 4, 5], 2) // [3, 4, 5, 1, 2]
offset([1, 2, 3, 4, 5], -2) // [4, 5, 1, 2, 3]


// --- / --- / --- / --- //
/**
 * Удаление элементов массива,
 * удовлетворяющих условию - функции
 */
const rejectItems = (pred, arr) => arr.filter((...args) => !pred(...args))

rejectItems((n) => n % 2 === 0, [1, 2, 3, 4, 5]) // [1, 3, 5]

rejectItems((s) => s.length > 4, ['Apple', 'Pear', 'Kiwi', 'Banana'])
// ['Pear', 'Kiwi']


// --- / --- / --- / --- //
/**
 * Перемешивание элементов массива -
 * тасование Фишера — Йетса
 * https://ru.wikipedia.org/wiki/%D0%A2%D0%B0%D1%81%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%A4%D0%B8%D1%88%D0%B5%D1%80%D0%B0_%E2%80%94_%D0%99%D0%B5%D1%82%D1%81%D0%B0
 */
const shuffleArr = ([...arr]) => {
  let l = arr.length
  while (l) {
    const i = ~~(Math.random() * l--)
    ;[(arr[l], arr[i])] = [arr[i], arr[l]]
  }
  return arr
}

const foo = [1, 2, 3]
shuffleArr(foo) // -> [2, 3, 1] foo -> [1, 2, 3]


// --- / --- / --- / --- //
/**
 * Определение процента элементов массива,
 * удовлетворяющих условию
 */
const percent = (arr, val) =>
  (100 *
    arr.reduce((a, v) => a + (v < val ? 1 : 0) + (v === val ? 0.5 : 0), 0)) /
  arr.length

percent([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 6) // 55


// --- / --- / --- / --- //
/**
 * Определение количества вхождений элемента в массиве
 */
const countOccur = (arr, val) =>
  arr.reduce((a, v) => (v === val ? a + 1 : a), 0)

countOccur([1, 1, 2, 1, 2, 3], 1) // 3


// --- / --- / --- / --- //
/**
 * Определение одинаковых элементов двух массивов
 */
const identicalItems = (a, b) => a.filter((i) => b.includes(i))

identicalItems([1, 2, 3], [1, 2, 4]) // [1, 2]


// --- / --- / --- / --- //
/**
 * Удаление указанных элементов
 */
const withoutItems = (arr, ...args) => arr.filter((i) => !args.includes(i))

withoutItems([2, 1, 3], 1, 2) // [3]


// --- / --- / --- / --- //
/**
 * Стабильная сортировка
 */
const stableSort = (arr, compare) =>
  arr
    .map((i, _i) => ({ i, _i }))
    .sort((a, b) => compare(a.i, b.i) || a._i - b._i)
    .map(({ i }) => i)

stableSort([2, 10, 20, 1]) // 1, 2, 10, 20


// --- / --- / --- / --- //
/**
 * Инверсия элементов массива без `reverse`
 */
// 1
const reverse = (arr) => {
  for (let i = 0; i < array.length / 2; i++) {
    ;[arr[i], arr[arr.length - 1 - i]] = [arr[arr.length - 1 - i], arr[i]]
  }
  return arr
}

reverse([1, 2, 3]) // [3, 2, 1]

// 2
const _reverse = (arr) => {
  const newArr = []
  while (arr.length) {
    newArr.push(...arr.splice(arr.length - 1))
  }
  return newArr
}


// --- / --- / --- / --- //
/**
 * Фильтрация объектов в массиве
 */
const reducedFilter = (arr, keys, fn) =>
  arr.filter(fn).map((el) =>
    keys.reduce((acc, key) => {
      acc[key] = el[key]
      return acc
    }, {})
  )

const data = [
  {
    id: 1,
    name: 'John',
    age: 23
  },
  {
    id: 2,
    name: 'Jane',
    age: 32
  }
]

reducedFilter(data, ['id', 'name'], (i) => i.age > 23)
// [ { id: 2, name: 'Jane'} ]


// --- / --- / --- / --- //
/**
 * Определение самого часто встречающегося элемента в массиве
 */
const mostFreq = (arr) =>
  Object.entries(
    arr.reduce((a, v) => {
      a[v] = a[v] ? a[v] + 1 : 1
      return a
    }, {})
  ).reduce((a, v) => (v[1] >= a[1] ? v : a), [null, 0])[0]

mostFreq(['a', 'b', 'c', 'b', 'b', 'a']) // b


// --- / --- / --- / --- //
/**
 * Определение количества вхождений каждого элемента в массиве
 */
const howFreq = (arr) =>
  arr.reduce((a, k) => {
    a[k] = a[k] ? a[k] + 1 : 1
    return a
  }, {})

howFreq(['a', 'b', 'c', 'b', 'b', 'a']) // { a: 2, b: 3, c: 1 }


// --- / --- / --- / --- //
/**
 * Удаление элементов по условию - функции
 * (возвращается массив удаленных элементов)
 */
const remove = (arr, fn) =>
  Array.isArray(arr)
    ? arr.filter(fn).reduce((a, v) => {
        arr.splice(arr.indexOf(v), 1)
        return a.concat(v)
      }, [])
    : []

remove([1, 2, 3, 4], (n) => n % 2 === 0) // [2, 4]


// --- / --- / --- / --- //
/**
 * Цикличный генератор
 */
const cycleGen = function* (arr) {
  let i = 0
  while (true) {
    yield arr[i % arr.length]
    i++
  }
}

const binaryCycle = cycleGenerator([0, 1])

binaryCycle.next() // { value: 0, done: false }
binaryCycle.next() // { value: 1, done: false }
binaryCycle.next() // { value: 0, done: false }
binaryCycle.next() // { value: 1, done: false }


// --- / --- / --- / --- //
/**
 * Добавление элементов по указанному индексу
 * (модицикация `splice`)
 */
const insertAt = (arr, i, ...args) => {
  arr.splice(i + 1, 0, ...args)
  return arr
}

insertAt([1, 2, 3], 1, 4, 5) // [1, 2, 4, 5, 3]


// --- / --- / --- / --- //
/**
 * Удаление всех указанных элементов
 */
const pull = (arr, ...args) => {
  const state = Array.isArray(args[0]) ? args[0] : args
  const pulled = arr.filter((i) => !state.includes(i))
  arr.length = 0
  pulled.forEach((i) => arr.push(i))
  return arr
}

const arr = ['a', 'b', 'c', 'a', 'b', 'c']

pull(arr, 'a', 'c') // [ 'b', 'b' ]


// --- / --- / --- / --- //
/**
 * Удаление элементов по индексам
 */
const pullAtIndex = (arr, arrToPull) => {
  const removed = []
  const pulled = arr
    .map((item, index) =>
      arrToPull.includes(index) ? removed.push(item) : item
    )
    .filter((_, index) => !arrToPull.includes(index))
  arr.length = 0
  pulled.forEach((item) => arr.push(item))
  return arr
}

const arr = ['a', 'b', 'c', 'd']

pullAtIndex(arr, [1, 3])
// arr -> [ 'a', 'c' ] removed -> [ 'b', 'd' ]


// --- / --- / --- / --- //
/**
 * Среднее значение объектов массива
 */
const averageBy = (arr, fn) =>
  arr
    .map(typeof fn === 'function' ? fn : (v) => v[fn])
    .reduce((a, c) => a + c, 0) / arr.length

averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], (x) => x.n) // 5
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n') // 5


// --- / --- / --- / --- //
/**
 * Сжатие (объединение) массивов
 */
const zipArr = (...arrs) => {
  const max = Math.max(...arrs.map((i) => i.length))
  return Array.from({ length: max }).map((_, i) =>
    Array.from({ length: arrs.length }, (_, k) => arrs[k][i])
  )
}

zipArr(['a', 'b'], [1, 2], [true, false])
// [ ['a', 1, true], ['b', 2, false] ]


// --- / --- / --- / --- //
/**
 * Определение наибольшего и наименьшего элементов
 * без `Math.max` и `Math.min`
 */
// наибольший элемент
// 1
const max = (arr) => {
  let len = arr.length
  let max = -Infinity
  while (len--) {
    if (arr[len] > max) {
      max = arr[len]
    }
  }
  return max
}

max([3, 2, 4]) // 4

// 2
const _max = (arr, n = 1) => [...arr].sort((x, y) => y - x).slice(0, n)

_max([1, 2, 3]) // [3]
_max([1, 2, 3], 2) // [3, 2]

// наименьший элемент
// 1
const min = (arr) => {
  let len = arr.length
  let min = Infinity

  while (len--) {
    if (arr[len] < min) {
      min = arr[len]
    }
  }

  return min
}

min([3, 2, 4]) // 2

// 2
const _min = (arr, n = 1) => arr.sort((x, y) => x - y).slice(0, n)

_min([1, 2, 3]) // [1]
_min([1, 2, 3], 2) // [1, 2]


// --- / --- / --- / --- //
/**
 * Определение наибольшего и наименьшего значений объектов в массиве
 */
// наибольшее значение
const maxBy = (arr, fn) =>
  Math.max(...arr.map(typeof fn === 'function' ? fn : (val) => val[fn]))

maxBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n') // 8

// наименьшее значение
const minBy = (arr, fn) =>
  Math.min(...arr.map(typeof fn === 'function' ? fn : (val) => val[fn]))

minBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n') // 2


// --- / --- / --- / --- //
/**
 * Сортировка и объединение массивов
 */
const mergeSortArrs = (a, b) => {
  const _a = [...a],
    _b = [...b]

  return Array.from({ length: _a.length + _b.length }, () => {
    if (!_a.length) return _b.shift()
    else if (!_b.length) return _a.shift()
    else return _a[0] > _b[0] ? _b.shift() : _a.shift()
  })
}

mergeSortArrs([1, 3, 5], [2, 4, 6]) // [1, 2, 3, 4, 5, 6]


// --- / --- / --- / --- //
/**
 * Сортировка и объединение массивов объектов
 * по условию (ключу объекта)
 */
const combine = (a, b, prop) =>
  Object.values(
    [...a, ...b].reduce((acc, val) => {
      if (val[prop])
        acc[val[prop]] = acc[val[prop]]
          ? { ...acc[val[prop]], ...val }
          : { ...val }
      return acc
    }, {})
  )

const x = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' }
]
const y = [{ id: 1, age: 28 }, { id: 3, age: 26 }, { age: 24 }]

combine(x, y, 'id')
/*
[
  { id: 1, name: 'John', age: 28 },
  { id: 2, name: 'Jane' },
  { id: 3, age: 26 }
]
*/


// --- / --- / --- / --- //
/**
 * Распаковка массива
 * (рекурсивная реализация `flat`)
 */
const flatBy = (arr, dep = 1) =>
  arr.reduce(
    (a, v) => a.concat(dep > 1 && Array.isArray(v) ? flatBy(v, dep - 1) : v),
    []
  )

flatBy([1, [2], 3, 4]) // [1, 2, 3, 4]
flatBy([1, [2, [3, [4, 5], 6], 7], 8], 2) // [1, 2, 3, [4, 5], 6, 7, 8]


// --- / --- / --- / --- //
/**
 * Проверка уникальности элементов
 * с помощью условия - функции
 */
const uniqueBy = (arr, fn) => arr.length === new Set(arr.map(fn)).size

uniqueBy([1.2, 2.4, 2.9], Math.round) // true
uniqueBy([1.2, 2.3, 2.4], Math.round) // false


// --- / --- / --- / --- //
/**
 * Выборка элементов
 */
// дублирующихся
const filterUnique = (arr) =>
  [...new Set(arr)].filter((i) => arr.indexOf(i) !== arr.lastIndexOf(i))

filterUnique([1, 2, 2, 3, 4, 4, 5]) // [2, 4]

// уникальных
const filterNonUnique = (arr) =>
  [...new Set(arr)].filter((i) => arr.indexOf(i) === arr.lastIndexOf(i))

filterNonUnique([1, 2, 2, 3, 4, 4, 5]) // [1, 3, 5]


// --- / --- / --- / --- //
/**
 * Выборка объектов в массиве по условию - функции
 */
// уникальных
const filterNonUniqueBy = (arr, fn) =>
  arr.filter((v, i) => arr.every((x, j) => (i === j) === fn(v, x, i, j)))

filterNonUniqueBy(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 1, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id === b.id
) // [ { id: 2, value: 'c' } ]

// дублирующихся
const filterUniqueBy = (arr, fn) =>
  arr.filter((v, i) => arr.some((x, j) => (i !== j) === fn(v, x, i, j)))

filterUniqueBy(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 3, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id === b.id
)
// [ { id: 0, value: 'a' }, { id: 0, value: 'e' } ]


// --- / --- / --- / --- //
/**
 * Разделение массива на части
 */
// n - количество элементов каждой части
const chunk = (arr, n) => (arr.length > n ? [arr, arr.splice(n)] : arr)

chunk([1, 2, 3, 4, 5, 6, 7], 4) // [ [1, 2, 3, 4], [5, 6, 7] ]

// n - количество частей, на которые делится массив
const _chunk = (arr, n) => {
  const size = ~~(arr.length / n)
  return Array.from({ length: n }, (_, i) =>
    arr.slice(i * size, i * size + size)
  )
}

_chunk([1, 2, 3, 4, 5, 6, 7], 4) // [ [1, 2], [3, 4], [5, 6], [7] ]


// --- / --- / --- / --- //
/**
 * Выборка элементов
 */
// не удовлетворяющих условию - функции
const takeUntil = (arr, fn) => {
  for (const [i, v] of arr.entries()) if (fn(v)) return arr.slice(0, i)
  return arr
}

takeUntil([1, 2, 3, 4], (n) => n >= 3) // [1, 2]

// удовлетворяющих условию - функции
const dropWhile = (arr, fn) => {
  while (arr.length > 0 && !fn(arr[0])) arr = arr.slice(1)
  return arr
}

dropWhile([1, 2, 3, 4], (n) => n >= 3) // [3, 4]


// --- / --- / --- / --- //
/**
 * Суммирование значений объектов в массиве
 */
const sumBy = (arr, fn) =>
  arr.map(typeof fn === 'function')
    ? fn
    : (val) => val[fn].reduce((acc, val) => acc + val, 0)

sumBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], (x) => x.n) // 20
sumBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n') // 20


// --- / --- / --- / --- //
/**
 * Объединение массивов по условию - функции
 * (возвращается новый объединенный массив)
 */
const unionBy = (a, b, fn) => {
  const s = new Set(a.map(fn))
  return Array.from(new Set([...a, ...b.filter((x) => !s.has(fn(x)))]))
}

unionBy([2.1], [1.2, 2.3], Math.floor) // [2.1, 1.2]
unionBy([{ id: 1 }, { id: 2 }], [{ id: 2 }, { id: 3 }], (x) => x.id)
// [{ id: 1 }, { id: 2 }, { id: 3 }]


// --- / --- / --- / --- //
/**
 * Группировка элементов по условию - функции
 * (критерий: количество элементов)
 */
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : (v) => v[fn]).reduce((a, v) => {
    a[v] = (a[v] || 0) + 1
    return a
  }, {})

countBy([6.1, 4.2, 6.3], Math.floor) // { 4: 1, 6: 2 }
countBy(['one', 'two', 'three'], 'length') // { 3: 2, 5: 1 }


// --- / --- / --- / --- //
/**
 * Группировка элементов по условию - функции
 * (критерий: массив элементов)
 */
const groupBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : (v) => v[fn]).reduce((a, v, i) => {
    a[v] = (a[v] || []).concat(arr[i])
    return a
  }, {})

groupBy([6.1, 4.2, 6.3], Math.floor) // {4: [4.2], 6: [6.1, 6.3]}
groupBy(['one', 'two', 'three'], 'length') // {3: ['one', 'two'], 5: ['three']}


// --- / --- / --- / --- //
/**
 * Фильтрация массива по условию - функции
 * ([ [элементы, удовлетворяющие условию], [прочие элементы] ])
 */
const bifurcateBy = (arr, fn) =>
  arr.reduce((acc, val, i) => (acc[fn(val, i) ? 0 : 1].push(val), acc), [
    [],
    []
  ])

bifurcateBy(['foo', 'bar', 'baz'], (x) => x[0] === 'b')
// [ ['bar', 'baz'], ['foo'] ]


// --- / --- / --- / --- //
/**
 * Определение разницы между массивами по условию - функции
 * (возвращается массив отличающихся элементов)
 */
const diffBy = (x, y, fn) => {
  const _x = new Set(x.map((i) => fn(i))),
    _y = new Set(y.map((i) => fn(i)))
  return [
    ...x.filter((i) => !_y.has(fn(i))),
    ...y.filter((i) => !_x.has(fn(i)))
  ]
}

diffBy([2.1, 1.2], [2.3, 3.4], Math.floor) // [1.2, 3.4]

diffBy(
  [{ id: 1 }, { id: 2 }, { id: 3 }],
  [{ id: 1 }, { id: 2 }, { id: 4 }],
  (i) => i.id
) // [ { id: 3 }, { id: 4 } ]


// --- / --- / --- / --- //
/**
 * Сортировка объектов в массиве по условиям -
 * массиву значений (ключам объекта)
 */
const orderBy = (arr, props, orders) =>
  [...arr].sort((x, y) =>
    props.reduce((a, p, i) => {
      if (a === 0) {
        const [p1, p2] =
          orders && orders[i] === 'desc' ? [y[p], x[p]] : [x[p], y[p]]
        a = p1 > p2 ? 1 : p1 < p2 ? -1 : 0
      }
      return a
    }, 0)
  )

const users = [
  { name: 'John', age: 23 },
  { name: 'Jane', age: 22 },
  { name: 'Alice', age: 24 },
  { name: 'Bob', age: 21 }
]

orderBy(users, ['name', 'age'])
/*
[
  {name: "Alice", age: 24},
  {name: "Bob", age: 21},
  {name: "Jane", age: 22},
  {name: "John", age: 23}
]
*/


// --- / --- / --- / --- //
/**
 * Сортировка объектов в массиве по условию (ключу объекта)
 * и порядку - массиву значений (значениям объектов)
 */
const orderWith = (arr, prop, order) => {
  const ordered = order.reduce((a, c, i) => {
    a[c] = i
    return a
  }, {})
  return [...arr].sort((a, b) => {
    if (ordered[a[prop]] === undefined) return 1
    if (ordered[b[prop]] === undefined) return -1
    return ordered[a[prop]] - ordered[b[prop]]
  })
}

const users = [
  { name: 'john', language: 'Javascript' },
  { name: 'jane', language: 'Typescript' },
  { name: 'alice', language: 'Javascript' },
  { name: 'bob', language: 'Java' },
  { name: 'igor' },
  { name: 'harry', language: 'Python' }
]

orderWith(users, 'language', ['Javascript', 'Typescript', 'Java'])
/*
[
  { name: 'john', language: 'Javascript' },
  { name: 'alice', language: 'Javascript' },
  { name: 'jane', language: 'Typescript' },
  { name: 'bob', language: 'Java' },
  { name: 'igor' },
  { name: 'harry', language: 'Python' }
]
*/
```

## <a name="object"></a> Объект

```js
/**
 * Является ли объект пустым?
 */
const isEmpty = (val) => val === null || !(Object.keys(val) || val).length

isEmpty('') // true
isEmpty([]) // true
isEmpty(['']) // false
isEmpty({ a: 1 }) // false


// --- / --- / --- / --- //
/**
 * Определение размера итерируемого объекта
 */
const getIterableSize = (val) =>
  Array.isArray(val)
    ? val.length
    : val && typeof val === 'object'
    ? val.size || val.length || Object.keys(val).length
    : typeof val === 'string'
    ? new Blob([val]).size
    : 0

getIterableSize([1, 2, 3]) // 3
getIterableSize('size') // 4
getIterableSize({ one: 1, two: 2, three: 3 }) // 3


// --- / --- / --- / --- //
/**
 * Извлечение значений по селекторам
 */
const getObjVals = (from, ...selectors) =>
  [...selectors].map((s) =>
    s
      .replace(/\[([^\[\]]*)\]/g, '.$1.')
      .split('.')
      .filter((t) => t !== '')
      .reduce((prev, cur) => prev && prev[cur], from)
  )

const obj = {
  selector: { to: { val: 'val to select' } },
  target: [1, 2, { a: 'test' }]
}

getObjVals(obj, 'selector.to.val', 'target[0]', 'target[2].a') // ["val to select", 1, "test"]


// --- / --- / --- / --- //
/**
 * Клонирование объекта (поверхностное или глубокое)
 */
const copyObj = (obj, shallow = true) =>
  shallow ? { ...obj } : JSON.parse(JSON.stringify(obj))

const obj = {
  foo: 'bar',
  baz: {
    qux: {
      a: 'b'
    }
  }
}

const _obj = copyObj(obj)
_obj.baz.qux.a = 'c'

obj.baz.qux.a // c

const __obj = copyObj(_obj, false)
__obj.baz.qux.a = 'd'

_obj.baz.qux.a // c


// --- / --- / --- / --- //
/**
 * Рекурсивное глубокое копирование объекта (массива)
 */
const deepClone = (obj) => {
  if (obj === null) return null
  const clone = Object.assign({}, obj)
  Object.keys(clone).forEach(
    (key) =>
      (clone[key] =
        typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key])
  )
  if (Array.isArray(obj)) {
    clone.length = obj.length
    return Array.from(clone)
  }
  return clone
}

const a = { foo: 'bar', obj: { a: 1, b: 2 } }
const b = deepClone(a) // a !== b, a.obj !== b.obj


// --- / --- / --- / --- //
/**
 * Удаление всех ложных значений
 */
const compactObj = (v) => {
  const d = Array.isArray(v) ? v.filter(Boolean) : v
  return Object.keys(d).reduce(
    (a, k) => {
      const v = d[k]
      if (Boolean(v)) a[k] = typeof v === 'object' ? compactObject(v) : v
      return a
    },
    Array.isArray(v) ? [] : {}
  )
}

const obj = {
  a: null,
  b: false,
  c: true,
  d: 0,
  e: 1,
  f: '',
  g: 'a',
  h: [null, false, '', true, 1, 'a'],
  i: { j: 0, k: false, l: 'a' }
}

compactObj(obj)
// { c: true, e: 1, g: 'a', h: [ true, 1, 'a' ], i: { l: 'a' } }


// --- / --- / --- / --- //
/**
 * Упаковка объекта
 */
const flatObj = (obj, prefix = '') =>
  Object.keys(obj).reduce((a, k) => {
    const pre = prefix.length ? `${prefix}.` : ''
    if (
      typeof obj[k] === 'object' &&
      obj[k] !== null &&
      Object.keys(obj[k]).length > 0
    )
      Object.assign(a, flatObj(obj[k], pre + k))
    else a[pre + k] = obj[k]
    return a
  }, {})

flatObj({ a: { b: { c: 1 } }, d: 2 }) // { a.b.c: 1, d: 2 }


// --- / --- / --- / --- //
/**
 * Распаковка объекта
 */
const unflatObj = (obj) =>
  Object.keys(obj).reduce((acc, key) => {
    key
      .split('.')
      .reduce(
        (a, e, i, keys) =>
          a[e] ||
          (a[e] = isNaN(Number(keys[i + 1]))
            ? keys.length - 1 === i
              ? obj[key]
              : {}
            : []),
        acc
      )
    return acc
  }, {})

unflatObj({ 'a.b.c': 1, d: 2 }) // { a: { b: { c: 1 } }, d: 2 }
unflatObj({ 'a.b': 1, 'a.c': 2, d: 3 }) // { a: { b: 1, c: 2 }, d: 3 }


// --- / --- / --- / --- //
/**
 * Удаление свойств по условию - массиву ключей
 */
const omit = (obj, arr) =>
  Object.keys(obj)
    .filter((v) => !arr.includes(v))
    .reduce((a, k) => ((a[k] = obj[k]), a), {})

omit({ a: 1, b: '2', c: 3 }, ['b']) // { a: 1, c: 3 }


// --- / --- / --- / --- //
/**
 * Извлечение свойств по условию - массиву ключей
 */
const pick = (obj, arr) =>
  arr.reduce((a, c) => (c in obj && (a[c] = obj[c]), a), {})

pick({ a: 1, b: '2', c: 3 }, ['a', 'c']) // { a: 1, c: 3 }


// --- / --- / --- / --- //
/**
 * Извлечение свойств по условию - функции
 */
const pickBy = (obj, fn) =>
  Object.keys(obj)
    .filter((k) => !fn(obj[k], k))
    .reduce((a, v) => ((a[v] = obj[v]), a), {})

pickBy({ a: 1, b: '2', c: 3 }, (x) => typeof x === 'number')
// { b: '2' }


// --- / --- / --- / --- //
/**
 * Удаление свойств по условию - функции
 */
const omitBy = (obj, fn) =>
  Object.keys(obj)
    .filter((key) => fn(obj[key], key))
    .reduce((a, v) => ((a[v] = obj[v]), a), {})

omitBy({ a: 1, b: '2', c: 3 }, (x) => typeof x === 'number')
// { a: 1, c: 3 }


// --- / --- / --- / --- //
/**
 * Группировка свойств по условию - функции
 * ({ значение: [ свойства ]  })
 */
const invertKeyVal = (obj, fn) =>
  Object.keys(obj).reduce((acc, key) => {
    const val = fn ? fn(obj[key]) : obj[key]
    acc[val] = acc[val] || []
    acc[val].push(key)
    return acc
  }, {})

invertKeyVal({ a: 1, b: 2, c: 1 }) // { 1: [ 'a', 'c' ], 2: [ 'b' ] }
invertKeyVal({ a: 1, b: 2, c: 1 }, (v) => '#' + v)
// { #1: [ 'a', 'c' ], #2: [ 'b' ] }


// --- / --- / --- / --- //
/**
 * Извлечение глубоко вложенного свойства
 */
const getDeepKey = (obj, key) =>
  key in obj
    ? obj[key]
    : Object.values(obj).reduce((acc, val) => {
        if (acc !== undefined) return acc
        if (typeof val === 'object') return getDeepKey(val, key)
      }, undefined)

const obj = {
  foo: {
    bar: {
      baz: 'qux'
    }
  }
}

getDeepKey(obj, 'baz') // qux
getDeepKey(obj, 'qux') // undefined


// --- / --- / --- / --- //
/**
 * Извлечение ключей по значению
 */
const findKeys = (obj, val) =>
  Object.keys(obj).filter((key) => obj[key] === val)

const ages = {
  John: 20,
  Jane: 22,
  Alice: 20
}

findKeys(ages, 20) // ['John', 'Alice']


// --- / --- / --- / --- //
/**
 * Извлечение свойства по условию - функции
 */
const findKey = (obj, fn) =>
  Object.keys(obj).find((key) => fn(obj[key], key, obj))

findKey(
  {
    john: { age: 23, active: true },
    jane: { age: 24, active: false },
    alice: { age: 25, active: true }
  },
  (x) => x['active']
)
// john


// --- / --- / --- / --- //
/**
 * Объединение объектов
 */
const mergeObj = (...objs) =>
  [...objs].reduce(
    (acc, obj) =>
      Object.keys(obj).reduce((a, k) => {
        a[k] = a.hasOwnProperty(k) ? [].concat(a[k]).concat(obj[k]) : obj[k]
        return acc
      }, {}),
    {}
  )

const x = {
  a: [{ x: 2 }, { y: 4 }],
  b: 1
}

const y = {
  a: { z: 3 },
  b: [2, 3],
  c: 'foo'
}

mergeObj(x, y)
// { a: [ { x: 2 }, { y: 4 }, { z: 3 } ], b: [ 1, 2, 3 ], c: 'foo' }


// --- / --- / --- / --- //
/**
 * Преобразование объекта в строку запроса
 */
const objToQueryStr = (params) =>
  params
    ? Object.entries(params).reduce((str, [k, v]) => {
        const s = str.length === 0 ? '?' : '&'
        str += typeof v === 'string' ? `${s}${k}=${v}` : ''
        return str
      }, '')
    : ''

objToQueryStr({ page: '1', size: '2kg', key: undefined }) // ?page=1&size=2kg


// --- / --- / --- / --- //
/**
 * Хеширование объекта
 */
const toHash = (obj, key) =>
  Array.prototype.reduce.call(
    obj,
    (acc, v, i) => ((acc[!key ? i : v[key]] = v), acc),
    {}
  )

const users = [
  { id: '1', name: 'John' },
  { id: '2', name: 'Jane' },
  { id: '3', name: 'Alice' },
  { id: '4', name: 'Bob' }
]
const managers = [{ manager: 1, employees: [2, 3, 4] }]

managers.forEach(
  (m) =>
    (m.employees = m.employees.map(function (id) {
      return this[id]
    }, toHash(users, 'id')))
)

managers
// [ {manager: 1, employees: [ {id: 2, name: 'Jane'}, {id: 3, name: 'Alice'}, {id: 4, name: 'Bob'} ] } ]


// --- / --- / --- / --- //
/**
 * "Привязка" методов к объекту (определение контекста методов)
 */
const bindAll = (obj, ...fns) =>
  fns.forEach((fn) => ((f = obj[fn]), (obj[fn] = () => f.apply(obj))))

const view = {
  label: '!',
  click: function () {
    console.log('clicked' + this.label)
  }
}

bindAll(view, 'click')

window.addEventListener('click', view.click)


// --- / --- / --- / --- //
/**
 * Глубокая "заморозка" объекта (объект становится неизменяемым - иммутабельным)
 */
const deepFreeze = (obj) => {
  Object.keys(obj).forEach((key) => {
    if (typeof obj[key] === 'object') deepFreeze(obj[key])
  })
  return Object.freeze(obj)
}


// --- / --- / --- / --- //
/**
 * Сжатие (объединение) объектов
 */
const zipObj = (props, vals) =>
  props.reduce((obj, prop, i) => ((obj[prop] = vals[i]), obj), {})

zipObj(['a', 'b', 'c'], [1, 2]) // { a: 1, b: 2, c: undefined }
zipObj(['a', 'b'], [1, 2, 3]) // { a: 1, b: 2 }


// --- / --- / --- / --- //
/**
 * Сравнение объектов
 */
const areEqual = (a, b) => {
  if (a === b) return true
  if (a instanceof Date && b instanceof Date) return a.getTime() === b.getTime()
  if (!a || !b || (typeof a !== 'object' && typeof b !== 'object'))
    return a === b
  if (a.prototype !== b.prototype) return false
  const keys = Object.keys(a)
  if (keys.length !== Object.keys(b).length) return false
  return keys.every((k) => areEqual(a[k], b[k]))
}

areEqual(
  { a: [2, { e: 3 }], b: [4], c: 'foo' },
  { a: [2, { e: 3 }], b: [4], c: 'foo' }
) // true
areEqual([1, 2, 3], { 0: 1, 1: 2, 2: 3 }) // true


// --- / --- / --- / --- //
/**
 * Содержит ли первый объект свойства второго?
 */
const matches = (obj, source) =>
  Object.keys(source).every(
    (key) => obj.hasOwnProperty(key) && obj[key] === source[key]
  )

matches(
  {
    age: 25,
    hair: 'long',
    beard: true
  },
  {
    hair: 'long',
    beard: true
  }
) // true
matches(
  {
    hair: 'long',
    beard: true
  },
  {
    age: 25,
    hair: 'long',
    beard: true
  }
) // false


// --- / --- / --- / --- //
/**
 * Поиск совпадения по условию - функции
 */
const matchesWith = (obj, source, fn) =>
  Object.keys(source).every((key) =>
    obj.hasOwnProperty(key) && fn
      ? fn(obj[key], source[key], key, obj, source)
      : obj[key] === source[key]
  )

const isGreet = (val) => /^h(?:i|ello)$/.test(val)
matchesWith(
  { greet: 'hello' },
  { greet: 'hi' },
  (a, b) => isGreet(a) && isGreet(b)
) // true


// --- / --- / --- / --- //
/**
 * Обеспечение возможности создания глубоко вложенных свойств
 * без промежуточных уровней
 */
const deepProp = (obj) =>
  new Proxy(obj, {
    get: (target, prop) => {
      if (!(prop in target)) target[prop] = new ProxyObj({})
      return target[prop]
    }
  })

const obj = new deepProp({})

obj.x.y.z = 'foo'

console.log(obj.x.y.z) // foo


// --- / --- / --- / --- //
/**
 * Создание итерируемого объекта
 */
// 1
const makeIterable = (obj) => {
  Object.defineProperty(obj, 'length', {
    value: Object.keys(obj).length
  })

  obj[Symbol.iterator] = (i = 0, values = Object.values(obj)) => ({
    next: () =>
      i < obj.length ? { done: false, value: values[i++] } : { done: true }
  })

  return obj
}

const iterableObj = makeIterable({
  name: 'John',
  age: 24
})

for (const v of iterableObj) console.log(v) // John 24
console.log(...iterableObj) // John 24

// 2
const _makeIterable = (obj) => {
  Object.defineProperties(obj, {
    length: {
      value: Object.keys(obj).length
    },

    [Symbol.iterator]: {
      value: function* () {
        for (const i in this) {
          yield this[i]
        }
      }
    }
  })

  return obj
}

const _iterableObj = _makeIterable({
  name: 'Jane',
  age: 23
})

for (const v of iterableObj) console.log(v) // Jane 23
console.log(...iterableObj) // Jane 23


// --- / --- / --- / --- //
/**
 * Создание объекта, доступного только для чтения
 */
const makeReadOnlyObj = (obj) =>
  new Proxy(obj, {
    get: (target, key) => target[key],
    set: (target, key, value) => {
      if (target.hasOwnProperty(key)) {
        throw new Error('error')
      }
      target[key] = value
    }
  })

const person = makeReadOnlyObj({ name: 'John', age: 30 })

console.log(person.name) // John
person.sex = 'm'
person.age = 20 // error
```

## <a name="function"></a> Функция

```js
/**
 * Обеспечение задержки между вызовами функций
 */
const sleep = (ms) => new Promise((r) => setTimeout(r, ms))
const log = console.log

const printNums = async () => {
  log(1)
  await sleep(1000)
  log(2)
  await sleep(1000)
  log(3)
}

printNums() // 1 2 3 с задержкой в 1 секунду между каждым `log`


// --- / --- / --- / --- //
/**
 * Измерение времени выполнения функции
 */
const timeTaken = (fn) => {
  console.time('t')
  const r = fn()
  console.timeEnd('t')
  return r
}


// --- / --- / --- / --- //
/**
 * Выполнение функции только при удовлетворении условия
 */
const makeWhen = (make, when) => (x) => (make(x) ? when(x) : x)

const doubleEvenNums = when(
  (n) => !(n & 1),
  (n) => n * 2
)

doubleEvenNums(2) // 4


// --- / --- / --- / --- //
/**
 * Определение типа переданного значения
 */
// 1
const getType = (v) =>
  v === 'undefined' ? 'undefined' : v === null ? 'null' : v.constructor.name

getType([]) // Array
getType(() => {}) // Function

// 2
const _getType = (value) => Object.prototype.toString.call(value).slice(8, -1)

_getType({}) // Object
_getType(function () {}) // Function


// --- / --- / --- / --- //
/**
 * Асинхронное выполнение функции с помощью веб-воркера (web worker)
 */
const runAsync = (fn) => {
  const worker = new Worker(
    URL.createObjectURL(new Blob([`postMessage((${fn})())`]), {
      type: 'application/javascript; charset=utf-8'
    })
  )
  return new Promise((res, rej) => {
    worker.onmessage = ({ data }) => {
      res(data), worker.terminate()
    }
    worker.onerror = (er) => {
      rej(er), worker.terminate()
    }
  })
}

const longRunFn = () => {
  let res = 0
  for (let i = 0; i < 1000; i++)
    for (let j = 0; j < 700; j++)
      for (let k = 0; k < 300; k++) res = res + i + j + k

  return res
}

runAsync(longRunFn).then(log) // 209685000000


// --- / --- / --- / --- //
/**
 * Последовательное выполнение промисов
 */
const runPromiseSeq = (ps) =>
  ps.reduce((p, next) => p.then(next), Promise.resolve())

const delay = (d) => new Promise((r) => setTimeout(r, d))

runPromiseSeq([() => delay(1000), () => delay(2000)])


// --- / --- / --- / --- //
/**
 * Определение самой производительной функции
 */
const mostPerform = (fns, iter = 10000) => {
  const times = fns.map((fn) => {
    const before = performance.now()
    for (let i = 0; i < iter; i++) fn()
    return performance.now() - before
  })
  return times.indexOf(Math.min(...times))
}


// --- / --- / --- / --- //
/**
 * Однократное выполнение функции
 */
const once = (fn) => {
  let called = false
  return function (...args) {
    if (called) return
    called = true
    return fn.apply(this, args)
  }
}


// --- / --- / --- / --- //
/**
 * Выполнение функции указанное количество раз
 */
const times = (n, fn, ctx = undefined) => {
  let i = 0
  while (fn.call(ctx, i) !== false && ++i < n) {}
}

let str = ''
times(5, (i) => (str += i))
console.log(str) // 012345


// --- / --- / --- / --- //
/**
 * "Туннелирование" (последовательное выполнение) функций
 */
const pipe = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)))

const add5 = (x) => x + 5
const mult = (x, y) => x * y

const multAndAdd5 = pipe(mult, add5)
multAndAdd5(5, 2) // 15


// --- / --- / --- / --- //
/**
 * Цепочка из асинхронных функций (их последовательное выполнение)
 */
const chainAsync = (fns) => {
  let curr = 0
  const last = fns[fns.length - 1]
  const next = () => {
    const fn = fns[curr++]
    fn === last ? fn() : fn(next)
  }
  next()
}

chainAsync([
  (next) => {
    log('0 seconds')
    setTimeout(next, 1000)
  },
  (next) => {
    log('1 second')
    setTimeout(next, 1000)
  },
  () => {
    log('2 seconds')
  }
])


// --- / --- / --- / --- //
/**
 * Отложенное выполнение функции
 */
const defer = (fn, ...args) => setTimeout(fn, 1, ...args)


// --- / --- / --- / --- //
/**
 * Привязка функции к объекту (определение контекста метода)
 */
const bindKey = (ctx, fn, ...boundArgs) => (...args) =>
  ctx[fn].apply(ctx, [...boundArgs, ...args])

const john = {
  user: 'John',
  greet: function (phrase, punct) {
    return `${phrase} ${this.user}${punct}`
  }
}
const johnBound = bindKey(john, 'greet')

johnBound('Hi,', '!') // Hi, John!


// --- / --- / --- / --- //
/**
 * Синхронное (последовательное) выполнение функций
 * (возвращается массив подмассивов результатов выполнения функций)
 */
const juxt = (...fns) => (...args) => [...fns].map((fn) => [...args.map(fn)])

juxt(
  (x) => x + 1,
  (x) => x - 1,
  (x) => x * 10
)(1, 2, 3)
// [ [2, 3, 4], [0, 1, 2], [10, 20, 30]

juxt(
  (s) => s.length,
  (s) => s.split(' ').join('-')
)('JavaScript is cool')
// [ [21], ['JavaScript-is-cool'] ]


// --- / --- / --- / --- //
/**
 * Объединение функций
 */
const over = (...fns) => (...args) => fns.map((fn) => fn.apply(null, args))

const minMax = over(Math.min, Math.max)

minMax(1, 2, 3, 4, 5) // [1, 5]


// --- / --- / --- / --- //
/**
 * "Промисификация"
 */
const promisify = (fn) => (...args) =>
  new Promise((resolve, reject) =>
    fn(...args, (err, res) => (err ? reject(err) : resolve(res)))
  )

const delay = promisify((d, cb) => setTimeout(cb, d))

delay(1000).then(() => {
  console.log('Привет!')
})


// --- / --- / --- / --- //
/**
 * Мемоизация (запоминание результатов выполнения функции, memoization)
 * https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D0%BC%D0%BE%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F
 */
// 1
const memo = (fn) => {
  const cache = new Map()

  return (n) => {
    if (cache.has(n)) {
      console.log('From cache')
      return cache.get(n)
    } else {
      console.log('Calculated')
      const res = fn(n)
      cache.set(n, res)
      return res
    }
  }
}

const add = (x) => x + 10
const memoAdd = memo(add)
memoAdd(10) // Caclulated 20
memoAdd(10) // From cache 20


// 2
const memo = (fn) =>
  new Proxy(fn, {
    cache: new Map(),
    apply(target, arg, args) {
      const key = args.toString()
      if (!this.cache.has(key)) this.cache.set(key, target.apply(arg, args))
      return this.cache.get(key)
    }
  })

// 3
const _memo = (fn, cache = new Map()) => (val) =>
  cache.get(val) || (cache.set(val, fn(val)) && cache.get(val))

const fact = (n) => (n < 1 ? 1 : n * fact(n - 1))

const memoFact = _memo(fact)

console.time('t1')
console.log(memoFact(150))
console.timeEnd('t1') // 0.15
console.time('t2')
console.log(memoFact(150))
console.timeEnd('t2') // 0.05


// --- / --- / --- / --- //
/**
 * Каррирование (карринг, уменьшение "арности" функции, currying)
 * https://ru.wikipedia.org/wiki/%D0%9A%D0%B0%D1%80%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5
 */
// 1
// es5
function curryRegSum(a) {
  return function (b) {
    return function (c) {
      return a + b + c
    }
  }
}

// es6
const curryArrowSum = (a) => (b) => (c) => a + b + c

curryArrowSum(1) // b => c => 1 + b + c
curryArrowSum(1)(2) // c => 3 + c
curryArrowSum(1)(2)(3) // 6

// 2
const curryCalcEval = (...x) => (op) =>
  x.reduce((acc, cur) => (acc = eval(`acc ${op} cur`)))

curryCalcEval(1, 2, 3)('+') // 6

// создание "каррированной" функции
// 1
const randomCurry = (fn) =>
  function curry(...args) {
    return fn.length <= args.length
      ? fn.apply(this, args)
      : (...args2) => curry.apply(this, args.concat(args2))
  }

const sum = randomCurry((a, b, c, d) => a + b + c + d)

sum(1)(2)(3)(4) // 10
sum(1, 2)(3, 4) // 10

// 2
const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args)

curry(Math.pow)(2)(10) // 1024
curry(Math.min, 3)(10)(50)(2) // 2


// --- / --- / --- / --- //
/**
 * Частичное применение функции (практические применение каррирования)
 */
const partial = (fn, ...a) => (...b) => fn(...a, ...b)

const greet = (phrase, name) => `${phrase}, ${name}!`
const hello = partial(greet, 'Привет')

hello('Иван') // Привет, Иван!

// еще один пример
const mult = (...x) => x.reduce((a, b) => (a *= b))

const partMult = partial(mult, 2) // 2

partMult(3) // 2 * 3 = 6


// --- / --- / --- / --- //
/**
 * "Декаррирование" (преобразование каррированной функции в обычную)
 */
const uncurry = (fn, n = 1) => (...args) => {
  const next = (acc) => (args) => args.reduce((x, y) => x(y), acc)
  if (n > args.length) throw new RangeError('Arguments too few!')
  return next(fn)(args.slice(0, n))
}

const curAdd = (x) => (y) => (z) => x + y + z

const uncurAdd = uncurry(add, 3)
uncurAdd(1, 2, 3) // 6


// --- / --- / --- / --- //
/**
 * Debounce
 */
// 1
const debounce = (f, t) =>
  function (args) {
    let prevCall = this.lastCall
    this.lastCall = Date.now()

    if (prevCall && this.lastCall - prevCall <= t) {
      clearTimeout(this.timer)
    }

    this.timer = setTimeout(() => f(args), t)
  }

const log = (args) => console.log(`Args: ${args}`)
const debouncedLog = debounce(log, 2000)
const arr = [1, 2, 3]

debouncedLog(arr)
debouncedLog(arr)
debouncedLog(arr)
// Args: 1, 2, 3 - один раз, через 2 секунды

// 2
const _debounce = (fn, ms = 0) => {
  let id
  return function (...args) {
    clearTimeout(id)
    id = setTimeout(() => fn.apply(this, args), ms)
  }
}


// --- / --- / --- / --- //
/**
 * Throttle
 */
const throttle = (f, t) =>
  function (args) {
    let prevCall = this.lastCall
    this.lastCall = Date.now()

    if (prevCall === undefined || this.lastCall - prevCall > t) {
      f(args)
    }
  }

const log = (args) => console.log(`Args: ${args}`)
const throttledLog = throttle(log, 2000)

const arr = [1, 2, 3]

throttledLog(arr)
throttledLog(arr)
throttledLog(arr)
// Args: 1, 2, 3 - сразу, один раз
```

## <a name="dom"></a> DOM

```js
/**
 * Копирование строки в буфер обмена
 */
const copy = async (text) => await navigator.clipboard.writeText(text)

/**
 * Вставка строки из буфера обмена
 */
const paste = async (el) => {
  const text = await navigator.clipboard.readText()
  el.textContent = text
}


// --- / --- / --- / --- //
/**
 * Определение элемента, находящегося в фокусе
 */
const isFocused = (el) => el === document.activeElement

// --- / --- / --- / --- //
/**
 * Находится ли элемент в области просмотра
 * (полностью или частично)?
 */
const isVisible = (el, part = false) => {
  const { top, left, bottom, right } = el.getBoundingClientRect()
  const { innerHeight, innerWidth } = window

  return part
    ? ((top > 0 && top < innerHeight) ||
        (bottom > 0 && bottom < innerHeight)) &&
        ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth))
    : top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth
}


// --- / --- / --- / --- //
/**
 * Переключение в полноэкранный режим
 */
const fullscreen = (mode = true, el = 'body') =>
  mode
    ? document.querySelector(el).requestFullscreen()
    : document.exitFullscreen()


// --- / --- / --- / --- //
/**
 * Определение величины прокрутки
 */
const getScrollPosition = (el = window) => ({
  x: el.pageXOffset !== undefined ? el.pageXOffset : el.scrollLeft,
  y: el.pageYOffset !== undefined ? el.pageYOffset : el.scrollTop
})


// --- / --- / --- / --- //
/**
 * Определение предпочтения пользователем темной цветовой темы (схемы)
 */
const prefersDarkColorScheme = () =>
  window &&
  window.matchMedia &&
  window.matchMedia('(prefers-color-scheme: dark)').matches


// --- / --- / --- / --- //
/**
 * Получение/установка стилей элемента
 */
const getStyle = (el, prop) => getComputedStyle(el)[prop]

const setStyle = (el, prop, val) => (el.style[prop] = val)


// --- / --- / --- / --- //
/**
 * Добавление стилей в виде объекта
 */
const addStyles = (el, obj) => {
  Object.assign(el.style, obj)
}


// --- / --- / --- / --- //
/**
 * Создание элемента с помощью шаблонных литералов
 */
//  1
const createElFromStr = (str) => {
  const el = document.createElement('div')
  el.innerHTML = str
  const child = el.fisrtElementChild
  el.remove()
  return child
}


const el = createEl(
  `<div class="container">
    <p>Hello!</p>
  </div>`
)

console.log(el.className) // container

// 2
const createElFromStr = (str) => {
  const parser = new DOMParser()
  const {
    body: { children }
  } = parser.parseFromString(str, 'text/html')
  return children[0]
}

// 3
const createElFromStr = (str) => {
  const range = new Range()
  const fragment = range.createContextualFragment(str)
  return fragment
}

// или в одну строку
const createElFromStr = (str) => new Range().createContextualFragment(str)


// --- / --- / --- / --- //
/**
 * "Сериализация" формы
 */
const serializeForm = (form) =>
  Array.from(new FormData(form), (field) =>
    field.map(encodeURIComponent).join('=')
  ).join('&')


// --- / --- / --- / --- //
/**
 * Преобразование формы в объект
 */
const formToObj = (form) =>
  Array.from(new FormData(form)).reduce(
    (acc, [key, val]) => ({
      ...acc,
      [key]: val
    }),
    {}
  )


// --- / --- / --- / --- //
/**
 * Преобразование массива в список элементов `li`
 */
const arrToList = (arr, selector) =>
  (document.querySelector(selector).innerHTML += arr
    .map((i) => `<li>${i}</li>`)
    .join(''))

// HTML
// <ul id="list"></ul>
arrToList(['foo', 'bar'], '#list')


// --- / --- / --- / --- //
/**
 * Наблюдение за всеми изменениями, происходящими в документе
 */
const observeMutations = (el, cb, opts) => {
  const O = new MutationObserver((ms) => ms.forEach((m) => cb(m)))
  O.observe(
    el,
    Object.assign(
      {
        childList: true,
        attributes: true,
        attributeOldValue: true,
        characterData: true,
        characterDataOldValue: true,
        subtree: true
      },
      opts
    )
  )
  return O
}

observeMutations(document, log)


// --- / --- / --- / --- //
/**
 * Добавление обработчика событий
 */
const on = (el, ev, fn, opts = {}) => {
  const delegator = (e) => e.target.matches(opts.target) && fn.call(e.target, e)
  el.addEventListener(ev, opts.target ? delegator : fn, opts.options || false)
  if (opts.target) return delegator
}

const fn = () => console.log('!')
on(document, 'click', fn) // при клике по `body` выводится '!'
// HTML
// <p>click me</p>
on(document, 'click', fn, { target: 'p' })
// при клике по `p` выводится '!'
on(document, 'click', fn, { options: true })
// capturing вместо bubbling


// --- / --- / --- / --- //
/**
 * Удаление обработчика событий
 */
const off = (el, ev, fn, opt = false) => {
  el.removeEventLisntener(ev, fn, opt)
}

const fn = () => console.log('!')
window.addEventListener('click', fn)
off(window, 'click', fn)


// --- / --- / --- / --- //
/**
 * Вызов пользовательского ("кастомного") события
 */
const trigger = (el, type, opts) =>
  el.dispatchEvent(new CustomEvent(type, { opts }))

const getOne = (selector, el = document) => el.querySelector(selector)

const par = getOne('#username')
const btn = getOne('button')
const fn = ({ target, detail: { username } }) => {
  target.textContent = username
}

on(el, 'click', fn)

// HTML
// <p id="username"></p>
// <button>trigger event</button>
btn.onclick = () => {
  trigger(el, 'click', { username: 'John' })
}


// --- / --- / --- / --- //
/**
 * Адреса изображений (с дубликатами или без)
 */
const getImgs = (el, dup = false) => {
  const imgs = [...el.querySelectorAll('img')].map((i) => i.getAttribute('src'))
  return dup ? imgs : [...new Set(imgs)]
}


// --- / --- / --- / --- //
/**
 * Один/все элементы, соответствующие указанному селектору
 */
const getOne = (selector, el = document) => el.querySelector(selector)

const getAll = (selector, el = document) => [...el.querySelectorAll(selector)]

const getEl = (selector, all = false, el = document) => all ? [...el.querySelectorAll(selector)] : el.querySelector(selector)


// --- / --- / --- / --- //
/**
 * Cкрытие/отображение указанных элементов
 */
const getAll = (selector, el = document) => [...el.querySelectorAll(selector)]

const hide = (val) => {
  const arr = typeof val === 'string' ? getAll(val) : [...val]
  arr.forEach((i) => {
    i.style.display = 'none'
  })
}

const show = (val) => {
  const arr = typeof val === 'string' ? getAll(val) : [...val]
  arr.forEach((i) => {
    i.style.display = ''
  })
}


// --- / --- / --- / --- //
/**
 * Определение завершения прокрутки
 */
const onScrollStop = (cb) => {
  let isScroll
  window.addEventListener(
    'scroll',
    () => {
      clearTimeout(isScroll)
      isScroll = setTimeout(() => {
        cb()
      }, 300)
    },
    false
  )
}

onScrollStop(() => {
  console.log('scrolling stopped')
})


// --- / --- / --- / --- //
/**
 * Регистрация клика за пределами и внутри указанного элемента
 */
const onClickOutside = (el, cb) => {
  document.addEventListener('click', ({ target }) => {
    if (!el.contains(target)) cb()
  })
}

const onClickInside = (el, cb) => {
  document.addEventListener('click', ({ target }) => {
    if (el.contains(target)) cb()
  })
}


// --- / --- / --- / --- //
/**
 * Запуск/остановка покадровой отрисовки страницы
 */
const recordAnimFrames = (cb, auto = true) => {
  let isRun = false,
    raf
  const stop = () => {
    if (!isRun) return
    isRun = false
    cancelAnimationFrame(raf)
  }
  const start = () => {
    if (isRun) return
    isRun = true
    run()
  }
  const run = () => {
    raf = requestAnimationFrame(() => {
      cb()
      if (isRun) run()
    })
  }
  if (auto) start()
  return { start, stop }
}


// --- / --- / --- / --- //
/**
 * Определение всех предков элемента
 */
const getAncestors = (el) => {
  let ans = []
  while (el) {
    ans.unshift(el)
    el = el.parentNode
  }
  return ans
}


// --- / --- / --- / --- //
/**
 * Определение наличия, добавление/удаление класса
 */
const hasClass = (el, str, part = false) =>
  !part ? el.classList.contains(str) : el.className.includes(str)

const addClass = (el, str, part = false) =>
  !part ? (el.className = str) : el.classList.add(str)

const removeClass = (el, str, part = false) =>
  !part ? (el.className = '') : el.classList.remove(str)


// --- / --- / --- / --- //
/**
 * Плавная прокрутка в начало страницы
 */
const scrollToTop = () => {
  const c = document.documentElement.scrollTop || document.body.scrollTop
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop)
    window.scrollTo(0, c - c / 8)
  }
}
// HTML
// <button>top</button>
document.querySelector('button').onclick = () => {
  scrollToTop()
}


// --- / --- / --- / --- //
/**
 * Плавная прокрутка к указанному элементу
 */
const getOne = (selector, el = document) => el.querySelector(selector)

const scroll = (selector) =>
  getOne(selector).scrollIntoView({
    behavior: 'smooth'
  })


// --- / --- / --- / --- //
/**
 * Удаление, замена ("обеззараживание") и восстановление HTML
 */
const removeHtml = (str) => str.replace(/<[^>]*>/g, '')

const escapeHtml = (str) =>
  str.replace(
    /[&<>'"]/g,
    (tag) =>
      ({
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
      }[tag] || tag)
  )

escapeHtml('<a href="#">React & TypeScript</a>')
// '&lt;a href=&quot;#&quot;&gt;React &amp; TypeScript&lt;/a&gt;'

const unescapeHtml = (str) =>
  str.replace(
    /&amp;|&lt;|&gt;|&#39;|&quot;/g,
    (tag) =>
      ({
        '&amp;': '&',
        '&lt;': '<',
        '&gt;': '>',
        '&#39;': "'",
        '&quot;': '"'
      }[tag] || tag)
  )

unescapeHtml('&lt;a href=&quot;#&quot;&gt;React &amp; TypeScript&lt;/a&gt;')
// '<a href="#">React & TypeScript</a>'
```

## <a name="diff"></a> Разное

```js
/**
 * Работа с URL
 */
// получение базового адреса
const getBaseURL = (url) => url.replace(/[?#].*$/, '')

getBaseURL('http://example.com/page?name=John&surname=Smith') // http://example.com

// является ли URL абсолютным?
const isAbsoluteURL = (str) => /^[a-z][a-z0-9+.-]*:/.test(str)

isAbsoluteURL('https://google.com') // true
isAbsoluteURL('ftp://www.myserver.net') // true
isAbsoluteURL('/foo/bar') // false

// перенаправление с http на https
const httpsRedirect = () => {
  if (location.protocol !== 'https:')
    location.replace('https://' + location.href.split('//')[1])
}

// извлечение параметров URL
const getURLParams = (url) =>
  (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
    (a, v) => (
      (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a
    ),
    {}
  )

getURLParams('http://url.com/page?name=John&surname=Smith') // { name: 'John', surname: 'Smith' }

// преобразование строки запроса в объект
const queryStrToObj = (url) =>
  [...new URLSearchParams(url.split('?')[1])].reduce(
    (a, [k, v]) => ((a[k] = v), a),
    {}
  )

queryStrToObj('https://google.com?page=1&count=10')
// { page: '1', count: '10' }

// генерация URL
const joinURL = (...args) =>
  args
    .join('/')
    .replace(/[\/]+/g, '/')
    .replace(/^(.+):\//, '$1://')
    .replace(/^file:/, 'file:/')
    .replace(/\/(\?|&|#[^!])/g, '$1')
    .replace(/\?/g, '&')
    .replace('&', '?')

joinURL('http://www.google.com', 'a', '/b/cd', '?foo=123', '?bar=baz')
// http://www.google.com/a/b/cd?foo=123&bar=baz


// --- / --- / --- / --- //
/**
 * GET/POST-запросы
 */
// GET
// XMLHttpRequest
const httpGet = (url, cb, err = console.error) => {
  const req = new XMLHttpRequest()
  req.open('GET', url, true)
  req.onload = () => cb(req.responseText)
  req.onerror = () => err(req)
  req.send()
}

// async/await
const httpGet = async (url, cb, err = console.error) => {
  try {
    const response = await fetch(url)
    const result = await response.json()
    cb(result)
  } catch (error) {
    err(error)
  }
}

// POST
// XMLHttpRequest
const httpPost = (url, data, cb, err = console.error) => {
  const req = new XMLHttpRequest()
  req.open('POST', url, true)
  req.setRequestHeader('Content-Type', 'application/json')
  req.onload = () => cb(req.responseText)
  req.onerror = () => err(req)
  req.send(data)
}

// async/await
const httpPost = async (url, data, cb, err = console.error) => {
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    })
    const result = await response.json()
    cb(result)
  } catch (error) {
    err(error)
  }
}


// --- / --- / --- / --- //
/**
 * Работа с цветом
 */
// 1
const getRandomHexColor = () =>
  `#${((Math.random() * 0xffffff) << 0).toString(16)}`

// 2
const getRandomRGBAColor = (opacity) => {
  const random = () => ~~(Math.random() * 255)
  return `rgba(${random()}, ${random()}, ${random()}, ${opacity})`
}

// 3
const RGBToHex = (r, g, b) =>
  ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0')

RGBToHex(255, 165, 1) // 'ffa501'


// --- / --- / --- / --- //
/**
 * Генерация уникальных значений
 */
// 1
const getRandomAlphaNumeric = (length) => {
  let s = ''
  Array.from({ length }).some(() => {
    s += Math.random().toString(36).slice(2)
    return s.length >= length
  })
  return s.slice(0, length)
}

getRandomAlphaNumeric(5) // bf8lf

// 2
// вспомогательная функция, обеспечивающая
// уникальность генерируемых значений
function outerMemo(fn, cache = new Set()) {
  return function innerMemo(res = fn()) {
    if (cache.has(res)) {
      return innerMemo()
    } else {
      cache.add(res)
      return res
    }
  }
}

const getUniqId = outerMemo(() =>
  (~~(Math.random() * 1e6)).toString(16).slice(0, 4).padStart(5, 'x')
)

getUniqId() // xc1b9

// 3
const genUUID = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  )

genUUID() // 7982fcfe-5721-4632-bede-6000885be57d


// --- / --- / --- / --- //
/**
 * Работа с датами
 */
// 1
const getDaysBetween = (start, end) =>
  Array.from({ length: (end - start) / (1000 * 3600 * 24) }).reduce((c) => {
    if (start.getDay() % 6 !== 0) c++
    start = new Date(start.setDate(start.getDate() + 1))
    return c
  }, 0)

getDaysBetween(new Date('2021-02-06'), new Date('2021-02-08')) // 2

// 2
const changeDay = (n) => {
  let d = new Date()
  d.setDate(d.getDate() + n)
  return d.toLocaleDateString()
}

changeDay(-1) // 18.02.2021


// --- / --- / --- / --- //
/**
 * Разбор куки
 */
const parseCookie = (str) =>
  str
    .split(';')
    .map((v) => v.split('='))
    .reduce((acc, cur) => {
      acc[decodeURIComponent(cur[0].trim())] = decodeURIComponent(cur[1].trim())
      return acc
    }, {})

parseCookie('foo=bar; equation=E%3Dmc%5E2')
// { foo: "bar", equation: "E=mc^2" }


// --- / --- / --- / --- //
/**
 * Счетчики
 */
// 1
const createCounter = (n = 0) => ({
  get: () => n,
  plus: () => ++n,
  minus: () => --n,
  reset: () => (n = 0)
})

const log = console.log

const counter = createCounter(5)
log(counter.get()) // 5
counter.plus()
counter.plus()
log(counter.get()) // 7
counter.reset()
log(counter.get()) // 0

// 2
const counter = (selector, start, end, step = 1, duration = 2000) => {
  const el = document.querySelector(selector)
  let current = start,
    _step = (end - start) * step < 0 ? -step : step,
    timer = setInterval(() => {
      current += _step
      el.textContent = current
      if (current >= end) el.textContent = end
      if (current >= end) clearInterval(timer)
    }, Math.abs(~~(duration / (end - start))))
}

// HTML
// <p id="counter"></p>
counter('#counter', 0, 100, 5, 10000)


// --- / --- / --- / --- //
/**
 * Генераторы
 */
// 1
const range = function* (start, end, step = 1) {
  let i = start
  while (i < end) {
    yield i
    i += step
  }
}

// 2
const _range = (end, start = 0, step = 1) => {
  function* generate() {
    let x = start - step
    while (x <= end - step) yield (x += step)
  }
  return {
    [Symbol.iterator]: generate
  }
}


// --- / --- / --- / --- //
/**
 * Переключатель
 */
const toggle = (...args) => {
  let state = -1
  const len = args.length

  return () => {
    state = (state + 1) % len
    return args[state]
  }
}

const onOff = toggle('on', 'off')

console.log(onOff()) // on
console.log(onOff()) // off


// --- / --- / --- / --- //
/**
 * Публикация/подписка
 */
const pubSub = (subscribers = []) => ({
  publish: ({ message, sender }) => {
    subscribers.forEach(({ name }) => {
      console.log(`${name} get ${message} from ${sender}`)
    })
  },
  subscribe: (subscriber) => {
    subscribers.push(subscriber)
  }
})

const pubSubObj = pubSub()
pubSubObj.subscribe({ name: 'John' })
pubSubObj.subscribe({ name: 'Jane' })
pubSubObj.publish({ message: 'Hello', sender: 'Bob' })
// John get Hello from Bob
// Jane get Hello from Bob


// --- / --- / --- / --- //
/**
 * "Шина" событий
 */
const createEventHub = () => ({
  hub: Object.create(null),
  emit(e, d) {
    ;(this.hub[e] || []).forEach((h) => h(d))
  },
  on(e, h) {
    if (!this.hub[e]) this.hub[e] = []
    this.hub[e].push(h)
  },
  off(e, h) {
    const i = (this.hub[e] || []).findIndex((handler) => handler === h)
    if (i > -1) this.hub[e].splice(i, 1)
    if (this.hub[e].length === 0) delete this.hub[e]
  }
})


// --- / --- / --- / --- //
/**
 * Fizz buzz
 * https://ru.wikipedia.org/wiki/Fizz_buzz
 */
// 1
const fizzBuzz = (n) => {
  const arr = []
  do {
    if (n % 15 === 0) arr.push('Fizz Buzz')
    else if (n % 3 === 0) arr.push('Fizz')
    else if (n % 5 === 0) arr.push('Buzz')
    else arr.push(n)
  } while (n-- > 1)
  return arr
}

fizzBuzz(15)
// ["Fizz Buzz", 14, 13, "Fizz", 11, "Buzz", "Fizz", 8, 7, "Fizz", "Buzz", 4, "Fizz", 2, 1]

// 2
const _fizzBuzz = (n) => {
  const arr = []
  for (let i = 1; i <= n; i++) {
    let a = i % 3 === 0,
      b = i % 5 === 0
    arr.push(a ? (b ? 'FizzBuzz' : 'Fizz') : b ? 'Buzz' : i)
  }
  return arr
}

_fizzBuzz(15)
// [1, 2, "Fizz", 4, "Buzz", "Fizz", 7, 8, "Fizz", "Buzz", 11, "Fizz", 13, 14, "FizzBuzz"]


// --- / --- / --- / --- //
/**
 * Лестница (stairs)
 */
// 1
const steps = (n) => {
  let stairs = ''

  for (let r = 0; r < n; r++) {
    let stair = ''

    for (let c = 0; c < n; c++) {
      stair += c <= r ? '#' : ' '
    }

    stairs += stair + '\n'
  }

  return stairs
}

steps(5)
/*
  #
  ##
  ###
  ####
  #####
*/

// 2
const _steps = (n, r = 0, stair = '', stairs = '') => {
  if (r === n) return stairs

  if (stair.length === n) return _steps(n, r + 1, '', stairs + stair + '\n')

  return _steps(n, r, stair + (stair.length <= r ? '#' : ' '), stairs)
}


// --- / --- / --- / --- //
/**
 * Пирамида (pyramid)
 */
// 1
const pyramid = (n) => {
  let levels = ''
  const mid = ~~((2 * n - 1) / 2)

  for (let r = 0; r < n; r++) {
    let level = ''

    for (let c = 0; c < 2 * n - 1; c++) {
      level += mid - r <= c && c <= mid + r ? '#' : ' '
    }

    levels += level + '\n'
  }

  return levels
}

pyramid(5)

/*
    #
   ###
  #####
 #######
#########
*/

// 2
const _pyramid = (n, r = 0, level = '', levels = '') => {
  if (n === r) return levels

  if (2 * n - 1 === level.length)
    return _pyramid(n, r + 1, '', levels + level + '\n')

  const mid = ~~((2 * n - 1) / 2)

  return _pyramid(
    n,
    r,
    level + (mid - r <= level.length && level.length <= mid + r ? '#' : ' '),
    levels
  )
}


// --- / --- / --- / --- //
/**
 * Спиральная матрица (spiral matrix)
 */
const spiral = (n) => {
  let counter = 1
  let startR = 0,
    endR = n - 1
  let startC = 0,
    endC = n - 1

  const matrix = []

  for (let i = 0; i < n; i++) matrix.push([])

  while (startC <= endC && startR <= endR) {
    for (let i = startC; i <= endC; i++) {
      matrix[startR][i] = counter
      counter++
    }
    startR++

    for (let i = startR; i <= endR; i++) {
      matrix[i][endC] = counter
      counter++
    }
    endC--

    for (let i = endC; i >= startC; i--) {
      matrix[endR][i] = counter
      counter++
    }
    endR--

    for (let i = endR; i >= startR; i--) {
      matrix[i][startC] = counter
      counter++
    }
    startC++
  }

  return matrix
}

spiral(3)
/*
[
  [1, 2, 3],
  [8, 9, 4],
  [7, 6, 5]
]
*/


// --- / --- / --- / --- //
/**
 * Шифр Цезаря
 * https://ru.wikipedia.org/wiki/%D0%A8%D0%B8%D1%84%D1%80_%D0%A6%D0%B5%D0%B7%D0%B0%D1%80%D1%8F
 */
const caesarCipher = (str, num) => {
  const arr = [...'abcdefghijklmnopqrstuvwxyz']
  let newStr = ''

  for (const char of str) {
    const lower = char.toLowerCase()

    if (!arr.includes(lower)) {
      newStr += char
      continue
    }

    let index = arr.indexOf(lower) + (num % 26)

    if (index > 25) index -= 26
    if (index < 0) index += 26

    newStr +=
      char === char.toUpperCase() ? arr[index].toUpperCase() : arr[index]
  }
  return newStr
}

caesarCipher('Hello World', 100) // Dahhk Sknhz
caesarCipher('Dahhk Sknhz', -100) // Hello World
```

## HOC

```js
// wrapping
const addTiming =
  (fn) =>
  (...args) => {
    const start = performance.now()
    try {
      const toReturn = fn(...args)
      // throw new Error('error')
      console.log('success')
      return toReturn
    } catch (e) {
      console.error('fail', e.message || e)
    } finally {
      console.log(
        'executing',
        fn.name,
        'takes',
        performance.now() - start,
        'ms'
      )
    }
  }

function add(x, y, z) {
  for (let i = 0; i < 1000000; i++);
  return x + y + z
}

add = addTiming(add)
// add(1, 2, 3)

const not =
  (fn) =>
  (...args) =>
    !fn(...args)

const unary =
  (fn) =>
  (first, ...rest) =>
    fn(first)

const demethodize =
  (fn) =>
  (...args) =>
    fn.bind(...args)()
const upper = demethodize(String.prototype.toUpperCase)
console.log(upper('hi'))
```

### Парсинг и подсветка синтаксиса JS или JSON-объекта

```js
const parseAndHighlightObj = (json) => {
  const inner = (_json) =>
    Object.entries(_json).map(([key, value]) => {
      let type = typeof value

      const isSimpleValue =
        ['string', 'number', 'boolean'].includes(type) || !value

      if (isSimpleValue && type === 'object') {
        type = 'null'
      }

      return `
        <div class="line">
          <span class="key">${key}:</span>
          ${
            isSimpleValue
              ? `<span class="${type}">${value}</span>`
              : inner(value)
          }
        </div>
      `
    })

  return `<div class="json">${inner(json).join('')}</div>`
}

const result = parseAndHighlightObj({
  a: 1,
  b: {
    c: {
      d: 'foo'
    }
  },
  2: {
    3: true
  }
})

document.body.innerHTML = result
```

```css
.json {
  padding: 1rem;
  background-color: #3c3c3c;
  border-radius: 4px;
  color: #f0f0f0;
}

.line {
  margin-left: 1rem;
}

.line > .line {
  margin-left: 1rem;
}

.key {
  font-style: italic;
}

.number {
  color: lightblue;
}

.string {
  color: lightcoral;
}

.boolean {
  color: lightgreen;
}
```