# Каталог рефакторингів

Курований каталог технік рефакторингу з книги Мартіна Фаулера *Refactoring* (2-ге видання). Кожен рефакторинг включає мотивацію, покрокову механіку та приклади.

> «Рефакторинг визначається своєю механікою — точною послідовністю кроків, яких ви дотримуєтесь для здійснення зміни.» — Мартін Фаулер

---

## Як використовувати цей каталог

1. **Виявити запах** за допомогою довідника запахів коду
2. **Знайти відповідний рефакторинг** у цьому каталозі
3. **Дотримуватися механіки** крок за кроком
4. **Тестувати після кожного кроку** для збереження поведінки

**Золоте правило**: Якщо будь-який крок займає більше 10 хвилин, розбийте його на менші кроки.

---

## Найпоширеніші рефакторинги

### Extract Method

**Коли використовувати**: Довгий метод, дубльований код, потрібно назвати концепцію

**Мотивація**: Перетворити фрагмент коду на метод, назва якого пояснює призначення.

**Механіка**:
1. Створити новий метод, названий за тим, що він робить (не як)
2. Скопіювати фрагмент коду в новий метод
3. Просканувати локальні змінні, використані у фрагменті
4. Передати локальні змінні як параметри (або оголосити в методі)
5. Обробити повернені значення належним чином
6. Замінити оригінальний фрагмент викликом нового методу
7. Тестувати

**До**:
```javascript
function printOwing(invoice) {
  let outstanding = 0;

  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");

  // Обчислення заборгованості
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }

  // Друк деталей
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

**Після**:
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, order) => sum + order.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

---

### Inline Method

**Коли використовувати**: Тіло методу таке ж зрозуміле, як його назва; надмірне делегування

**Мотивація**: Видалити непотрібну непрямість, коли метод не додає цінності.

**Механіка**:
1. Перевірити, що метод не є поліморфним
2. Знайти всі виклики методу
3. Замінити кожен виклик тілом методу
4. Тестувати після кожної заміни
5. Видалити визначення методу

**До**:
```javascript
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

**Після**:
```javascript
function getRating(driver) {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

### Extract Variable

**Коли використовувати**: Складний вираз, важкий для розуміння

**Мотивація**: Дати назву частині складного виразу.

**Механіка**:
1. Переконатися, що вираз не має побічних ефектів
2. Оголосити незмінну змінну
3. Встановити її як результат виразу (або частини)
4. Замінити оригінальний вираз змінною
5. Тестувати

**До**:
```javascript
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100);
```

**Після**:
```javascript
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```

---

### Inline Variable

**Коли використовувати**: Назва змінної не передає більше, ніж вираз

**Мотивація**: Видалити непотрібну непрямість.

**Механіка**:
1. Перевірити, що права частина не має побічних ефектів
2. Якщо змінна не незмінна, зробити її такою і тестувати
3. Знайти перше посилання та замінити виразом
4. Тестувати
5. Повторити для всіх посилань
6. Видалити оголошення та присвоєння
7. Тестувати

---

### Rename Variable

**Коли використовувати**: Назва не чітко передає призначення

**Мотивація**: Хороші назви критичні для чистого коду.

**Механіка**:
1. Якщо змінна широко використовується, розглянути інкапсуляцію
2. Знайти всі посилання
3. Змінити кожне посилання
4. Тестувати

**Поради**:
- Використовуйте назви, що розкривають намір
- Уникайте абревіатур
- Використовуйте доменну термінологію

```javascript
// Погано
const d = 30;
const x = users.filter(u => u.a);

// Добре
const daysSinceLastLogin = 30;
const activeUsers = users.filter(user => user.isActive);
```

---

### Change Function Declaration

**Коли використовувати**: Назва функції не пояснює призначення, параметри потребують зміни

**Мотивація**: Хороші назви функцій роблять код самодокументованим.

**Механіка (проста)**:
1. Видалити непотрібні параметри
2. Змінити назву
3. Додати потрібні параметри
4. Тестувати

**Механіка (міграція — для складних змін)**:
1. Якщо видаляєте параметр, переконайтеся, що він не використовується
2. Створити нову функцію з бажаним оголошенням
3. Зробити так, щоб стара функція викликала нову
4. Тестувати
5. Змінити виклики на нову функцію
6. Тестувати після кожного
7. Видалити стару функцію

**До**:
```javascript
function circum(radius) {
  return 2 * Math.PI * radius;
}
```

**Після**:
```javascript
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

---

### Encapsulate Variable

**Коли використовувати**: Прямий доступ до даних з кількох місць

