# TinyNdArray

Single Header C++ Implementation of NumPy NdArray.

I look forward to your pull-request.

## Requirement

* C++14 compiler

## Sample Code
<pre lang="cpp">
#define TINYNDARRAY_IMPLEMENTATION
#include "tinyndarray.h"

using tinyndarray::NdArray;

int main(int argc, char const* argv[]) {
    auto m1 = NdArray::Arange(12).reshape(2, 2, 3) - 2.f;
    auto m2 = NdArray::Ones(3, 1) * 10.f;
    auto m12 = m1.dot(m2);
    std::cout << m12 << std::endl;

    NdArray m3 = {{{-0.4f, 0.3f}, {-0.2f, 0.1f}},
                  {{-0.1f, 0.2f}, {-0.3f, 0.4f}}};
    m3 = Sin(std::move(m3));
    std::cout << m3 << std::endl;

    auto sum_abs = Abs(m3).sum();
    std::cout << sum_abs << std::endl;

    auto m4 = Where(0.f < m3, -100.f, 100.f);
    bool all_m4 = All(m4);
    bool any_m4 = Any(m4);
    std::cout << m4 << std::endl;
    std::cout << all_m4 << " " << any_m4 << std::endl;

    return 0;
}
</pre>

## Quick Guide

TinyNdArray supports only float array.

In the following Python code `dtype=float32` is omitted, and in C++ code assuming `using namespace tinyndarray;` is declared.

For more detail, please see declarations in top of the header file.

### Copy behavior

Copy behavior of NdArray is shallow copy which is same as NumPy.

<table>
<tr align="center">
    <td> <b>Numpy (Python)</b> </td>
    <td> <b>TinyNdArray (C++)</b> </td>
</tr>
<td><pre lang="python">
    a = np.ones((2, 3))
    b = a
    b[0, 0] = -1
    print(a[0, 0])  # -1
</pre></td>
<td><pre lang="cpp">
    auto a = NdArray::Ones(2, 3);
    auto b = a;
    b[{0, 0}] = -1;
    std::cout << a[{0, 0}] << std::endl;  // -1
</pre></td>
</tr>
</table>

### Basic Constructing

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a = np.array([2, 3])```                               | ```NdArray a({2, 3});``` or                              |
|                                                          | ```NdArray a(Shape{2, 3});```                            |

### Float Initializer

Supports up to 10 dimensions.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a = np.array([1.0, 2.0])```                           | ```NdArray a = {1.f, 2.f};```                            |
| ```a = np.array([[1.0, 2.0]])```                         | ```NdArray a = {{1.f, 2.f}};```                          |
| ```a = np.array([[1.0, 2.0], [3.0, 4.0]])```             | ```NdArray a = {{1.f, 2.f}, {3.f, 4.f}};```              |
| ```a = np.array([[[[[[[[[[1.0, 2.0]]]]]]]]]])```         | ```NdArray a = {{{{{{{{{{1.f, 2.f}}}}}}}}}};```          |

### Static Initializer

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a = np.empty((2, 3))```                               | ```auto a = NdArray::Empty(2, 3);``` or                  |
|                                                          | ```auto a = NdArray::Empty({2, 3});``` or                |
|                                                          | ```auto a = NdArray::Empty(Shape{2, 3});```              |
| ```a = np.zeros((2, 3))```                               | ```auto a = NdArray::Zeros(2, 3);``` or                  |
|                                                          | ```auto a = NdArray::Zeros({2, 3});``` or                |
|                                                          | ```auto a = NdArray::Zeros(Shape{2, 3});```              |
| ```a = np.ones((2, 3))```                                | ```auto a = NdArray::Ones(2, 3);``` or                   |
|                                                          | ```auto a = NdArray::Ones({2, 3});``` or                 |
|                                                          | ```auto a = NdArray::Ones(Shape{2, 3});```               |
| ```a = np.arange(10)```                                  | ```auto a = NdArray::Arange(10);```                      |
| ```a = np.arange(0, 100, 10)```                          | ```auto a = NdArray::Arange(0, 100, 10);```              |

### Random Initializer

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a = np.random.uniform(10)```                          | ```auto a = NdArray::Uniform(10);```                     |
| ```a = np.random.uniform(size=(2, 10))```                | ```auto a = NdArray::Uniform({2, 10});``` or             |
|                                                          | ```auto a = NdArray::Uniform(Shape{2, 10});```           |
| ```a = np.random.uniform(low=0.0, high=1.0, size=10)```  | ```auto a = NdArray::Uniform(0.0, 1.0, {10});``` or      |
|                                                          | ```auto a = NdArray::Uniform(0.0, 1.0, Shape{10});```    |
| ```a = np.random.normal(10)```                           | ```auto a = NdArray::Normal(10);```                      |
| ```a = np.random.normal(size=(2, 10))```                 | ```auto a = NdArray::Normal({2, 10});``` or              |
|                                                          | ```auto a = NdArray::Normal(Shape{2, 10});```            |
| ```a = np.random.normal(loc=0.0, scale=1.0, size=10)```  | ```auto a = NdArray::Normal(0.0, 1.0, {10}); ``` or      |
|                                                          | ```auto a = NdArray::Normal(0.0, 1.0, Shape{10});```     |

