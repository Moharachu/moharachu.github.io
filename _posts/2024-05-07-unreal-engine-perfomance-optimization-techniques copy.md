---
layout: post
title: "Unreal Engine Perfomance Optimization Techniques"
category: blog
tags: unreal perfomance
---

## Unreal Engine Perfomance Optimization Techniques

#### Intro

A lot of people doing same mistakes, when developing games on Unreal Engine especially with optimization, so i decided to gather the information, what i'll have with my experience while working on projects and share it with you.

First, it's really good to understand what is the `ms` before we going deep at threads, all pretty easy `ms = 1000/FPS`, for example: `30 FPS = 33.33ms`, `60 FPS = 16.67ms`, `90 FPS = 11.11ms`, `120 FPS = 8.33ms` and etc.

Next it's good to know what is

#### Preparation

#### First Steps

After we done with checklist, we can start understanding where is bottleneck: `CPU`, `GPU`, `Memory`

|                       Console Commands                       | Description                                                                            |
| :----------------------------------------------------------: | -------------------------------------------------------------------------------------- |
|                         `stat Unit`                          | Overall frame time as well as the game thread, rendering thread and GPU times.         |
|                         `stat UnitGraph`                     | Individual thread time.                                                                |
|                         `r.ScreenPercentage`                 | Showing FPS.                                                                           |
|                         `r.ScreenPercentage`                 | Change to check, if it's GPU bottleneck. *Decrease value to get info. Default - `100`* |
|                         `stat SceneUpdate`                   | Time taken to add, update and remove entities.                                         |
|                         `stat Memory`                        | Memory usage by varios subsystems.                                                     |
|                         `stat MathVerbose`                   | Shows performance information of math operations.                                      |
|               `clear()`                                      | Removes all elements from the heap.<br>(*capacity is set to DEFAULT_CAP* & Max-heap/Min-heap property is preserved.) |
|               `clear(bool is_max_heap)`               | Removes all elements from the heap.<br>(*capacity is set to DEFAULT_CAP* & You may change Max-heap/Min-heap property) |
|                           `size()`                           | Returns the number of elements in the heap.                  |
|                         `capacity()`                         | Returns the size of the storage space currently allocated for the heap. |
|                          `empty()`                           | Returns true if heap is empty (i.e. its size is `0`). |

#### Unreal Insights

```c++
FZenPackageResourceManager(const TCHAR* ManifestPath) : CompletionEvents(512)
{
    Trace
}
```
