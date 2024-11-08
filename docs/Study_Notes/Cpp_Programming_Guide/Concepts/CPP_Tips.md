---
id: CPP_Tips
aliases:
  - Tips
tags: []
---

# Tips

## Braces {} for initialization

You can define a variable in 2 formats:

```cpp
double d1 = 2.3;double d2{4.9};
```

The `=` form is traditional and dates back to C, but if in doubt, use the general `{}`-list forms. It saves you from a conversation that loses information.

```cpp
int i1 = 7.8; // i1 becomes 7int i2 {7.8}; //error: floating point to integer conversion
```

**Using `{}` detects conversion and forces you to correct the mistakes. It is a good step to stop mistakes.**

---

## Single Quotes as digit separators

To make long literals more readable for humans, we can use a single quote (’) as a digit separator. For example, *π* is about

```cpp
3.14159'26535'89793'23846'26433'83279'50288
```

---

## std::for_each

Guide: [std::for_each](https://en.cppreference.com/w/cpp/algorithm/for_each)

**Syntax:**

```cpp
for (<data_type> <variable_name>:<list_name>)    {        //Use <variable_name> for work    }
```

Another option is to use `std::for_each().` It provides you with control over the indexes.

## Resources