### Random Seed

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a = np.random.seed()```                               | ```auto a = NdArray::Seed();```                          |
| ```a = np.random.seed(0)```                              | ```auto a = NdArray::Seed(0);```                         |

### Basic Embeded Method

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```id(a)```                                              | ```a.id()```                                             |
| ```a.size```                                             | ```a.size()```                                           |
| ```a.shape```                                            | ```a.shape()```                                          |
| ```a.ndim```                                             | ```a.ndim()```                                           |
| ```a.fill(2.0)```                                        | ```a.fill(2.f)```                                        |
| ```a.copy()```                                           | ```a.copy()```                                           |

### Original Embeded Method

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```---```                                                | ```a.empty()```                                          |
| ```---```                                                | ```a.data()```                                           |
| ```---```                                                | ```a.begin()```                                          |
| ```---```                                                | ```a.end()```                                            |

### Single Element Casting

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```float(a)```                                           | ```static_cast<float>(a)```                              |

### Index Access

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a[2, -3]```                                           | ```a[{2, -3}]``` or                                      |
|                                                          | ```a[Index{2, -3}]``` or                                 |
|                                                          | ```a(2, -3)```                                           |

### Reshape methods

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a.reshape(-1, 2, 1)```                                | ```a.reshape({-1, 2, 1})``` or                           |
|                                                          | ```a.reshape(Shape{-1, 2, 1})``` or                      |
|                                                          | ```a.reshape(-1, 2, 1)```                                |
| ```a.flatten()```                                        | ```a.flatten()```                                        |
| ```a.ravel()```                                          | ```a.ravel()```                                          |

### Reshape functions

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.reshape(a, (-1, 2, 1))```                          | ```Reshape(a, {-1, 2, 1})```                             |
| ```np.squeeze(a)```                                      | ```Squeeze(a)```                                         |
| ```np.squeeze(a, [0, -2])```                             | ```Squeeze(a, {0, -2})```                                |
| ```np.expand_dims(a, 1)```                               | ```ExpandDims(a, 1)```                                   |

### Slice

Slice methods create copy of the array, not reference.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a[1:5, -4:-1]```                                      | ```a.slice({{1, 5}, {-4, -1}})``` or                     |
|                                                          | ```a.slice(SliceIndex{{1, 5}, {-4, -1}})``` or           |
|                                                          | ```a.slice({1, 5}, {-4, -1})```                          |

### Print

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```print(a)```                                           | ```std::cout << a << std::endl;```                       |

### Single Operators
| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```+np.ones((2, 3))```                                   | ```+NdArray::Ones(2, 3)```                               |
| ```-np.ones((2, 3))```                                   | ```-NdArray::Ones(2, 3)```                               |

### Arithmetic Operators

All operaters supports broadcast.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.ones((2, 1, 3)) + np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) + NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) - np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) - NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) * np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) * NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) / np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) / NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) + 2.0```                           | ```NdArray::Ones(2, 1, 3) + 2.f```                       |
| ```np.ones((2, 1, 3)) - 2.0```                           | ```NdArray::Ones(2, 1, 3) - 2.f```                       |
| ```np.ones((2, 1, 3)) * 2.0```                           | ```NdArray::Ones(2, 1, 3) * 2.f```                       |
| ```np.ones((2, 1, 3)) / 2.0```                           | ```NdArray::Ones(2, 1, 3) / 2.f```                       |
| ```2.0 + np.ones((2, 1, 3))```                           | ```2.f + NdArray::Ones(2, 1, 3)```                       |
| ```2.0 - np.ones((2, 1, 3))```                           | ```2.f - NdArray::Ones(2, 1, 3)```                       |
| ```2.0 * np.ones((2, 1, 3))```                           | ```2.f * NdArray::Ones(2, 1, 3)```                       |
| ```2.0 / np.ones((2, 1, 3))```                           | ```2.f / NdArray::Ones(2, 1, 3)```                       |

