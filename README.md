# Json Function

[![Build Status](https://travis-ci.com/aykutkardas/Json-Function.svg?branch=master)](https://travis-ci.com/aykutkardas/Json-Function)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)


## [Documentation](https://worn.gitbook.io/json-function/) • [Changelog](https://worn.gitbook.io/json-function/changelog)

## Install

```
npm install json-function
```

# Usage

## JsonFunction [• documentation](https://worn.gitbook.io/json-function/)

Chaining

```js
import JsonFunction from "json-function";

const result = JsonFunction
  .where({ completed: false })
  .select(["title", "completed"])
  .orderBy("title", "DESC")
  .limit(2)
  .get(data);
```

or Basic

```js
import JsonFunction from "json-function";

JsonFunction.where({ completed: false });
JsonFunction.select(["title", "completed"]);
JsonFunction.orderBy("title", "DESC");
JsonFunction.limit(2);
const result = JsonFunction.get(data);
```

or create a query and use it at any time.
```js
const queryTwoIncompleteTasks = JsonFunction
  .where({ completed: false })
  .select(["title", "completed"])
  .limit(2)
  .getQuery();
  
```

Query usage
```js
JsonFunction.setQuery(queryTwoIncompleteTasks).get(data);
// or
JsonFunction.get(data, { query: queryTwoIncompleteTasks });
```


# Methods

Instead of an entire "class", you can use only the methods you need.

## innerJoin [• documentation](https://worn.gitbook.io/json-function/functions/inner-join)


```js
import { innerJoin } from "json-function";

innerJoin(data, data2, "id", "userId");
```

## schema [• documentation](https://worn.gitbook.io/json-function/functions/schema)

```js
import { schema } from "json-function";

schema(data, {
  book: {
    id: "id",
    title: "title"
  },
  firstname: "user.firstname",
  lastname: "user.lastname"
});
```

Use "callback" for advanced conversions.

```js
schema(data, (sc) => ({
  id: "id",
  fullName: sc.join("user.firstname", "user.lastname")
}));
```

Custom separator

```js
schema(data, (sc) => ({
  id: "id",
  fullName: sc.join("user.firstname", "user.lastname", { separator: "_" })
}));
```

Use your own special function.
```js
schema(data, (sc) => ({
  id: "id",
  fullName: sc.custom(
    (firstname, lastname) => `${firstname.toUpperCase()} ${lastname.toUpperCase()}`,
    "user.firstname",
    "user.lastname"
  ),
}))
```

Example
```js
schema(data, (sc) => ({
  id: "id",
  createdAt: sc.custom(
    (createdAt) => moment(createdAt).format("DD/MM/YYYY"),
    "createdAt",
  ),
}))
```

## where [• documentation](https://worn.gitbook.io/json-function/functions/where)


```js
import { where } from "json-function";

// Single
// (completed === false)
where(data, { completed: false });

// Multiple (or)
// (completed === false || userId === 2)
where(data, [{ completed: false }, { userId: 2 }]);
```

Use "callback" for advanced filter.

```js
// id <= 3
where(data, (wh) => ({
  id: wh.lte(3),
}));
```

Other **wh** methods.
```js
wh.lte(3)        // value <= 3
wh.lt(3)         // value <  3
wh.gte(3)        // value >= 3
wh.gt(3)         // value >  3
wh.between(3,5)  // value >= 3 && value <= 5
wh.eq("3")       // value == 3
wh.ne("3")       // value != 3
wh.in("test")    // value.includes("test")
wh.nin("test")   // !value.includes("test")
```

## select [• documentation](https://worn.gitbook.io/json-function/functions/select)


```js
import { select } from "json-function";

// Single
select(data, "title");

// Multiple
select(data, ["title", "completed"]);
```

## limit [• documentation](https://worn.gitbook.io/json-function/functions/limit)


```js
import { limit } from "json-function";

// limit
limit(data, 2);

// limit and Start
limit(data, 2, 2);
```

## orderBy [• documentation](https://worn.gitbook.io/json-function/functions/order-by)


```js
import { orderBy } from "json-function";

orderBy(data, "title", "DESC");
```