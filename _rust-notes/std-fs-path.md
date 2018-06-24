---
layout: note
title: "std::fs and std::path"
---

`std::fs` and `std::path` are essential when you want to do operations that involve manipulating files and directories. I used these when writing a test as a part of my PR for Diesel. Since functions in `std::fs` accept `AsRef<Path>` we have to wrap our file path as a `Path` first. The code below initializes a new `Path`.

```
use std::path::Path;

let my_path_to_file = Path::new("my_file.md");
```

Of course this is very different to how one might do it in Python. :D In Python we usually use the `os` library and we're free to pass our file path as a good 'ol string, like:

```
import os 

os.remove("my_file.md") 
```