**Мотивація**: Забезпечити чітку точку доступу для маніпуляції даними.

**Механіка**:
1. Створити функції getter та setter
2. Знайти всі посилання
3. Замінити читання на getter
4. Замінити записи на setter
5. Тестувати після кожної зміни
6. Обмежити видимість змінної

**До**:
```javascript
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

// Використовується в багатьох місцях
spaceship.owner = defaultOwner;
```

**Після**:
```javascript
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };

function defaultOwner() { return defaultOwnerData; }
function setDefaultOwner(arg) { defaultOwnerData = arg; }

spaceship.owner = defaultOwner();
```

---

### Introduce Parameter Object

**Коли використовувати**: Кілька параметрів, що часто зʼявляються разом

**Мотивація**: Згрупувати дані, що природно належать разом.

**Механіка**:
1. Створити новий клас/структуру для згрупованих параметрів
2. Тестувати
3. Використати Change Function Declaration для додавання нового обʼєкта
4. Тестувати
5. Для кожного параметра в групі видалити його з функції та використати новий обʼєкт
6. Тестувати після кожного

**До**:
```javascript
function amountInvoiced(startDate, endDate) { ... }
function amountReceived(startDate, endDate) { ... }
function amountOverdue(startDate, endDate) { ... }
```

**Після**:
```javascript
class DateRange {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}

function amountInvoiced(dateRange) { ... }
function amountReceived(dateRange) { ... }
function amountOverdue(dateRange) { ... }
```

---

### Combine Functions into Class

**Коли використовувати**: Кілька функцій оперують тими самими даними

**Мотивація**: Згрупувати функції з даними, над якими вони працюють.

**Механіка**:
1. Застосувати Encapsulate Record до спільних даних
2. Перемістити кожну функцію в клас
3. Тестувати після кожного переміщення
4. Замінити аргументи даних використанням полів класу

**До**:
```javascript
function base(reading) { ... }
function taxableCharge(reading) { ... }
function calculateBaseCharge(reading) { ... }
```

**Після**:
```javascript
class Reading {
  constructor(data) { this._data = data; }

  get base() { ... }
  get taxableCharge() { ... }
  get calculateBaseCharge() { ... }
}
```

---

### Split Phase

**Коли використовувати**: Код працює з двома різними речами

**Мотивація**: Розділити код на окремі фази з чіткими межами.

**Механіка**:
1. Створити другу функцію для другої фази
2. Тестувати
3. Ввести проміжну структуру даних між фазами
4. Тестувати
5. Витягти першу фазу в окрему функцію
6. Тестувати

**До**:
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  const shippingPerCase = (basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = quantity * shippingPerCase;
  return basePrice - discount + shippingCost;
}
```

**Після**:
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const priceData = calculatePricingData(product, quantity);
  return applyShipping(priceData, shippingMethod);
}

function calculatePricingData(product, quantity) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  return { basePrice, quantity, discount };
}

function applyShipping(priceData, shippingMethod) {
  const shippingPerCase = (priceData.basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = priceData.quantity * shippingPerCase;
  return priceData.basePrice - priceData.discount + shippingCost;
}
```

---

## Переміщення функцій (Moving Features)

### Move Method

**Коли використовувати**: Метод використовує більше можливостей іншого класу, ніж свого

**Мотивація**: Розміщувати функції разом з даними, які вони використовують найбільше.

**Механіка**:
1. Дослідити всі елементи програми, використані методом у своєму класі
2. Перевірити, чи метод є поліморфним
3. Скопіювати метод у цільовий клас
4. Адаптувати до нового контексту
5. Зробити оригінальний метод делегуючим до цільового
6. Тестувати
7. Розглянути видалення оригінального методу

---

### Move Field

**Коли використовувати**: Поле використовується більше іншим класом

**Мотивація**: Тримати дані разом з функціями, що їх використовують.

**Механіка**:
1. Інкапсулювати поле, якщо ще не зроблено
2. Тестувати
3. Створити поле в цільовому класі
4. Оновити посилання на використання цільового поля
5. Тестувати
6. Видалити оригінальне поле

---

### Move Statements into Function

**Коли використовувати**: Той самий код завжди зʼявляється разом з викликом функції

**Мотивація**: Видалити дублювання, перемістивши повторюваний код у функцію.

**Механіка**:
1. Витягти повторюваний код у функцію, якщо ще не зроблено
2. Перемістити оператори в цю функцію
3. Тестувати
4. Якщо виклики більше не потребують окремих операторів, видалити їх

---

### Move Statements to Callers

**Коли використовувати**: Загальна поведінка різниться між викликачами

