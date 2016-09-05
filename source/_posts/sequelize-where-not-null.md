---
title: Sequelize 에서 IS NOT NULL 조건 주기
date: 2016-09-05 23:14:16
tags: Java Script, Node JS, Sequelize, MySQL, RDB
---

<p>IS NOT NULL 조건으로 쿼리를 날리는 방법을 알게돼 포스트를 한다.<br />Sequelize 에서 `COLUMN_NAME IS NOT NULL` 조건을 추가하기 위해서는 다음곽 같이 코드를 작성한다.</p>
```js
TableName
  .findAll({
    where : { column_name: { ne: null } } // ne : Not Equal
  })
  .then((rows) => {
    // TODO
  })
  .catch((err) => {
    // Error Handling
  });
```
<p>이 외에도, where 절의 조건으로 `gt(greater than)`, `lt(less than)`, `in(where in Array)`, `like` 등의 property 를 사용할 수 있다.</p>

