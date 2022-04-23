# How to understand `enable_if_t<is_trivially_copy_constructible_v<T>> * = nullptr`?

## Q
In lecture 12 file optimized_copy.h line 27:
`template<typename T, enable_if_t<is_trivially_copy_constructible_v<T>> * = nullptr>`
I understand by stating `enable_if_t<is_trivially_copy_constructible_v<T>> * = nullptr` it is a partial specialization when T is trivially copy constructible. However the syntax is enigmatic. It seems like we are assigning nullptr to some type, how can it be used as a judgment whether T is trivially copy constructible?

Similar examples on cpppreference:
https://en.cppreference.com/w/cpp/types/enable_if

```
struct T {
    enum { int_t, float_t } type;
    template <typename Integer,
              std::enable_if_t<std::is_integral<Integer>::value, bool> = true
    >
    T(Integer) : type(int_t) {}
 
    template <typename Floating,
              std::enable_if_t<std::is_floating_point<Floating>::value, bool> = true
    >
    T(Floating) : type(float_t) {} // OK
};
```

## A by Professor Mike
It is a way to prevent instantiation of a template if T is not trivially copy constructible. If `b` is a constexpr bool, `enable_if_t<b>` is an alias for `enable_if<b>::type`. However `enable_if<false>` is something like an empty class, so `enable_it_t<false>::type` doesn't exist, so the template is not instantiated.

I agree that this technique, known as SFINAE, has an enigmatic syntax, which is what motivated concepts, so in C++20, you could say something like

`template<is_trivially_copy_constructible T>`
which is much better, but there's not yet an is_trivially_copy_constructible concept (and it's important to know the enable_if idiom because it's ubiquitous in existing C++ code).

## Key facts
1. `= nullptr` set the default type to `nullptr`, which hides the implementation details of the template.
2. One possible implementation of `enable_if`:
```
template<bool B, class T = void>  // case #1
struct enable_if {};
 
template<class T> // case #2
struct enable_if<true, T> { typedef T type; };

template< bool B, class T = void >
using enable_if_t = typename enable_if<B,T>::type;
```
`enable_if<false>` would match case #1, lead to empty struct with no `type` member;
`enable_if<true>` would match case #2 with `T` default to `void`:
`enable_if<true, void>` thus `enable_if_t<true, void>` is now `void`.