# 
> **Item1 - 4 待补充**

**Item 5: Prefer auto to explicit type declarations**
虽然 `auto` 可能会产生非预期的类型推断，但仍然推荐使用，因为 `auto` 可以减少很多 *手动声名类型* 带来的问题。

**Item 6: Use the explicitly typed initializer idiom when auto deduces undesired types**
正如准则5所说，有时候使用 `auto` 进行类型推断可能会产生令人意外的结果，比如下例：

```cpp
    std::vector<bool> features(const Widget& w);
    
    Widget w;
    bool highPriority = features(w)[5];
    processWidget(w, highPriority);
```

明确声名类型是完全正确的，但是此时如果使用 `auto` 进行推断就有问题。


    auto highPriority = features(w)[5];

此时， `highPriority` 并不是 `bool` 类型，而是 `std::vector<bool>::reference` 类型。之所以会造成这种问题是因为 `std::vector<bool>` 需要用 one bit per bool 的紧凑形式来存储，但是 C++ 禁止对 bit 的引用，所以存在一个叫 `reference` 的内部类用来表示对 bool 的引用。

