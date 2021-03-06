# TS-Mapperize


![ts-mapperize-ci](https://github.com/witty-services/ts-mapperize/workflows/ts-mapperize-build/badge.svg)
[![Coverage Status](https://coveralls.io/repos/github/witty-services/ts-mapperize/badge.svg?branch=master)](https://coveralls.io/github/witty-services/ts-mapperize?branch=master)
[![npm version](https://badge.fury.io/js/%40witty-services%2Fts-mapperize.svg)](https://badge.fury.io/js/%40witty-services%2Fts-mapperize)
![GitHub](https://img.shields.io/github/license/witty-services/ts-mapperize)
![GitHub repo size](https://img.shields.io/github/repo-size/witty-services/ts-mapperize)
![GitHub last commit](https://img.shields.io/github/last-commit/witty-services/ts-mapperize)
![GitHub issues](https://img.shields.io/github/issues/witty-services/ts-mapperize)
![GitHub top language](https://img.shields.io/github/languages/top/witty-services/ts-mapperize)


# Get Started

## Install

```
npm install @witty-services/ts-mapperize
```

## Create simple mapper

```typescript
import {Mapper, MapperFn} from '@witty-services/ts-mapperize'

class A {
  a: string;
}

class B {
  b: string;
}


class MyMapper {

    @Mapper(() => B, [
      {target: 'b', source: 'a'}
    ])
    public mapAToB: MapperFn<A, B>;
  
}

const mapper: MyMapper = new MyMapper();

mapper.mapAToB(new A())
// should return B{ b: '...' }
```

## Working with array

### Define all mapping behavior

```typescript
import {ArrayMapper, ArrayMapperFn} from '@witty-services/ts-mapperize'

class A {
  a: string;
}

class B {
  b: string;
}


class MyMapper {

    @ArrayMapper(() => B, [
      {target: 'b', source: 'a'}
    ])
    public mapAToB: ArrayMapperFn<A, B>;
  
}

const mapper: MyMapper = new MyMapper();

mapper.mapAToB([new A()])
// should return [B{ b: '...' }]
```

### Reuse behavior from function

```typescript
import {Mapper, MapperFn, ArrayMapper, ArrayMapperFn} from '@witty-services/ts-mapperize'

class A {
  a: string;
}

class B {
  b: string;
}


class MyMapper {

    @Mapper(() => B, [
      {target: 'b', source: 'a'}
    ])
    public mapAToB: MapperFn<A, B>;

    @ArrayMapper('mapAToB')
    public mapAToBArray: ArrayMapperFn<A, B>;
  
}

const mapper: MyMapper = new MyMapper();

mapper.mapAToBArray([new A()])
// should return [B{ b: '...' }]
```

## API

### MapperParamContext

Argument | Type | Required | Description
---------|------|----------|------------
source | string, keyof\<Input> | false | select from the input, the value to be mapped
target | string, keyof\<Output> | false | select the destination of the value inside the output object
customTransformer | (() => new(...args: any[]) => CustomTransformer<any, any>) | false | use an existing [CustomTransformer](TODO) class
transform | (input: any) => any | false | custom function to map value from selected source to target
type | () => new(...args: any[]) => any | false | type of the child object
params | MapperParamContext<any, any>[] | false | list of mapping information for child object

## How to run Unit Tests

To run unit tests and generate coverage with Jest, run :

```
npm run test
```
