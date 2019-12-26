```ts
// 取得第一个元素
type TupleHead<T extends any[]> = T[0];

// 去除第一个元素
type TupleTail<T extends any[]> = ((...t: T) => void) extends (x: any, ...t: infer R) => void ? R : never;

// 取得最后一个元素
type TupleLast<T extends any[]> = T[TupleTail<T>['length']];

// 去除最后一个元素
type TupleInit<T extends any[]> = TypeAssert<Overwrite<TupleTail<T>, T>, any[]>;

// 从头部加入一个元素
type TupleUnshift<T extends any[], X> = ((x: X, ...t: T) => void) extends (...t: infer R) => void ? R : never;

// 从末尾加入一个元素
type TuplePush<T extends any[], X> = TypeAssert<Overwrite<TupleUnshift<T, any>, T & { [x: string]: X }>, any[]>;

// 连接两个元组
type TupleConcat<A extends any[], B extends any[]> = {
    1: A,
    0: TupleConcat<TuplePush<A, B[0]>, TupleTail<B>>
}[B extends [] ? 1 : 0];

// 用到的 helper 类型，简化代码和解决某些情况下的类型错误
type TypeAssert<T, A> = T extends A ? T : never;
type Overwrite<T, S extends any> = { [P in keyof T]: S[P] };
//https://zhuanlan.zhihu.com/p/38687656

//https://zhuanlan.zhihu.com/p/58704376


type TupleReverse<T extends any[], U extends any[] = []> = {
    0: TupleReverse<TupleTail<T>, TupleUnshift<U, TupleHead<T>>>
    1: U
}[T extends [] ? 1 : 0]

type TupleMapId<T extends any[], U = never> = {
    0: TupleMapId<TupleTail<T>,TupleHead<T>['id']|U>
    1: U
}[T extends [] ? 1 : 0]

```
