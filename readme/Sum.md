| Name | Avg exec time* | train set | classification parameters | mistakes, elements  |
|-----------|---------------|------------------------------------------|-------------------------------------------------------------------------|------------------------|
| kNN | ~35s | iris | distances, nn value | 5 |
| WkNN | ~45s | iris | distances, nn value w/ weight gradation | 6 |
| PW | ~50s | iris | distances, window width, kernel func | 6 |
| PF | *unstable* | iris | distances, potential width, kernel func, potential, mistakes threshold  | user-defined threshold |
| STOLP-kNN | ~70s | compressed iris (150 => 14)  - standards | distances, nn value  (set compressing parameters are not suitable here) | user-defined threshold |

