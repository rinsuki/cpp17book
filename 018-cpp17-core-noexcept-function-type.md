## 関数型としての例外指定

C++17では例外指定が関数型に組み込まれた。

例外指定とは`noexcept`のことだ。`noexcept`と`noexcept(true)`が指定された関数は例外を外に投げない。

C++14ではこの例外指定は型システムに入っていなかった。そのため、無例外指定の付いた関数へのポインター型は型システムで無例外を保証することができなかった。

~~~cpp
// C++14のコード
void f()
{
    throw 0 ; 
}

int main()
{
    // 無例外指定の付いたポインター
    void (*p)() noexcept = &f ;

    // 無例外指定があるにもかかわらず例外を投げる
    p() ;
}
~~~

C++17では例外指定が型システムに組み込まれた。例外指定のある関数型を例外指定のない関数へのポインター型に変換することはできる。逆はできない。

~~~cpp
// 型はvoid()
void f() { }
// 型はvoid() noexcept
void g() noexcept { }

// OK
// p1, &fは例外指定のない関数へのポインター型
void (*p1)() = &f ;
// OK
// 例外指定のある関数へのポインター型&gを例外指定のない関数へのポインター型p2
// に変換できる
void (*p2)() = &g ; // OK

// エラー
// 例外指定のない関数へのポインター型&fは例外指定のある関数へのポインター型p3
// に変換できない
void (*p3)() noexcept = &f ;

// OK
// p4, &fは例外指定のある関数へのポインター型
void (*p4)() noexcept = &f ;
~~~

機能テストマクロは`__cpp_noexcept_function_type`, 値は201510。
