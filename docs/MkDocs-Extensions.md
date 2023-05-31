# MkDocs Extensions

## 内容标签栏

=== "Tab 1"
	Content 1

=== "Tab 2"
	Content 2

## 常用 section

!!! note
	note

!!! abstract
	abstract

!!! quote
	quote

## 代码块

* `linenums="1"`
* `hl_lines="2-5"`
* `// (1)`

``` py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

```py linenums="1" hl_lines="2-5"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

```{ .java .no-copy }
Optional<String> message = Optional.ofNullable(record.value()); // (1)
if (message.isPresent()) {
    System.out.println(message.get());
}
```

1. optional判空处理.
