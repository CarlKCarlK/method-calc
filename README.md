# Understanding Rust Result and Option Methods

I made this table to help me understand converting between Option, Result, etc. Corrections and contributions welcome. It's on [GitHub](https://github.com/CarlKCarlK/method-calc/settings) and currently includes an Excel version and the very start of a WASM-based calculator.

Who knew that you could apply `.map` to a result?

| from | to | lambda to | Ok(t)/Some(t)/Some(Ok(t))/Ok(Some(t)) | None/Err(e)/Some(Err(e))/Ok(None) | na/na/None/Err(e) | method | side effect |
|---|---|---|---|---|---|---|---|
| Option<Result<T,E>> | Result<Option\<T>,E> |  | Ok(Some(t)) | Err(e) | Ok(None) | transpose() |  |
| Option\<T> | bool |  | false | true |  | is_none() |  |
| Option\<T> | bool |  | true | false |  | is_some() |  |
| Option\<T> | Option\<T> | T | Some(t) | Some(lazy_t) |  | or_else(\|\|lazy_t) |  |
| Option\<T> | Option\<T> |  | Some(t) | None |  | take() | sets original to None |
| Option\<T> | Option\<U> | Option\<U> | lambda(t) | None |  | and_then(\|t\| …) |  |
| Option\<T> | Option\<U> | U | Some(lambda(t)) | None |  | map(\|t\| …) |  |
| Option\<T> | T | T | t | lazy_t |  | unwrap_or_else(\|\| lazy_t) |  |
| Option\<T> | T |  | t | panic!(lazy message) |  | .unwrap_or_else(\|\| panic!("lazy message")) |  |
| Option\<T> | T |  | t | panic!("your message") |  | expect("your message") |  |
| Option\<T> | T |  | t | panic!("std message") |  | unwrap() |  |
| Option\<T> | T |  | t | t0 |  | unwrap_or(t0) |  |
| Option\<T> | T |  | t | T.default() |  | unwrap_or_default() |  |
| Option\<T> | U | U | lambda(t) | u0 |  | map_or(u0, \|t\| …) |  |
| Result<Option\<T>,E> | Option<Result<T,E>> |  | Some(Ok(t)) | None | Some(Err(e)) | transpose() |  |
| Result<T,E> | bool |  | false | true |  | is_err() |  |
| Result<T,E> | bool |  | true | false |  | is_ok() |  |
| Result<T,E> | E |  | panic!("your message") | e |  | expect_err |  |
| Result<T,E> | E |  | panic!("std message") | e |  | unwrap_err |  |
| Result<T,E> | Option\<T> |  | Option(error) | None |  | err() |  |
| Result<T,E> | Option\<T> |  | Some(t) | None |  | ok() |  |
| Result<T,E> | Option\<T> |  | Some(t) | None |  | take() | sets original to Ok(T::default) or leaves as e |
| Result<T,E> | Result<T,E> | T | Ok(t) | Ok(lazy_t) |  | or_else(\|\|lazy_t) |  |
| Result<T,E> | Result<T,E> | U | Ok(lambda(t)) | Err(e) |  | map(\|t\| …) |  |
| Result<T,E> | Result<U,E> | Result<U,E> | lambda(t) | Err(e) |  | and_then(\|t\|…) |  |
| Result<T,E> | T | T | t | lazy_t |  | unwrap_or_else(\|\| lazy_t) |  |
| Result<T,E> | T |  | t | panic!("your message") |  | expect("your message") |  |
| Result<T,E> | T |  | t | panic!("message with error description") |  | unwrap() |  |
| Result<T,E> | T |  | t | t0 |  | unwrap_or(t0) |  |
| Result<T,E> | T |  | t | T.default() |  | unwrap_or_default() |  |
| Result<T,E> | T |  | t | panic!("lazy message") |  | unwrap_or_else(\|\| panic!("lazy message")) |  |
| Result<T,E> | U | U | lambda(t) | u0 |  | map_or(u0, \|t\| …) |
