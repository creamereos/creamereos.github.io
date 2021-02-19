---  
layout: post
title: JS - if, else, else if, switch
subtitle:
categories: dev
tags: js
comments: true  
--- 

### if, else if, else

```javascript
if (condition1) {
  //  block of code to be executed if condition1 is true

} else if (condition2) {
  //  block of code to be executed if the condition1 is false and condition2 is true

} else {
  //  block of code to be executed if the condition1 is false and condition2 is false
}
```

### if else shortcut: conditional operator

- if else

```javascript
condition ? expression_1 : expression_2
```

Like the if statement, the condition is an expression that evaluates to true or false.

If the condition evaluates to true, the operator returns the value of the expression_1; otherwise, it returns the value of the expression_2.

```javascript
age > 18 ? (
    alert("OK, you can register."),
    redirectTo("register.html");
) : (
    stop = true,
    alert("Sorry, you are too young!")
);
```


### switch, break
select one of many code blocks to be executed.

If multiple cases matches a case value, the first case is selected.

If no matching cases are found, the program continues to the default label.

If no default label is found, the program continues to the statement(s) after the switch.

```javascript
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
     // - It is not necessary to 'break' the last case in a switch block. The block breaks (ends) there anyway. 

     // - If you omit the 'break' statement, the next case will be executed even if the evaluation does not match the case.
  default:
    // code block

    // - The 'default' keyword specifies the code to run if there is no case match.
}
```
The switch expression is evaluated once.

The value of the expression is compared with the values of each case.

If there is a match, the associated block of code is executed.

If there is no match, the default code block is executed.

- Example
The getDay() method returns the weekday as a number between 0 and 6.

(Sunday=0, Monday=1, Tuesday=2 ..)

```javascript
switch (new Date().getDay()) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
     day = "Tuesday";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
}
```

```javascript
switch (new Date().getDay()) {
  case 4:
  case 5:
    text = "Soon it is Weekend";
    break;
  case 0:
  case 6:
    text = "It is Weekend";
    break;
  default:
    text = "Looking forward to the Weekend";
}
```

- The 'default' case does not have to be the last case in a switch block:

 - If 'default' is not the last case in the switch block, remember to end the default case with a break.

 ```javascript
 switch (new Date().getDay()) {
  default:
    text = "Looking forward to the Weekend";
    break;
  case 6:
    text = "Today is Saturday";
    break;
  case 0:
    text = "Today is Sunday";
}
```