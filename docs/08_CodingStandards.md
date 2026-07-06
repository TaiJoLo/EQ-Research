# Coding Standards

## Data Model

Generic object name:

Level

Never:

ClassicLevel

GapLevel

BOLevel

---

## Function Naming

detectXXX()

createXXX()

updateXXX()

drawXXX()

Never mix responsibilities.

---

## Arrays

Store every Level in

allLevels

Never create one array for each Level Type.

---

## Constants

Always use named constants.

Never use magic numbers.

---

## Architecture

Detection

↓

Creation

↓

State Update

↓

Rendering

## Pine Script Safety Rules

Always check array size before iterating.  

Never call array.get() on an empty array.  

Never modify global variables inside functions.  

Every script must compile and execute without runtime errors.

## Data Model

Fields should be ordered by domain importance.  

1. Core Market Data
2. State
3. Runtime Objects

