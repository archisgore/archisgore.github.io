---
layout: post
title: "Semantic versioning has never versioned Semantics!"
description: "If you changed the sort() implementation from MergeSort to QuickSort, do you up the major version?"
date: 2018-12-15 20:18:38 +0000
original_url: https://medium.com/@archisgore/semantic-versioning-has-never-versioned-semantics-ebe191e48acd
---

My rants against SemVer are famous. Here’s a meta-rant: Semantic Versioning is broken at a higher level than what it does. It doesn’t version semantics (meaning, behavior, effect, outcome), but rather versions binding signatures (basically any linking, loading, IDE-calling, etc. won’t error when binding.)

The semantic meaning of semantic versioning is itself gaslighting. At best, it should be called SigVer (Signature Versioning).

It doesn’t version “interfaces”, which may have a more concrete runtime contract expectation.

For example, consider the difference between the two:

```
// V1.0.0  
function sort(arr int*, len int) {  
   // Do sort here  
}
```

Now, suppose we realize people are passing in nil pointers, so we add a type-check.

```
// V1.0.0  
function sort(arr int*, len int) {  
    if (NULL == arr) {  
        // error, panic, change signature?
```

```
    }
```

```
   // Do sort here  
}
```

This is interface versioning. The agreed-upon contract is now changed. How is this communicated to a caller? Major version? Minor Version? Patch Version?

If you don’t think about this, then you could get away with patch version, because the **binding is compatible**.

The problem is far deeper than merely changing an interface contract. What happens when we change the one thing Semantic Versioning promises to version: Semantics?

What if in one version, the `function sort(arr struct*, len int, cf compareFunc*);` is implemented using a MergeSort, and in the later version, using a QuickSort? All sorts of unit tests and data justify this change — this is reliable, safe, and you can stand behind it.

For a moment ignore the debates around side-effects and performance. Look at this consumer code:

```
struct home {  
    city char*  
    state char*
```

```
}
```

```
function SortAlphaByState(homes (struct home *), len int) {
```

```
    // Let's sort by city first  
    sort(homes, len, compareByState)
```

```
    // Let's sort by state (preserving city-ordering)  
    sort(homes, len, compareByCity)
```

```
}
```

[MergeSort](https://en.wikipedia.org/wiki/Merge_sort) is a [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability). [QuickSort](https://en.wikipedia.org/wiki/Quicksort) is usually not.

The Semantics have changed. This has major downstream implications to business logic, airlines, space craft and all the mission-critical stuff we want to defend.

Do you bump up Major version? Minor Version? Patch Version?
