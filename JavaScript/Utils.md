
##一些常用的js判断方法

```javascript

import assign from 'object-assign';


function isObject(obj) {
  return Object.prototype.toString.call(obj) === '[Object Object]';
}

function isString(obj) {
  return Object.prototype.toString.call(obj) === '[Object String]';
}

function isFunc(obj) {
  return Object.prototype.toString.call(obj) === '[Object Function]';
}

export const isArray = obj => Object.prototype.toString.call(obj) === '[Object Array]';

// 判断对象是否为空
function isEmptyObject(obj) {
  if (typeof obj === 'undefined' || obj === null) {
    return true;
  }
  const keys = Object.keys(obj);
  if (keys.length > 0) {
    return false;
  }
  return true;
}

// 深克隆
const deepCopy = obj => JSON.parse(JSON.stringify(obj));
// 浅克隆
const copySingleObj = obj => {
  if (isArray(obj)) {
    return [].concat(obj);
  } else if (isObject(obj)) {
    return assign({}, obj);
  }
  return obj;
}


// 判断是否空数组

export const isEmptyArray = obj => {
  if (typeof obj !== 'undefined' && isArray(obj) && obj.length === 0) {
    return true;
  }
  return false;
}


```