### Comparison Operators

All operaters supports broadcast.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.ones((2, 1, 3)) == np.ones((4, 1))```              | ```NdArray::Ones(2, 1, 3) == NdArray::Ones(4, 1)```      |
| ```np.ones((2, 1, 3)) != np.ones((4, 1))```              | ```NdArray::Ones(2, 1, 3) != NdArray::Ones(4, 1)```      |
| ```np.ones((2, 1, 3)) > np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) > NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) >= np.ones((4, 1))```              | ```NdArray::Ones(2, 1, 3) >= NdArray::Ones(4, 1)```      |
| ```np.ones((2, 1, 3)) < np.ones((4, 1))```               | ```NdArray::Ones(2, 1, 3) < NdArray::Ones(4, 1)```       |
| ```np.ones((2, 1, 3)) <= np.ones((4, 1))```              | ```NdArray::Ones(2, 1, 3) <= NdArray::Ones(4, 1)```      |
| ```np.ones((2, 1, 3)) == 1.f```                          | ```NdArray::Ones(2, 1, 3) == 1.f```                      |
| ```np.ones((2, 1, 3)) != 1.f```                          | ```NdArray::Ones(2, 1, 3) != 1.f```                      |
| ```np.ones((2, 1, 3)) > 1.f```                           | ```NdArray::Ones(2, 1, 3) > 1.f```                       |
| ```np.ones((2, 1, 3)) >= 1.f```                          | ```NdArray::Ones(2, 1, 3) >= 1.f```                      |
| ```np.ones((2, 1, 3)) < 1.f```                           | ```NdArray::Ones(2, 1, 3) < 1.f```                       |
| ```np.ones((2, 1, 3)) <= 1.f```                          | ```NdArray::Ones(2, 1, 3) <= 1.f```                      |
| ```1.f == np.ones((4, 1))```                             | ```1.f == NdArray::Ones(4, 1)```                         |
| ```1.f != np.ones((4, 1))```                             | ```1.f != NdArray::Ones(4, 1)```                         |
| ```1.f > np.ones((4, 1))```                              | ```1.f > NdArray::Ones(4, 1)```                          |
| ```1.f >= np.ones(4, 1))```                              | ```1.f >= NdArray::Ones(4, 1)```                         |
| ```1.f < np.ones((4, 1))```                              | ```1.f < NdArray::Ones(4, 1)```                          |
| ```1.f <= np.ones((4, 1))```                             | ```1.f <= NdArray::Ones(4, 1)```                         |

### Compound Assignment Operators

All operaters supports broadcast. However, left-side variable keep its size.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.ones((2, 1, 3)) += np.ones(3)```                   | ```NdArray::Ones(2, 1, 3) += NdArray::Ones(3)```         |
| ```np.ones((2, 1, 3)) -= np.ones(3)```                   | ```NdArray::Ones(2, 1, 3) -= NdArray::Ones(3)```         |
| ```np.ones((2, 1, 3)) *= np.ones(3)```                   | ```NdArray::Ones(2, 1, 3) *= NdArray::Ones(3)```         |
| ```np.ones((2, 1, 3)) /= np.ones(3)```                   | ```NdArray::Ones(2, 1, 3) /= NdArray::Ones(3)```         |
| ```np.ones((2, 1, 3)) += 2.f```                          | ```NdArray::Ones(2, 1, 3) += 2.f```                      |
| ```np.ones((2, 1, 3)) -= 2.f```                          | ```NdArray::Ones(2, 1, 3) -= 2.f```                      |
| ```np.ones((2, 1, 3)) *= 2.f```                          | ```NdArray::Ones(2, 1, 3) *= 2.f```                      |
| ```np.ones((2, 1, 3)) /= 2.f```                          | ```NdArray::Ones(2, 1, 3) /= 2.f```                      |

### Math Functions

