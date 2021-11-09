---
layout: post
title: less code (words frequency)
---

A blog that solves a programming puzzle (_words frequency_ kata) in three ways.
The goal is first to write a correct program, then at each solution
to reduce the total lines of the program.

## The kata

The problem has as an input a string with characters and as an output a map
that the key is a character and a value how many times this character appeared.

### input/output example

input: "aaabb"
output: {a: 3, b: 2} // in java map the keys are the a, b and the corresponding values 3, 2

### method signature

```java
Map<Character, Integer> computeFrequencies(String input)
```

## Solution 1 (no use of functional java)

```java
    public static Map<Character, Integer> computeFrequenciesV1(String input) {
        Map<Character, Integer> result = new HashMap<>();
        for (int index = 0 ; index < input.length(); index++) {
            char c = input.charAt(index);
            if (result.containsKey(c)) {
                Integer oldValue = result.get(c);
                result.put(c, ++oldValue);
            } else {
                result.put(c, 1);
            }
        }
        return result;
    }
```

solution explanation

1. Map instantiation (1 line)
2. classic for loop with index (1 line)
3. get the current character (1 line)
4. check if the character if it is contained in the map (with if statement) (1 line)
   if it is get the previous counter value and increase it
   (!! notice the ++ as prefix if you write it as suffix the counter will not increase the number)
   and put the new value in the map (2 lines)
5. else, put the new character with the value 1 (3 lines, else and curly brace included)
6. for termination (1 line)
7. finally, return map (1 line)

__solution total lines__: 11

## Solution 2 (with functional java)

```java
    public static Map<Character, Integer> computeFrequenciesV2(String input) {
        Map<Character, Integer> result = new HashMap<>();
        for (int index = 0 ; index < input.length(); index++) {
            char c = input.charAt(index);
            result.compute(c, (key, value) -> value == null ? value = 1 : ++value);
        }
        return result;
    }
```

solution explanation

1. Map instantiation (1 line)
2. classic for loop with index (1 line)
3. get the current character (1 line)
4. at this line compute method is used (magic happens here!).
   compute method has two arguments:
   The first is the key of the map.
   The second is a function (or BiFunction) with the key and the value.
   If there is a value that value increased and this is the new value for the key (character).
   Otherwise, the value (counter) is initialized to 1. (1 line!!)
   __1 line instead of 6 of the first solution__
5. curly brace (1 line)
6. finally, return map (1 line)

__solution total lines__: 6 from 11, not bad and still readable I think

## Solution 3 (only functional java)

```java
    public static Map<Character, Long> computeFrequenciesV3(String input) {
        return input.chars()
                .mapToObj(c -> (char) c)
                .collect(groupingBy(c -> c, Collectors.counting()));
    }
```

solution explanation

1. with chars method we get an IntStream
2. with mapToObj we map the int value to char
3. finally, by using the collect method with the groupingBy
   we get the map that we want!

__solution total lines__: 3, we have a winner!

groupingBy explanation:

- takes two arguments, the first specifies the key of group (e.g. c -> c).

for example (in our case): "aaabb"
the keys will be: a, b two groups in total

- the second is the action that is performed in the group.
  In the specific kata the action is to count (e.g. Collectors.counting())
  the elements of the group.

for example (in our case): "aaabb"
the group with "a" we count three elements,
while in the "b" group we count two element.

If you haven't specified an action you will had a map with two keys and values:

key: 'a'
value: ['a', 'a', 'a']

key: "b"
value: ['a', 'a']

the action operates in the lists (the reduction step).
