## mybatis-mapper ##

mybatis-mapper can generate SQL statements from the MyBatis3 Mapper XML file in node.js. You can also use several major Dynamic SQL elements, for example, &lt;if&gt;, &lt;where&gt;, &lt;foreach&gt;.


## Installation ##

TBD

## Usage ##

### 1) Basic ###

### 2) Parameters ( #{...}, ${...} ) ###

#### fruits.xml ####
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="fruit">  
  <select id="testParameters">
    SELECT
      name,
      category,
      price
    FROM
      fruits 
    WHERE
      category = #{category}
      AND price > ${price}
  </select>
</mapper>
```

#### fruits.js ####
```javascript
var mybatisMapper = require('../index');
mybatisMapper.createMapper([ './fruits.xml' ]);

var param = {
    category : 'apple'
    param : 100
}
    
var query = mybatisMapper.getStatement('fruit', 'testParameters', param);
console.log(query);
```

#### result SQL ####
```sql
    SELECT
      name,
      category,
      price
    FROM
      fruits 
    WHERE
      category = 'apple'
      AND price > 100
```

As in the example above, if a variable is enclosed in #{ }, the variable is wrapped in quotation marks.
The other side, if the variable is enclosed in ${ }, the variable is converted as it is.
In general, you can use #{ } for a String variable, and ${ } for a numeric value.

### 3) &lt;if&gt; element ###

#### fruits.xml ####
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="fruit">  
  <select id="testIf">
    SELECT
      name,
      category,
      price
    FROM
      fruits 
    WHERE
      1=1
      <if test="category != null and category !=''">
        AND category = #{category}
      </if>
      <if test="price != null and price !=''">
        AND price = ${price}
      </if>
      <if test="category == 'apple' and price >= 400">
        AND name = 'Fuji'
      </if>
  </select>
</mapper>
```

#### fruits.js ####
```javascript
var mybatisMapper = require('../index');

mybatisMapper.createMapper([ './fruits.xml' ]);
var param = {
    category : 'apple',
    price : 500
}

var query = mybatisMapper.getStatement('fruit', 'testIf', param);
console.log(query);
```

#### result SQL ####
```sql
    SELECT
      name,
      category,
      price
    FROM
      fruits
    WHERE
      1=1
        AND category = 'apple'
        AND price = 500
        AND name = 'Fuji'
```