Functions which takes two arguments support broadcast.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.abs(a)```                                          | ```Abs(a)```                                             |
| ```np.sign(a)```                                         | ```Sign(a)```                                            |
| ```np.ceil(a)```                                         | ```Ceil(a)```                                            |
| ```np.floor(a)```                                        | ```Floor(a)```                                           |
| ```np.clip(a, x_min, x_max)```                           | ```Clip(a, x_min, x_max)```                              |
| ```np.sqrt(a)```                                         | ```Sqrt(a)```                                            |
| ```np.exp(a)```                                          | ```Exp(a)```                                             |
| ```np.log(a)```                                          | ```Log(a)```                                             |
| ```np.square(a)```                                       | ```Square(a)```                                          |
| ```np.power(a, b)```                                     | ```Power(a, b)```                                        |
| ```np.power(a, 2.0)```                                   | ```Power(a, 2.f)```                                      |
| ```np.power(2.0, a)```                                   | ```Power(2.f, a)```                                      |
| ```np.sin(a)```                                          | ```Sin(a)```                                             |
| ```np.cos(a)```                                          | ```Cos(a)```                                             |
| ```np.tan(a)```                                          | ```Tan(a)```                                             |
| ```np.arcsin(a)```                                       | ```ArcSin(a)```                                          |
| ```np.arccos(a)```                                       | ```ArcCos(a)```                                          |
| ```np.arctan(a)```                                       | ```ArcTan(a)```                                          |
| ```np.arctan2(a, b)```                                   | ```ArcTan2(a, b)```                                      |
| ```np.arctan2(a, 10.0)```                                | ```ArcTan2(a, 10.f)```                                   |
| ```np.arctan2(10.0, a)```                                | ```ArcTan2(10.f, a)```                                   |

### Axis Functions

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.sum(a)```                                          | ```Sum(a)```                                             |
| ```np.sum(a, axis=0)```                                  | ```Sum(a, {0})``` or                                     |
|                                                          | ```Sum(a, Axis{0})```                                    |
| ```np.sum(a, axis=(0, 2))```                             | ```Sum(a, {0, 2})``` or                                  |
|                                                          | ```Sum(a, Axis{0, 2})```                                 |
| ```np.mean(a, axis=0)```                                 | ```Mean(a, {0})```                                       |
| ```np.min(a, axis=0)```                                  | ```Min(a, {0})```                                        |
| ```np.max(a, axis=0)```                                  | ```Max(a, {0})```                                        |

### Logistic Functions

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.all(a)```                                          | ```All(a, {0})```                                        |
| ```np.all(a, axis=0)```                                  | ```All(a, {0})```                                        |
| ```np.any(a)```                                          | ```Any(a, {0})```                                        |
| ```np.any(a, axis=0)```                                  | ```Any(a, {0})```                                        |
| ```np.where(condition, x, y)```                          | ```Where(condition, x, y)```                             |

### Axis Method

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```a.sum()```                                            | ```a.sum()```                                            |
| ```a.sum(axis=0)```                                      | ```a.sum({0})``` or                                      |
|                                                          | ```a.sum(Axis{0})```                                     |
| ```a.sum(axis=(0, 2))```                                 | ```a.sum({0, 2})``` or                                   |
|                                                          | ```a.sum(Axis{0, 2})```                                  |
| ```a.mean(axis=0)```                                     | ```a.mean({0})```                                        |
| ```a.min(axis=0)```                                      | ```a.min({0})```                                         |
| ```a.max(axis=0)```                                      | ```a.max({0})```                                         |

### Grouping Functions

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.stack((a, b, ...), axis=0)```                      | ```Stack({a, b, ...}, 0)```                              |
| ```np.concatenate((a, b, ...), axis=0)```                | ```Concatenate({a, b, ...}, 0)```                        |
| ```np.split(a, 2, axis=0)```                             | ```Split(a, 2, 0)```                                     |
| ```np.split(a, [1, 3], axis=0)```                        | ```Split(a, {1, 3}, 0)```                                |
|                                                          | ```Separate(a, 0)```  An inverse of Stack(a, 0)          |

### View Changing Functions

View chaining methods create copy of the array, not reference.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.transpose(x)```                                    | ```Transpose(x)```                                       |
| ```np.swapaxes(x, 0, 2)```                               | ```Swapaxes(x, 0, 2)```                                  |
| ```np.broadcast_to(x, (3, 2))```                         | ```BroadcastTo(x, {3, 2})```                             |
|                                                          | ```SumTo(x, {3, 2})```  An inverse of BroadcastTo(x, {3, 2}) |

### Matrix Products

All dimension rules of numpy are implemented.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.dot(a, b)```                                       | ```Dot(a, b)```                                          |
| ```a.dot(b)```                                           | ```a.dot(b)```                                           |
| ```np.matmul(a, b)```                                    | ```Matmul(a, b)```                                       |
| ```np.cross(a, b)```                                     | ```Cross(a, b)```                                        |
| ```a.cross(b)```                                         | ```a.cross(b)```                                         |