**Мотивація**: Коли поведінка має відрізнятися, перемістити її з функції.

**Механіка**:
1. Використати Extract Method на коді для переміщення
2. Використати Inline Method на оригінальній функції
3. Видалити тепер вбудований виклик
4. Перемістити витягнутий код до кожного виклику
5. Тестувати

---

## Організація даних (Organizing Data)

### Replace Primitive with Object

**Коли використовувати**: Елемент даних потребує більше поведінки, ніж просте значення

**Мотивація**: Інкапсулювати дані разом з їхньою поведінкою.

**Механіка**:
1. Застосувати Encapsulate Variable
2. Створити простий клас значення
3. Змінити setter для створення нового екземпляра
4. Змінити getter для повернення значення
5. Тестувати
6. Додати багатшу поведінку до нового класу

**До**:
```javascript
class Order {
  constructor(data) {
    this.priority = data.priority; // рядок: "high", "rush" тощо
  }
}

// Використання
if (order.priority === "high" || order.priority === "rush") { ... }
```

**Після**:
```javascript
class Priority {
  constructor(value) {
    if (!Priority.legalValues().includes(value))
      throw new Error(`Invalid priority: ${value}`);
    this._value = value;
  }

  static legalValues() { return ['low', 'normal', 'high', 'rush']; }
  get value() { return this._value; }

  higherThan(other) {
    return Priority.legalValues().indexOf(this._value) >
           Priority.legalValues().indexOf(other._value);
  }
}

// Використання
if (order.priority.higherThan(new Priority("normal"))) { ... }
```

---

### Replace Temp with Query

**Коли використовувати**: Тимчасова змінна зберігає результат виразу

**Мотивація**: Зробити код зрозумілішим, витягнувши вираз у функцію.

**Механіка**:
1. Перевірити, що змінна присвоюється лише один раз
2. Витягти праву частину присвоєння в метод
3. Замінити посилання на тимчасову змінну викликом методу
4. Тестувати
5. Видалити оголошення та присвоєння тимчасової змінної

**До**:
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) {
  return basePrice * 0.95;
} else {
  return basePrice * 0.98;
}
```

**Після**:
```javascript
get basePrice() {
  return this._quantity * this._itemPrice;
}

// В методі
if (this.basePrice > 1000) {
  return this.basePrice * 0.95;
} else {
  return this.basePrice * 0.98;
}
```

---

## Спрощення умовної логіки (Simplifying Conditional Logic)

### Decompose Conditional

**Коли використовувати**: Складний умовний оператор (if-then-else)

**Мотивація**: Зробити намір зрозумілим, витягнувши умови та дії.

**Механіка**:
1. Застосувати Extract Method на умові
2. Застосувати Extract Method на then-гілці
3. Застосувати Extract Method на else-гілці (якщо є)

**До**:
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quantity * plan.summerRate;
} else {
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
}
```

**Після**:
```javascript
if (isSummer(aDate, plan)) {
  charge = summerCharge(quantity, plan);
} else {
  charge = regularCharge(quantity, plan);
}

function isSummer(date, plan) {
  return !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
}

function summerCharge(quantity, plan) {
  return quantity * plan.summerRate;
}

function regularCharge(quantity, plan) {
  return quantity * plan.regularRate + plan.regularServiceCharge;
}
```

---

### Consolidate Conditional Expression

**Коли використовувати**: Кілька умов з однаковим результатом

**Мотивація**: Зробити зрозумілим, що умови є однією перевіркою.

**Механіка**:
1. Перевірити відсутність побічних ефектів в умовах
2. Обʼєднати умови за допомогою `and` або `or`
3. Розглянути Extract Method на обʼєднаній умові

**До**:
```javascript
if (employee.seniority < 2) return 0;
if (employee.monthsDisabled > 12) return 0;
if (employee.isPartTime) return 0;
```

**Після**:
```javascript
if (isNotEligibleForDisability(employee)) return 0;

function isNotEligibleForDisability(employee) {
  return employee.seniority < 2 ||
         employee.monthsDisabled > 12 ||
         employee.isPartTime;
}
```

---

### Replace Nested Conditional with Guard Clauses

**Коли використовувати**: Глибоко вкладені умови ускладнюють відстеження потоку

**Мотивація**: Використовувати захисні вирази (guard clauses) для спеціальних випадків, зберігаючи нормальний потік зрозумілим.

**Механіка**:
1. Знайти умови спеціальних випадків
2. Замінити їх захисними виразами з раннім поверненням
3. Тестувати після кожної зміни

