---
layout: post
title: "Literal Arrays In Objective C"
og_image_url: ""
description: "Small research on arrays in my favorite ancient language, Objective C"
---

Making use of literals for arrays is extremely useful. Before literals, array creation would look something like this:
```wasm
NSArray *teams = [NSArray arrayWithObjects: @"Arsenal", @"Man Utd", @"Bayern", @"Red Star", nil];
```
However, if you use literals, this looks much clearer:
```wasm
NSArray *teams = @[@"Arsenal", @"Man Utd", @"Bayern", @"Red Star"];
```

But don't be fooled, there is more to it than just syntactic sugar. A common use would be to retrieve object at a certain index, which is also something that looks easy when you utilize literals.
Without literals, you would retrieve object at index as follows:
```wasm
NSString *team = [NSArray objectAtIndex:1];
```
With literals, however, it's a matter of doing the following:
```wasm
NSString *team = teams[1];
```

Now, a common problem when using array literals is that if any of the objects is nil, an exception will be thrown, since literal syntax is just a convenient way for creating array and adding all objects within the square brackets.
Let's say we have a few objects:
```wasm
id object1 = //;
id object2 = //;
id object3 = //;

NSArray *array1 = [NSArray arrayWithObjects: object1, object2, object3, nil];

NSArray *array2 = @[object1, object2, object3];
```

And let's say object1 and object3 are pointing to valid objects, but object2 is nil. Literal array (array2 in this example) will throw exception, while array1 will be created but will contain only object1. Sudden realization for me was figuring out that arrayWithObjects method looks through the arguments until it hits nil, which is object2 in this case, and it returns before adding object3.
Obviously, you would rather want an exception to be thrown instead of having false data, and debugging what could possibly go wrong. With literal syntax, bugs can be found more easily.
