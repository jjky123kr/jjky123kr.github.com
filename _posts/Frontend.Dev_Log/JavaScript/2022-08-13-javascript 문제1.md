---
layout: post
related_posts:
  - /frontdev-log/javascript/javascript-11/
title:  Javascript  과제
categories: 
  - frontdev-log
  - javascript
---

# Javascript  과제

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
var lotto = [];
for (i = 0; i <= 6; i++) {
  var num = Math.floor(Math.random() * 45) + 1;
  if (lotto.indexOf(num) === -1) {
    lotto.push(num);
  } else {
    i--
  }
}
lotto.sort(function (a, b) {
  return a - b;
});

document.write(lotto);
</script>
</body>
</html>