**До**:
```javascript
function payAmount(employee) {
  let result;
  if (employee.isSeparated) {
    result = { amount: 0, reasonCode: "SEP" };
  } else {
    if (employee.isRetired) {
      result = { amount: 0, reasonCode: "RET" };
    } else {
      result = calculateNormalPay(employee);
    }
  }
  return result;
}
```

**Після**:
```javascript
function payAmount(employee) {
  if (employee.isSeparated) return { amount: 0, reasonCode: "SEP" };
  if (employee.isRetired) return { amount: 0, reasonCode: "RET" };
  return calculateNormalPay(employee);
}
```

---

### Replace Conditional with Polymorphism

**Коли використовувати**: Switch/case за типом, умовна логіка що варіюється за типом

**Мотивація**: Дозволити обʼєктам обробляти свою поведінку самостійно.

**Механіка**:
1. Створити ієрархію класів (якщо не існує)
2. Використати фабричну функцію для створення обʼєктів
3. Перемістити умовну логіку в метод суперкласу
4. Створити метод підкласу для кожного випадку
5. Видалити оригінальну умову

**До**:
```javascript
function plumage(bird) {
  switch (bird.type) {
    case 'EuropeanSwallow':
      return "average";
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? "tired" : "average";
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? "scorched" : "beautiful";
    default:
      return "unknown";
  }
}
```

**Після**:
```javascript
class Bird {
  get plumage() { return "unknown"; }
}

class EuropeanSwallow extends Bird {
  get plumage() { return "average"; }
}

class AfricanSwallow extends Bird {
  get plumage() {
    return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
}

class NorwegianBlueParrot extends Bird {
  get plumage() {
    return (this.voltage > 100) ? "scorched" : "beautiful";
  }
}

function createBird(data) {
  switch (data.type) {
    case 'EuropeanSwallow': return new EuropeanSwallow(data);
    case 'AfricanSwallow': return new AfricanSwallow(data);
    case 'NorwegianBlueParrot': return new NorwegianBlueParrot(data);
    default: return new Bird(data);
  }
}
```

---

### Introduce Special Case (Null Object)

**Коли використовувати**: Повторні перевірки на null для спеціальних випадків

**Мотивація**: Повертати спеціальний обʼєкт, що обробляє спеціальний випадок.

**Механіка**:
1. Створити клас спеціального випадку з очікуваним інтерфейсом
2. Додати перевірку isSpecialCase
3. Ввести фабричний метод
4. Замінити перевірки на null використанням обʼєкта спеціального випадку
5. Тестувати

**До**:
```javascript
const customer = site.customer;
// ... багато місць з перевіркою
if (customer === "unknown") {
  customerName = "occupant";
} else {
  customerName = customer.name;
}
```

**Після**:
```javascript
class UnknownCustomer {
  get name() { return "occupant"; }
  get billingPlan() { return registry.defaultPlan; }
}

// Фабричний метод
function customer(site) {
  return site.customer === "unknown"
    ? new UnknownCustomer()
    : site.customer;
}

// Використання — перевірки на null не потрібні
const customerName = customer.name;
```

---

## Рефакторинг API (Refactoring APIs)

### Separate Query from Modifier

**Коли використовувати**: Функція і повертає значення, і має побічні ефекти

**Мотивація**: Зробити зрозумілим, які операції мають побічні ефекти.

**Механіка**:
1. Створити нову функцію-запит
2. Скопіювати логіку повернення оригінальної функції
3. Змінити оригінал, щоб повертав void
4. Замінити виклики, що використовують повернене значення
5. Тестувати

**До**:
```javascript
function alertForMiscreant(people) {
  for (const p of people) {
    if (p === "Don") {
      setOffAlarms();
      return "Don";
    }
    if (p === "John") {
      setOffAlarms();
      return "John";
    }
  }
  return "";
}
```

**Після**:
```javascript
function findMiscreant(people) {
  for (const p of people) {
    if (p === "Don") return "Don";
    if (p === "John") return "John";
  }
  return "";
}

function alertForMiscreant(people) {
  if (findMiscreant(people) !== "") setOffAlarms();
}
```

---

### Parameterize Function

**Коли використовувати**: Кілька функцій роблять схожі речі з різними значеннями

**Мотивація**: Видалити дублювання додаванням параметра.

**Механіка**:
1. Обрати одну функцію
2. Додати параметр для варійованого літерала
3. Змінити тіло для використання параметра
4. Тестувати
5. Змінити виклики на параметризовану версію
6. Видалити тепер невикористані функції

**До**:
```javascript
function tenPercentRaise(person) {
  person.salary = person.salary * 1.10;
}

function fivePercentRaise(person) {
  person.salary = person.salary * 1.05;
}
```