### Inverse

All dimension rules of numpy are implemented.

| **Numpy (Python)**                                       | **TinyNdArray (C++)**                                    |
|:--------------------------------------------------------:|:--------------------------------------------------------:|
| ```np.linalg.inv(a)```                                   | ```Inv(a, b)```                                          |


## In-place Operation

In NumPy, in-place and not inplace operations are written by following.

<table>
<tr align="center">
    <td> <b>Numpy (Python) In-place</b> </td>
    <td> <b>Numpy (Python) Not in-place</b> </td>
</tr>
<td><pre lang="python">
    a = np.ones((2, 3))
    a_id = id(a)
    np.exp(a, out=a)  # in-place
    print(id(a) == a_id)  # True
</pre></td>
<td><pre lang="python">
    a = np.ones((2, 3))
    a_id = id(a)
    a = np.exp(a)  # not in-place
    print(id(a) == a_id)  # False
</pre></td>
</tr>
</table>

In TinyNdArray, when right-reference values are passed, no new arrays are created and operated in-place.

<table>
<tr align="center">
    <td> <b>TinyNdArray (C++) In-place</b> </td>
    <td> <b>TinyNdArray (C++) Not in-place</b> </td>
</tr>
<td><pre lang="cpp">
    auto a = NdArray::Ones(2, 3);
    auto a_id = a.id();
    a = np.exp(std::move(a));  // in-place
    std::cout << (a.id() == a_id)
              << std::endl;  // true
</pre></td>
<td><pre lang="cpp">
    auto a = NdArray::Ones(2, 3);
    auto a_id = a.id();
    a = np.exp(a);  // not in-place
    std::cout << (a.id() == a_id)
              << std::endl;  // false
</pre></td>
</tr>
</table>

However, even right-reference values are passed, when the size is changed by broadcasting, a new array will be created.

<table>
<tr align="center">
    <td> <b>TinyNdArray (C++) In-place</b> </td>
    <td> <b>TinyNdArray (C++) Not in-place</b> </td>
</tr>
<td><pre lang="cpp">
   auto a = NdArray::Ones(2, 1, 3);
   auto b = NdArray::Ones(3);
   auto a_id = a.id();
   a = np.exp(std::move(a));  // in-place
   std::cout << a.shape()
             << std::endl;  // [2, 1, 3]
   std::cout << (a.id() == a_id)
             << std::endl;  // true
</pre></td>
<td><pre lang="cpp">
   auto a = NdArray::Ones(2, 1, 3);
   auto a = NdArray::Ones(3, 1);
   auto a_id = a.id();
   a = np.exp(srd::move(a));  // looks like in-place
   std::cout << a.shape()
             << std::endl;  // [2, 3, 3]
   std::cout << (a.id() == a_id)
             << std::endl;  // false
</pre></td>
</tr>
</table>

## Parallel Execusion

In default, most of all operations run in parallel by threads.

When changing the number of workers, please set via `NdArray::SetNumWorkers()`.

<pre lang="cpp">
// Default setting. Use all of cores.
NdArray::SetNumWorkers(-1);
// Set no parallel.
NdArray::SetNumWorkers(1);
// Use 4 cores
NdArray::SetNumWorkers(4);
</pre>

## Memory profiling

When a macro `TINYNDARRAY_PROFILE_MEMORY` is defined, memory profiler is activated.

The following methods can be used to get the number of instances and the size of allocated memories..

```NdArray::GetNumInstance()```
```NdArray::GetTotalMemory()```

## Macros

* TINYNDARRAY_H_ONCE
* TINYNDARRAY_NO_INCLUDE
* TINYNDARRAY_NO_NAMESPACE
* TINYNDARRAY_NO_DECLARATION
* TINYNDARRAY_IMPLEMENTATION
* TINYNDARRAY_PROFILE_MEMORY

## TODO

* [x] Replace axis reduction function with more effective algorithm.
   * [ ] Implement more effective algorithm.
* [x] Replace slice method's recursive call with loop for speed up.
   * [x] Make parallel
* [ ] Improve inverse function with LU decomposition.
* [ ] Implement reference slice which dose not effect the current performance.
* [ ] Introduce SIMD instructions.

Everything in the upper list are difficult challenges. If you have any ideas, please let me know.
