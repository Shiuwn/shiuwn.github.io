---
title: "Type Challenge"
date: 2022-09-19T10:27:36+08:00
draft: false
categories: ["编程"]
tags: ["typescript"]
---
## 项目地址
[Type Challenge](https://github.com/type-challenges/type-challenges/blob/main/README.zh-CN.md)

## 实现Pick
```typescript
type MyPick<T, K extends keyof T> = {[k in K]: T[k]}
```
## 实现ReadOnly

### 目标 
```typescript
interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}

todo.title = "Hello" // Error: cannot reassign a readonly property
todo.description = "barFoo" // Error: cannot reassign a readonly property
```
### 实现

```typescript
type MyReadonly<T> = {readonly [k in keyof T]: T[k]}
```

## 数组转换为对象
### 目标
```typescript
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const

type result = TupleToObject<typeof tuple> // expected { tesla: 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
```

### 实现

```typescript
type TupleToObject<T extends any[]> = {[k in T[number]]: k}
```

## 第一个元素
### 目标
```typescript
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type head1 = First<arr1> // expected to be 'a'
type head2 = First<arr2> // expected to be 3
```

### 实现

```typescript
type First<T extends any[]> = T extends [infer first, ...infer rest]? first:never
```
## 获取元组长度
### 目标

```typescript
type tesla = ['tesla', 'model 3', 'model X', 'model Y']
type spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT']

type teslaLength = Length<tesla> // expected 4
type spaceXLength = Length<spaceX> // expected 5
```

### 实现

```typescript
type Length<T extends readonly unknown[]> = T['length']
```

## Exclude
### 目标
```typescript
type Result = MyExclude<'a' | 'b' | 'c', 'a'> // 'b' | 'c'
```
### 实现
```typescript
type MyExclude<T, U> = T extends U? never: T
```
### Awaited
### 目标
```typescript
type ExampleType = Promise<string>

type Result = MyAwaited<ExampleType> // string
```
### 实现
```typescript
type MyAwaited<T extends Promise<unknown>> = T extends Promise<infer K>?K extends Promise<unknown>?MyAwaited<K>:K:never
```

## If
### 目标
```typescript
type A = If<true, 'a', 'b'>  // expected to be 'a'
type B = If<false, 'a', 'b'> // expected to be 'b'
```
### 实现
```typescript
type If<C extends boolean, T, F> = C extends true? T:F
```
## Concat
### 目标
```typescript
type Result = Concat<[1], [2]> // expected to be [1, 2]
```
### 实现
```typescript
type Concat<T, U> = T extends any[]?U extends any[]?[...T, ...U]:never: never
```
## Includes
### 目标
```typescript
type isPillarMen = Includes<['Kars', 'Esidisi', 'Wamuu', 'Santana'], 'Dio'> // expected to be `false`
type hasNumber = Includes<[1 | 2], 1> // expected false
type hasBoolean = Includes<[boolean, 2, 3, 5, 6, 7], false> // expected false
```
### 实现
```typescript
type MyEqual<T, U> =
  (<G>() => G extends T ? 1 : 2) extends
  (<G>() => G extends U ? 1 : 2) ? true : false

type Includes<T extends unknown[], U> = T extends [infer First, ...infer Rest]? MyEqual<First,U> extends true ? true: Includes<Rest, U>: false 
```

## Push
### 目标
```typescript
type Result = Push<[1, 2], '3'> // [1, 2, '3']
```
### 实现
```typescript
type Push<T extends any[], U> = [...T, U]
```
## Unshift
### 目标
```typescript
type Result = Unshift<[1, 2], 0> // [0, 1, 2,]
```
### 实现
```typescript
type Unshift<T extends any[], U> = [U, ...T]
```
## Parameters
### 目标
```typescript
const foo = (arg1: string, arg2: number): void => {}

type FunctionParamsType = MyParameters<typeof foo> // [arg1: string, arg2: number]
```
### 实现
```typescript
type MyParameters<T extends (...args: any[]) => any> = T extends (... args: infer K) => any?K:never
```

## 获取函数返回类型
### 目标
```typescript
const fn = (v: boolean) => {
  if (v)
    return 1
  else
    return 2
}

type a = MyReturnType<typeof fn> // 应推导出 "1 | 2"
```
### 实现
```typescript
type MyReturnType<T extends (...args: any[]) => unknown> = T extends (...args: any[])=>infer K ? K : never
```
## Omit
### 目标
```typescript
interface Todo {
  title: string
  description: string
  completed: boolean
}

type TodoPreview = MyOmit<Todo, 'description' | 'title'>

const todo: TodoPreview = {
  completed: false,
}
```
### 实现
```typescript
type MyOmit<T, K extends keyof T> = {[k in keyof T as k extends K?never:k]: T[k]}
```