**Після**:
```javascript
function raise(person, factor) {
  person.salary = person.salary * (1 + factor);
}

// Використання
raise(person, 0.10);
raise(person, 0.05);
```

---

### Remove Flag Argument

**Коли використовувати**: Булевий параметр, що змінює поведінку функції

**Мотивація**: Зробити поведінку явною через окремі функції.

**Механіка**:
1. Створити явну функцію для кожного значення прапорця
2. Замінити кожен виклик відповідною новою функцією
3. Тестувати після кожної зміни
4. Видалити оригінальну функцію

**До**:
```javascript
function bookConcert(customer, isPremium) {
  if (isPremium) {
    // логіка преміум-бронювання
  } else {
    // логіка звичайного бронювання
  }
}

bookConcert(customer, true);
bookConcert(customer, false);
```

**Після**:
```javascript
function bookPremiumConcert(customer) {
  // логіка преміум-бронювання
}

function bookRegularConcert(customer) {
  // логіка звичайного бронювання
}

bookPremiumConcert(customer);
bookRegularConcert(customer);
```

---

## Робота з успадкуванням (Dealing with Inheritance)

### Pull Up Method

**Коли використовувати**: Той самий метод у кількох підкласах

**Мотивація**: Видалити дублювання в ієрархії класів.

**Механіка**:
1. Перевірити, що методи ідентичні
2. Перевірити, що сигнатури однакові
3. Створити новий метод у суперкласі
4. Скопіювати тіло з одного підкласу
5. Видалити один метод підкласу, тестувати
6. Видалити інші методи підкласів, тестувати кожен

---

### Push Down Method

**Коли використовувати**: Поведінка релевантна лише для підмножини підкласів

**Мотивація**: Розмістити метод там, де він використовується.

**Механіка**:
1. Скопіювати метод у кожен підклас, що його потребує
2. Видалити метод із суперкласу
3. Тестувати
4. Видалити з підкласів, що його не потребують
5. Тестувати

---

### Replace Subclass with Delegate

**Коли використовувати**: Успадкування використовується некоректно, потрібна більша гнучкість

**Мотивація**: Віддавати перевагу композиції над успадкуванням, коли доречно.

**Механіка**:
1. Створити порожній клас для делегата
2. Додати поле в клас-хост для утримання делегата
3. Створити конструктор для делегата, що викликається з хоста
4. Перемістити функціональність до делегата
5. Тестувати після кожного переміщення
6. Замінити успадкування делегуванням

---

## Extract Class

**Коли використовувати**: Великий клас з кількома відповідальностями

**Мотивація**: Розділити клас для збереження єдиної відповідальності.

**Механіка**:
1. Вирішити, як розділити відповідальності
2. Створити новий клас
3. Перемістити поле з оригіналу до нового класу
4. Тестувати
5. Перемістити методи з оригіналу до нового класу
6. Тестувати після кожного переміщення
7. Переглянути та перейменувати обидва класи
8. Вирішити, як відкрити новий клас

**До**:
```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }

  get telephoneNumber() {
    return `(${this._officeAreaCode}) ${this._officeNumber}`;
  }
}
```

**Після**:
```javascript
class Person {
  constructor() {
    this._telephoneNumber = new TelephoneNumber();
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return this._telephoneNumber.toString(); }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
}

class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
  toString() { return `(${this._areaCode}) ${this._number}`; }
}
```

---

## Короткий довідник: Запах → Рефакторинг

| Запах коду | Основний рефакторинг | Альтернатива |
|------------|---------------------|--------------|
| Long Method | Extract Method | Replace Temp with Query |
| Duplicate Code | Extract Method | Pull Up Method |
| Large Class | Extract Class | Extract Subclass |
| Long Parameter List | Introduce Parameter Object | Preserve Whole Object |
| Feature Envy | Move Method | Extract Method + Move |
| Data Clumps | Extract Class | Introduce Parameter Object |
| Primitive Obsession | Replace Primitive with Object | Replace Type Code |
| Switch Statements | Replace Conditional with Polymorphism | Replace Type Code |
| Temporary Field | Extract Class | Introduce Null Object |
| Message Chains | Hide Delegate | Extract Method |
| Middle Man | Remove Middle Man | Inline Method |
| Divergent Change | Extract Class | Split Phase |
| Shotgun Surgery | Move Method | Inline Class |
| Dead Code | Remove Dead Code | - |
| Speculative Generality | Collapse Hierarchy | Inline Class |

---

## Додаткове читання

- Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.)
- Онлайн-каталог: https://refactoring.com/catalog/
