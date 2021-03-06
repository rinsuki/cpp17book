## using属性名前空間

C++17では、属性名前空間に`using`ディレクティブのような記述ができるようになった。

~~~c++

// [[extention::foo, extention::bar]]と同じ
[[ using extention : foo, bar ]] int x ;
~~~

属性トークンには、属性名前空間を付けることができる。これにより、独自拡張の属性トークンの名前の衝突を避けることができる。

たとえば、あるC++コンパイラーには独自拡張として`foo`, `bar`という属性トークンがあり、別のC++コンパイラーも同じく独自拡張として`foo`, `bar`という属性トークンを持っているが、それぞれ意味が違っている場合、コードの意味も違ってしまう。

~~~c++
[[ foo, bar ]] int x ;
~~~

このため、C++には属性名前空間という文法が用意されている。注意深いC++コンパイラーは独自拡張の属性トークンには属性名前空間を設定していることだろう。

~~~c++
[[ extention::foo, extention::bar ]] int x ;
~~~

問題は、これをいちいち記述するのは面倒だということだ。

C++17では、`using`属性名前空間という機能により、`using`ディレクティブのような名前空間の省略が可能になった。文法は`using`ディレクティブと似ていて、属性の中で`using name : ...`と書くことで、コロンに続く属性トークンに、属性名前空間`name`を付けたものと同じ効果が得られる。

